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

Example Java code:

{% highlight java%}

@Transactional
@Endpoint
@RequestMapping("/basepath")
@Api(group="public")
public class ExampleEndpoint
{
    public static final String NS = "urn://foo/bar";

    /**
     * Allow the reading of an item by id
     * @param id The id of the item
     * @return The item
     */
    @Secured("ROLE_READER")
    @RequestMapping(method=RequestMethod.GET, value="/items/{id}")
    @PayloadRoot(localPart = "GetItemRequest", namespace = NS)
    public @ResponsePayload String read(@RequestPayload int id)
    {
        ...
    }

    @Secured("ROLE_WRITER")
    @RequestMapping(method=RequestMethod.PUT, value="/items/{id}") 
    public String write(int id, String content) throws FileNotFoundException, IOException
    {
        ...
    }

    @Api(group="hidden")
    @Secured("ROLE_ADMIN")
    @RequestMapping(method=RequestMethod.PUT, value="/admin/stats") 
    public @ResponseBody @ResponsePayload ResponseObject stats(@RequestPayload @RequestBody RequestObject req)
    {
        ...
    }
}

{% highlight %}

The produces JSON file:

{% highlight javascript%}

{
  "com.ethlo.ws.ExampleEndpoint" : {
    "packageName" : "com.ethlo.ws",
    "className" : "ExampleEndpoint",
    "annotations" : {
      "org.springframework.ws.server.endpoint.annotation.Endpoint" : { },
      "org.springframework.web.bind.annotation.RequestMapping" : {
        "properties" : {
          "value" : [ "/basepath" ]
        }
      },
      "com.ethlo.api.annotations.Api" : {
        "properties" : {
          "group" : [ "public" ]
        }
      }
    },
    "methods" : [ {
      "methodName" : "read",
      "annotations" : {
        "org.springframework.security.access.annotation.Secured" : {
          "properties" : {
            "value" : [ "ROLE_READER" ]
          }
        },
        "org.springframework.web.bind.annotation.RequestMapping" : {
          "properties" : {
            "method" : [ "GET" ],
            "value" : [ "/items/{id}" ]
          }
        },
        "org.springframework.ws.server.endpoint.annotation.PayloadRoot" : {
          "properties" : {
            "localPart" : "GetItemRequest",
            "namespace" : "urn://foo/bar"
          }
        },
        "org.springframework.ws.server.endpoint.annotation.ResponsePayload" : { }
      },
      "params" : [ {
        "name" : "id",
        "type" : {
          "type" : "int"
        },
        "annotations" : [ { } ]
      } ],
      "returnType" : {
        "type" : "java.lang.String"
      },
      "javadoc" : " Allow the reading of an item by id\n @param id The id of the item\n @return The item\n"
    }, {
      "methodName" : "stats",
      "annotations" : {
        "com.ethlo.api.annotations.Api" : {
          "properties" : {
            "group" : [ "private" ]
          }
        },
        "org.springframework.security.access.annotation.Secured" : {
          "properties" : {
            "value" : [ "ROLE_ADMIN" ]
          }
        },
        "org.springframework.web.bind.annotation.RequestMapping" : {
          "properties" : {
            "method" : [ "PUT" ],
            "value" : [ "/admin/stats" ]
          }
        },
        "org.springframework.web.bind.annotation.ResponseBody" : { },
        "org.springframework.ws.server.endpoint.annotation.ResponsePayload" : { }
      },
      "params" : [ {
        "name" : "req",
        "type" : {
          "type" : "com.ethlo.ws.RequestObject"
        },
        "annotations" : [ { }, { } ]
      } ],
      "returnType" : {
        "type" : "com.ethlo.ws.ResponseObject"
      }
    }, {
      "methodName" : "write",
      "annotations" : {
        "org.springframework.security.access.annotation.Secured" : {
          "properties" : {
            "value" : [ "ROLE_WRITER" ]
          }
        },
        "org.springframework.web.bind.annotation.RequestMapping" : {
          "properties" : {
            "method" : [ "PUT" ],
            "value" : [ "/items/{id}" ]
          }
        }
      },
      "params" : [ {
        "name" : "id",
        "type" : {
          "type" : "int"
        }
      }, {
        "name" : "content",
        "type" : {
          "type" : "java.lang.String"
        }
      } ],
      "declaredExceptions" : [ {
        "type" : "java.io.FileNotFoundException"
      }, {
        "type" : "java.io.IOException"
      } ],
      "returnType" : {
        "type" : "java.lang.String"
      }
    } ]
  }
}

{% highlight %}

Then I use an Angular application to render it. Simple to integrate and and adds minimal time to the compilation time. 

<p><a href="http://github.com/ethlo/api-ui">ethlo API-UI</a> project example rendering:</p>
<div>
<a style="float:left;" href="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119659/dcf01cf6-e748-11e3-8637-433cd72abb49.png"/></a>

<a style="float:left;"href="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png" target="_blank"><img width="960" height="540" src="https://cloud.githubusercontent.com/assets/1384689/3119660/dcf04ea6-e748-11e3-80d7-6c1e3a7ee4ba.png"/></a>
</div>
