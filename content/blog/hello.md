---
author: "Brendan Roy"
date: 2017-07-23
title: Hello World!
weight: 10
tags: ["foo", "bar"]
---

This is a temporary post I can use to test the frameworks I'm using for this blog. Posts are rendered into html from markdown using [Hugo](https://gohugo.io).

# H1
## H2
### H3
#### H4
##### H5
###### H6

Alternatively, for H1 and H2, an underline-ish style:

Alt-H1
======

Alt-H2

Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

1. First ordered list item
2. Another item
* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
1. Ordered sub-list
4. And another item.

You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

To have a line break without a paragraph, you will need to use two trailing spaces.
Note that this line is separate, but within the same paragraph.
(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

Here's our logo (hover to see the title text):

Inline-style:
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style:
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.

Three or more...

---

Hyphens

***

Asterisks

___

Underscores

### Filler text
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean tellus nisi, fringilla non orci nec, pharetra egestas mauris. Nunc mattis erat eu tortor convallis ultrices. Maecenas mauris quam, sodales in erat eu, bibendum blandit tortor. Vivamus vitae diam at mi ultrices placerat. Proin eu facilisis dui. Etiam leo eros, varius et augue lobortis, vulputate rhoncus nunc. Proin a dolor eget nulla pulvinar consequat. Morbi molestie convallis ipsum, non mollis velit facilisis at. Etiam neque ex, placerat et aliquet quis, tristique vel felis. Sed a mi sit amet lectus cursus dictum vel vel magna. Proin ac euismod mauris, scelerisque euismod dolor. Aenean commodo leo eget lectus pellentesque, ac bibendum sapien tincidunt. Sed id tortor vulputate, porta libero sed, condimentum massa. Fusce at orci ipsum.

```bash
# this is a comment
$ echo this is a command
this is a command

# edit the file
$vi foo.md
+++
date = "2014-09-28"
title = "creating a new theme"
+++

# show it
$ cat foo.md
+++
date = "2014-09-28"
title = "creating a new theme"
+++
```

## More filler
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean tellus nisi, fringilla non orci nec, pharetra egestas mauris. Nunc mattis erat eu tortor convallis ultrices. Maecenas mauris quam, sodales in erat eu, bibendum blandit tortor. Vivamus vitae diam at mi ultrices placerat. Proin eu facilisis dui. Etiam leo eros, varius et augue lobortis, vulputate rhoncus nunc. Proin a dolor eget nulla pulvinar consequat. Morbi molestie convallis ipsum, non mollis velit facilisis at. Etiam neque ex, placerat et aliquet quis, tristique vel felis. Sed a mi sit amet lectus cursus dictum vel vel magna. Proin ac euismod mauris, scelerisque euismod dolor. Aenean commodo leo eget lectus pellentesque, ac bibendum sapien tincidunt. Sed id tortor vulputate, porta libero sed, condimentum massa. Fusce at orci ipsum.

```golang
// Go provides built-in support for [base64
// encoding/decoding](http://en.wikipedia.org/wiki/Base64).

package main

// This syntax imports the `encoding/base64` package with
// the `b64` name instead of the default `base64`. It'll
// save us some space below.
import b64 "encoding/base64"
import "fmt"

func main() {

    // Here's the `string` we'll encode/decode.
    data := "abc123!?$*&()'-=@~"

    // Go supports both standard and URL-compatible
    // base64. Here's how to encode using the standard
    // encoder. The encoder requires a `[]byte` so we
    // cast our `string` to that type.
    sEnc := b64.StdEncoding.EncodeToString([]byte(data))
    fmt.Println(sEnc)

    // Decoding may return an error, which you can check
    // if you don't already know the input to be
    // well-formed.
    sDec, _ := b64.StdEncoding.DecodeString(sEnc)
    fmt.Println(string(sDec))
    fmt.Println()

    // This encodes/decodes using a URL-compatible base64
    // format.
    uEnc := b64.URLEncoding.EncodeToString([]byte(data))
    fmt.Println(uEnc)
    uDec, _ := b64.URLEncoding.DecodeString(uEnc)
    fmt.Println(string(uDec))
}
```

