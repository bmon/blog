---
author: "Brendan Roy"
date: 2017-10-18
title: e-portfolio
weight: 10
---

e-Portfolio Part 2: Brendan Roy (z5017725)

## Development Experience Reflection

My time at Sentia has continued to be a great learning experience, and has been very enjoyable. Ive been able to continue to develop many of my skills and improve my technical understanding in several areas. In addition, Ive also had many opportunities to get experience and and improve my communication skills.

Ive been able to continue to improve my technical skills regarding setup and maintenance of application servers. When I was doing server setup for the FSHD app, I was able to complete the setup of the system without having to ask any co workers for assistance. I had to troubleshoot and resolve several issues along the way. To give a little context, this server would run both an API for a mobile app some of my coworkers were developing, and a web admin portal to create and manage content for the app.

When testing, I was able to only partially create content - the text would save, but the images were not saving and displaying on the app. The application logs were giving a very cryptic error for something that seemed unrelated, but after a bit of research I found that a bug in a different part of the app was causing the issue, and that it had not been noticed previously because the bug would not trigger in the staging environment, just the production one (which I was setting up). I was able to diagnose and resolve the issue entirely.

During this process I learned a lot about configuration tools and subtleties that I had previously not realised when working with someone else to setup these servers. Additionally I was able to learn a lot about diagnosing and debugging system issues, and how to resolve them.

In that same setup, I was able to research and implement an improvement to our server setup process. Usually, when we want to store files on an external service, we store a username and password. The problem is that its not ideal to store that information, and if it happened to leak, someone could access that system without us knowing. Fortunately the service provider we were using has an advanced configuration setup that allows you to configure access to a specific service, from a specific server. This meant we didnt have to store the username and password anymore, and the systems are also more secure as a result. I was able to implement and demonstrate this improvement to my team, and now it is a standard part of our system setup process.

This experience taught me a lot about the type of infrastructure people in my field use to deploy applications, and some of the advanced techniques people use to automate and improve some processes. I was able to demonstrate my improved communication skills when demoing the new system to my colleagues, and by communicating in a clear but succinct manner, I was able to quickly demonstrate that the approach Id taken was a clear improvement to our existing process. I was then able to explain the details of how to implement the improvement on future developments.

## STAR Statement

Situation:

A client had requested development of a very large task with a very short deadline. Part of the application I was working on built some large forms from database configured models, but the client wanted a lot of changes which were not possible with the current system, and it would have to be refactored. Additionally, the clients deadline was strict, as their mandating body required the changes to be made before a specific date.

Task:

I had to develop a solution for the client that would allow them to create these new types of questions, but development of that system wouldnt fast enough to meet the deadline.

Action:

I discussed in depth with the client, and drew out some detailed requirements. After clarifying exactly what needed to happen when, we realised that the change could be split into two parts, with only some of the changes required to be implemented before the deadline. In addition, those changes could be applied entirely by the client using the tools they had available. I broke down the large development into individual tasks, and discussed with my team as to how long we estimated each task to take, and why.

Result:

The client was able to implement the mandated changes without issue well before the deadline, and we were able to develop the rest of requested changes without cutting corners to meet a strict deadline. Overall the quality of the end product was high, and now the client is able to implement all of their required changes, and we will be able to extend the system easily should they require new questions to be implemented.

## Final Reflection

During my placement I have been able to learn substantially and improve many of my technical and communication skills. I have also had the chance to gain experience in emotional intelligence, and have demonstrated and perhaps improved my generic skills.

Technical Skills

Ive been given the opportunity to expand my technical knowledge dramatically. The two areas here that were most improved would be development and skill with the web framework Django, and experience and understanding of the servers we use and the Linux operating system they run on. By working among a team developing a custom application for a clients needs, Ive been able to experience and help design and solve many development tasks and problems. Ive now got a much better understanding of the ins and outs of django, and am confident I could develop similar web applications myself. By participating in several server/environment configurations my knowledge of web application servers and the systems they run on has vastly improved. Ive been given opportunities to do my own research and learning alongside my work placement, and have been able to gain extra understanding, and even improve an existing process.

Emotional Intelligence

By working closely with coworkers and clients, Ive been able to gain experience and improve my emotional intelligence. My project manager has guided me and given feedback as to areas he thought I could improve, and what he thought I did well. Specifically, he said often I am too blunt, and need to work to improve my communication such that I dont unintentionally put off co-workers and clients when talking with them about tasks or issues. As part of my goal was to improve my emotional intelligence, Ive been seeking out examples of discussion between people and clients in order to learn from them. Some of those examples were meetings between other developers and clients that I sat in on, however some of them were also external examples and resources that Ive been seeking out on my own time. I hope to continue to improve this area throughout my career.

Generic Skills

Throughout my placement I have been working on improving parts of my general skills, specifically my communication skills, and my analytical/problem solving skills. Ive had many opportunities to work on both, with communication via email and teleconferences part of my daily routine during placement, and approaching coworkers to help them solve issues they may have. By continually placing myself in these environments, I get to practice skills Ive learned and gain feedback on what I could improve. By exposing myself to lots of different types of problems and issues, especially in areas which I am not experienced with, Ive been able to improve my general reasoning and problem solving, and learn some important things about problem solving. For instance, when debugging unusual errors in an app/webpage, often its useful to briefly inspect the http requests being made to the application server, for things that you might not expect to be there, such as the cf-cache-status: HIT http header I saw when debugging an issue where someone's changes had been deployed but not reflected in the application. The issue was a middleman service was aggressively caching the pages due to a misconfiguration.As my placement has progressed, Ive found an increased passion for this area of development. Since starting placement, Ive contributed to several open source projects in my own time, to improve my knowledge but also demonstrate my commitment and attitude to development.



Overall, Ive found this placement to be highly valuable, and hope to continue to work here for a while. Id like to continue to learn, and not get comfortable in a role. Part of what drives me to development is the opportunity to constantly be learning and expanding my knowledge base, (as software development is a highly volatile industry). I really value that aspect of work, because it both keeps me interested, and is highly valuable.
