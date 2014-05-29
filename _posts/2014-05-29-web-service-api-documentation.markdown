---
layout: post
title: Generic API documenter for web-services
blog: true
---
For some time I have meant to fully document all the different APIs I'm involved in. It gives a more professional feel, it helps integrators and front-end developers and it actually makes it easier to spot inconsistencies in naming or configuration. 

Most of our software is written in 100% Java 7∕8. I was making an effort to find tools that could help me do this automatically, because nothing is worse than outdated and misleading documentation. And believe it or not, I do not like maintaining code if I do not need to, so the hunt for 
a documentation tool started.

In short (mainly because I'm too lazy to write all my reasoning), I found the documentation tools to fall into on or more of the following three categories:

* The produced documentation did not look very appealing

* The lack of support for important parts of information (I want at least security roles, HTTP methods, URL, and optionally Javadoc documentation or other information carved into the source code)

* Too much hassle to setup

* (Or last, not free/open source)

Now it sounds that I have (re-)invented the wheel and the best thing since sliced bread. I do not think I still believe this project fills a gap. 

The strategy I have used is to create an independent model using an APT processor in the form of <a href="http://github.com/ethlo/api-documenter">ethlo api-documenter</a>. This outputs the model as a JSON file.

This solution is easy to integrate and adds minimal time to the compilation time. It even extracts the JSON schema from the method parameters and return value. 

<p><a href="http://github.com/ethlo/api-ui">ethlo API-UI</a> project example rendering:</p>
<div>
<a style="float:left;" href="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png"/></a>

<a style="float:left;"href="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png"/></a>
</div>
