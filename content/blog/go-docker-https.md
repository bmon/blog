---
author: "Brendan Roy"
date: 2018-11-22
title: Automagical HTTPS with Docker and Go
weight: 10
tags: ["go", "docker", "web"]
---

I've been building a lot of webhooks lately, and more often than not, I need serve my applications over HTTPS.
A common way of quickly achieving this is by utilising [Let's Encrypt](https://letsencrypt.org/), however it can be a bit fiddly to setup. I'd really like to be able to automate the process entirely, including certificate renewal.
I've been building my applications using docker, and I'd like to keep the build process and container images as lightweight as possible.
Lastly, I'd like to have the entire process as application code, so I can easily change and re-deploy things on the fly. Also, having no ops/shell scripts allows my applications to be portable and easily deployed to many different environments.

Fortunately, there is a way to achieve all of these things, using Go. It all starts with the [acme/autocert](https://godoc.org/golang.org/x/crypto/acme/autocert) package. For the unaware, ACME stands for _Automated Certificate Management Environment_, a protocol which allows for low cost and automated TLS certificate generation and verification.

Let's Encrypt utilises ACME to provide free domain-validated certificates to anybody who can verify that they control the domain they're requesting to be certified.
One way to verify is for an application to request a secret token from Let's Encrypt (over a secure connection), and then Let's Encrypt will make a HTTP request to the domain name being verified.
If the application can serve the secret token back to Let's Encrypt, it's verified that is has control of the domain, and Let's Encrypt will sign a certificate for use on the domain.

## Let's get cooking

The first thing you'll need is a domain name. Any domain/subdomain will do, as long as you can deploy your application to a service hosted by that domain. You can have more than one domain, for example, you could host and verify both `chat.example.com` and `www.example.com` from the same application.
For this example, I will be hosting my application at `kappa.serv.brendanr.net` Ideally your application will not be load balanced - while it's possible to achieve ACME verification on a load balanced domain, it's more complicated.

Next, you'll need a server which you can deploy your application to. Make sure to configure your DNS server to point your domain(s) to your server.

Last you'll need to make use of that [acme/autocert](https://godoc.org/golang.org/x/crypto/acme/autocert) package I mentioned earlier to request and respond to ACME challenges. There is a great [sample implementation](https://github.com/kjk/go-cookbook/blob/master/free-ssl-certificates/main.go) written by [Krzysztof Kowalczyk](https://blog.kowalczyk.info/) which you should check out, but I'm going to show you a cut down version to better explain how it works.

## Our Application

```go
func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("Hello world"))
	})

	certManager := autocert.Manager{
		Prompt:     autocert.AcceptTOS,
		Cache:      autocert.DirCache("cert-cache"),
		// Put your domain here:
		HostPolicy: autocert.HostWhitelist("kappa.serv.brendanr.net"),
	}

	server := &http.Server{
		Addr:    ":443",
		Handler: mux,
		TLSConfig: &tls.Config{
			GetCertificate: certManager.GetCertificate,
		},
	}

	go http.ListenAndServe(":80", certManager.HTTPHandler(nil))
	server.ListenAndServeTLS("", "")
}
```

Let's step through this bit by bit.
```go
mux := http.NewServeMux()
mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Hello world"))
})
```
Here, we're setting up a request handler for our index page. Once you've got this working, you should register handler functions for your application in a similar way.


```go
certManager := autocert.Manager{
    Prompt: autocert.AcceptTOS,
    Cache:  autocert.DirCache("cert-cache"),
    HostPolicy: autocert.HostWhitelist("kappa.serv.brendanr.net"),
}
```
This block is setting up some configuration for ACME. `Prompt: autocert.AcceptTOS` specifies that you accept the Let's Encrypt [Terms of Service](https://letsencrypt.org/repository/). The `Cache` field specifies if, and how, the autocert package should cache certificates. Let's Encrypt has [rate limits](https://letsencrypt.org/docs/rate-limits/) which limit how often you can request a certificate, so it's important to store certificates somewhere you can retrieve them later. Here we are specifying that the certificates should be stored in the `cert-cache` directory.
Lastly, the `HostPolicy` allows us to whitelist which domains we wish to request certificates for. Without this setting it's possible for attackers to exhaust your rate limit allocation and possibly stop you from generating the certificates you need, so it is important.


```go
server := &http.Server{
	Addr:    ":443",
	Handler: mux,
	TLSConfig: &tls.Config{
		GetCertificate: certManager.GetCertificate,
	},
}

go http.ListenAndServe(":80", certManager.HTTPHandler(nil))
server.ListenAndServeTLS("", "")
```

Lastly we configure and start both a HTTP and a HTTPS service. The HTTPS service will respond with the handler we wrote earlier.
It will also automatically obtain an HTTPS certificate (either from cache or Let's Encrypt) when answering an HTTPS request.
The HTTP service specifically exists to allow Let's Encrypt to make a request for the secret token I mentioned earlier.
It's possible to set up the HTTP server to redirect to HTTPS as well - check out the [sample from earlier](https://github.com/kjk/go-cookbook/blob/master/free-ssl-certificates/main.go) for this.

Build and deploy your application, and make a https request to it! It should take a few seconds on the first request, while the application is going through the ACME challenge process with Let's Encrypt, but your application should respond with a secure and trusted HTTPS page.

## You promised me Docker

There's a couple of gotchas you'll need to dodge when containerizing this application.  the full dockerfile. I am making use of modules, and this dockerfile is a modified version of Pierre Prinetti's wonderful [Go 1.11 web service Dockerfile](https://medium.com/@pierreprinetti/the-go-1-11-dockerfile-a3218319d191)

```dockerfile
# Accept the Go version for the image to be set as a build argument.
# Default to Go 1.11
ARG GO_VERSION=1.11

# First stage: build the executable.
FROM golang:${GO_VERSION}-alpine AS builder

# Git is required for fetching the dependencies.
RUN apk add --no-cache ca-certificates git

# Set the working directory outside $GOPATH to enable the support for modules.
WORKDIR /src

# Fetch dependencies first; they are less susceptible to change on every build
# and will therefore be cached for speeding up the next build
COPY ./go.mod ./go.sum ./
RUN go mod download

# Import the code from the context.
COPY ./ ./

# Build the executable to `/app`. Mark the build as statically linked.
RUN CGO_ENABLED=0 go build \
    -installsuffix 'static' \
    -o /app .

# Final stage: the running container.
FROM scratch AS final

# Import the compiled executable from the first stage.
COPY --from=builder /app /app
# Import the root ca-certificates (required for Let's Encrypt)
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Expose both 443 and 80 to our application
EXPOSE 443
EXPOSE 80

# Mount the certificate cache directory as a volume, so it remains even after
# we deploy a new version
VOLUME ["/cert-cache"]

# Run the compiled binary.
ENTRYPOINT ["/app"]
```

The dockerfile works in two stages, builder and final. This allows us to ship an extremely minimal final image - we can even use `scratch` (an empty layer) as our base. The above code and dockerfile builds a final image which is only 7 MB!

Here's what you need to pay attention to:

- __You have to install ca-certificates on the final image__, even if your application isn't making TLS requests. This is because all requests your application will make to Let's Encrypt will be HTTPS requests, so you'll need the root certificates.
- __You have to expose port 80__, even if you're not going to serve anything over HTTP. This is because the ACME challenge we are using is the HTTP challenge, which means Let's Encrypt needs to be able to make a HTTP connection to our application.
- __You should use a volume for your cache dir__, so your certificates are stored, even between deployments. If you don't do this you will likely hit Let's Encrypt rate limits.

## Wrapup

I hope this article helps save you some time and head scratching when setting up your next web service, or at the very least makes you interested in writing a little more Go ;)

The full code is available at https://github.com/bmon/go-web-base
