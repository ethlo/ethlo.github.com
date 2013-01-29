---
layout: post
title: Spring DefaultKeyGenerator gotcha
blog: true
---
To speed things up I had applied the excellent [@Cacheable](http://static.springsource.org/spring/docs/3.2.x/javadoc-api/org/springframework/cache/annotation/Cacheable.html) annotation to a service method in our application (don't miss the excellent performance analysis [here](http://nurkiewicz.blogspot.com/2013/01/cacheable-overhead-in-spring.html), btw). However I had not paid too much attention to the parameters going into the cache key. I would have expected something like concatinated `toString()` or something similar, however the default is to use the `hashCode()` of each parameter and create another (32-bit) hash code. Obviously this can easily generate collisions as we shall soon see.

The service method in my service class took two parameters, a long and an int:
{% highlight java%}
@Cacheable
getData(long a, int b)
{% endhighlight %}

The test failed intermittently with an assertion error that the data returned was wrong, which baffled me, until I had a look at the [DefaultKeyGenerator](http://static.springsource.org/spring/docs/3.2.x/javadoc-api/org/springframework/cache/interceptor/DefaultKeyGenerator.html) source code:

{% highlight java %}
public Object generate(Object target, Method method, Object... params) {
	if (params.length == 1) {
		return (params[0] == null ? NULL_PARAM_KEY : params[0]);
	}
	if (params.length == 0) {
		return NO_PARAM_KEY;
	}
	int hashCode = 17;
	for (Object object : params) {
		hashCode = 31 * hashCode + (object == null ? NULL_PARAM_KEY : object.hashCode());
	}
	return Integer.valueOf(hashCode);
}
{% endhighlight %}

Truly simple input can be used to expose the problem:

{% highlight java %}
System.out.println(new DefaultKeyGenerator().generate(null, null, 1, 0)); // 16368
System.out.println(new DefaultKeyGenerator().generate(null, null, 0, 31)); // 16368
{% endhighlight %}

Looking at the documentation of `DefaultKeyGenerator`, there is a mention of using the arguments's hash codes, however I feel the risks should be better communicated. Too not jsut sound like a grumpy old fart, I [notified the Spring team](https://jira.springsource.org/browse/SPR-10237).

