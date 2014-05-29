---
layout: post
title: Generic API documenter for web-services
blog: true
---
For some time I have meant to _fully_ document the API for our core SaaS/PaaS product at Kezzler AS. Most of our software is written in 100% Java 7∕8. I was making an effort to find tools that could 
help me do this automatically, because nothing is worse than outdated and misleading documentation. And beleive it or not, I do not like maintaining code if I do not need to, so the hunt for 
a documentation tool started.

In short (mainly because I'm too lazy to write all my reasoning), I found the documentation tools to fall into on or more of the following three categories:

* The produced doumentation did not look very appealing

* The lack of support for important parts of information (I want at least security roles, HTTP methods, URL, and optionally Javadoc documentation or other information carved into the source code)

* Too much hazzle to setup

* (Or last, not free/open source)

Now it sounds that I have (re-)invented the wheel and the best thing since sliced bread. I do not think I still believe this project fills a gap. 

The strategy I've used is to create an independent model using an APT processor which outputs the model as JSON. Then I use an Angular application to render it. Simple to integrate and and adds minimal time to the compilation time. 

<a href="http://github.com/ethlo/api-ui">ethlo API-UI</a> project example rendering:
<div>
<a href="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png"/></a>

<a href="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png"/></a>
</div>
