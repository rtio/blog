+++
title = "From the beginning until today, the PHP infrastructure story."
slug = "from-the-beginning-until-today-the-php-infrastructure-story"
date = "2019-11-26"
author = "rtio"
cover = "infrastructure.jpg"
tags = ["php", "serverless"]
keywords = ["lambda", "php", "serverless", "aws", "infrastructure"]
description = "We can start thinking about why some programming languages pass through the decades and some others, no? Why don't major programming languages like Fortran keep on the top 10 languages these days? If you take a look at the TIOBE index, you can see the top 10 at the '90s until 2019; most of those aren't present today."
showFullContent = false
+++

I'm going to tell you a brief story of several PHP infrastructure scenarios and their evolution in the past years. As the main focus, I'm going to present some of the many infrastructure scenarios passed on the PHP life, since on-premise servers until our current infrastructure like Kubernetes, and serverless technologies. All based on many stories heard in the past years.

# Thinking outside the box

We can start thinking about why some programming languages pass through the decades and some others, no? Why don't major programming languages like Fortran or COBOL keep on the top 10 languages these days? If you take a look at the TIOBE index, you can see the top 10 at the '90s until 2019; most of those aren't present today. But for example, Java, keeps present, why? This chart shows the top 10 most used programming languages in the last 20 years:

![TIOBE index chart](/tiobe_index_last_20_years.png)

I believe one of those reasons for this technology change is because some languages aren't designed to run in our current environments. After thinking a lot, I figure out that the infrastructure says which languages keep in use and which becomes obsolete.

# Virtues of PHP

Said that I go to start talking about PHP and their scenario through the years. PHP was created in 94's during the web developing start. I won't deep dig into the technical detail about PHP's creation. Still, I need to talk about web servers and why PHP is so well designed for them. 

Shared-nothing architecture that used to be laughed about - nowadays it's obviously known as a significant advantage - PHP can scale smoothly with no overhead.

Each web request runs in a single PHP thread. It seems at first, like a limitation. Since your program executes in a web server context, we have a natural approach of concurrency available: web requests. Asynchronously curl' ing to the server providing a shared-nothing, copy-in/copy-out way of exploiting parallelism. In practice, this is safer and more resilient to errors than the locks-and-shared-state approach that most other general-purpose languages provide.

# At the beginning

**On-premise servers** - Everybody knows, PHP had its dark days, and I love to start talking about this important moment to the language. The internet was becoming popular in parallel PHP growth side-by-side. PHP originally meant Personal Home Page. 

It was first released in 95s' by Rasmus Lerdorf. It has been used for far more complicated projects than its creators anticipated, and its reputations cames out as a dirk language. Each deployment was made by hand, and nobody knows which way to follow. Nobody's land.

**XAMPP** - I don't need to waste time talking about this one, which is just a toolbox that means X (Server), A (Apache), M (MySQL/MariaDB), (P) Pearl, and (P) PHP — used to create a fast and straightforward DEVELOPMENT environment. I know, that you already had used it to production environments, and shame you! 

# Teenagerhood

**Shared servers** - At this time, we start using version control, like final_version_3.zip and uses the old but gold S/FTP to deploy our sites. We didn't care a lot about security and performance, after all, they were very cheap, and our bosses approved.

**VPS** - Here we start thinking about deploy strategies, SSH, and security. At least, we had more control here.

# Youngerhood

**IaaS** - Infrastructure as a Service. In mid-2010, becoming popular, IaaS start to be part of our lives. AWS, Azure, Google Cloud, these giants provide many services to make our day better, but of course, not cheap at all.

**Docker** - In weak words, Docker is a tool that wraps our application as a virtual machine does, but the main difference is, it shares the host machine resources. Docker runs with minimal programs and local resources, just what it needs for its proposals, such as webserver, and developer environment. If you need more details about what Docker means you can take a look [here](https://blog.usejournal.com/what-is-docker-in-simple-english-a24e8136b90b).

With that evolution, our necessities growth up, for example, we start to do more complex deploy strategies, roll-backs, and we also need more resilient applications. Seeing that, the companies launch services like Kubernetes, and AWS ECS to orchestrate our many services and scenarios that we have. Unsatisfied with this, we also need configuration-management tools to keep our "real" machines provisioned automatically when our application needs. Is it quite hard to maintain, huh?

**Serverless** - To make it more clear, let start talking about function as a service. A Lambda function takes an event and does whatever it once and then returns a result. Pretty cool, huh?

So, what is an event? It can be an HTTP request, a message, a cron, and many things at Amazon, and Google cloud does. Let's keep our focus on HTTP requests. 

![Lambda infrastructure](/lambda.png)

How do we pay for it? Let me show you a simple calculation to you know how you are going to bill. On request 300ms = $0.0000002 + (3 X $0.000000208). 

How about scalability? How does it run? How does it scale? In a simple explanation, we can talk like this: when an event happens, Amazon takes your code and puts it on something like a container (actually isn't a container but works similarly), and them the container process the event, and then it dies. Each execution is stateless. That means each container handles each request that it has, it creates "infinite scalability".  Don't kill your database!

There are some providers that kind of service, like AWS (Not support PHP out-of-the-box), IBM OpenWhisk (actually it runs the code on containers), Azure (There is a kind of support), Google Cloud (doesn't support). The best recommendation is using AWS because they are the leading provider at the moment. 

November last year, Amazon open their runtime code API because they don't want to maintain other languages on AWS Lambda. They opened the system to other people "to adapt" Lambda to run other languages on AWS Lambda. 

**Lambda layers** - that we can create availability for custom runtimes, like PHP. Probably you don't want to know the technical details about it. So, let's start to talk about **Bref**, the idea behind that it creates everything you need to run PHP on AWS Lambda and create serverless applications. 

There are 3 ways to run PHP on Lambda with Bref, as PHP function, as HTTP application, and as a console application. With the first one, you can run a function as JavaScript does, and Python does. In the second one, you can run an application that receives a request HTTP to make some process and return a response. And the third one you can run console applications, like Symfony console or Laravel artisan's commands because Lambda doesn't have ssh to access the application directly.                 

**Cold starts** - Not always a bed of roses. During execution, the same function can be reutilized many times, but after these calls, it dies, and the next request the container has to be started and takes more or less 300 ms just to start the container, and it is the cold starts. There are ways to "keep warm", don't worry about it.

I'm not pretending to cover all the details, if you are interested in Lambda, take a look at this [video](https://www.youtube.com/watch?v=4xdFQfOJgYI), the Bref's creator explains that much better then me. Also, that a look at the Bref's [website](https://bref.sh/).

# What lays ahead?

PHP still on the cutting edge. It becomes pretty clear to follow its evolution. Each day we have a new tool, language available to choose, but by my viewpoint, pick PHP to a new project is a good thing in the next years. 
 
Take care!