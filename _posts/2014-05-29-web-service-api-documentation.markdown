---
layout: post
title: Generic API documenter for web-services
blog: true
---
For some time I have meant to fully document all the different APIs I'm involved in. It gives a more professional feel, it helps integrators and front-end developers and it actually makes it easier to spot inconsistencies in naming or configuration. 

Most of our software is written in 100% Java 7 or 8. I was making an effort to find tools that could help me do this automatically, because nothing is worse than outdated and misleading documentation. And believe it or not, I do not like maintaining projects and code if I do not need to, so the hunt for a documentation tool started.

In short (mainly because I'm too lazy to write all my reasoning), I found the documentation tools to fall into on or more of the following three categories:

* The produced documentation did not look very appealing

* The lack of support for important parts of information (I want at least security roles, HTTP methods, URL, and optionally JavaDoc documentation or other information carved into the source code)

* Too much hassle to setup or the tool trying to be an integral part of my software.

* (Or last, not free/open source)

Now it sounds that I have (re-)invented the wheel and the best thing since sliced bread, and it may very well not be, however I believe this project fills a gap. 

The strategy I have used is to create an independent model using an <a href="https://www.jcp.org/en/jsr/detail?id=269">APT</a> processor in the form of <a href="http://github.com/ethlo/api-documenter">ethlo api-documenter</a>. This outputs the model as a JSON file.

Example
-------

<script src="https://gist.github.com/ethlo/5e4ac22c83cdf98f265a.js"></script>

<script src="https://gist.github.com/ethlo/ad2738910b2895ff05f5.js"></script>

* <a href="http://ethlo.com/demo/api-documenter">See the api-documenter/api-ui demo here!</a>

* <a href="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png">Screenshot #1</a>

* <a href="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png">Screenshot #2</a>

References
----------

* <a href="http://github.com/ethlo/api-documenter">ethlo API Documenter</a>

* <a href="http://github.com/ethlo/api-ui">ethlo API UI</a>
