---
layout:     post
title:      Prototype in Javascript and its pollution.
date:       2020-09-09 
summary:    Hi, Everybody this post try to explain Prototypes in javascript.
categories: javascript
---

### What even is prototype ? 
Prototype in Javascript is a mechanism by which different objects inherit properties of another.

Let see it by example - 

Lets create an empty object and try to see if it contains any inherited method by default (_secret: Yes it does_).

![altText](/images/PJAP-1.jpg) 


### Prototype Pollution

Now, suppose there is an web-application which uses this code

{% highlight js %}
	secureObject={};
	InsecureObject={};
	userControlledInput = "" // allowed characters are (\_,A-Z,a-z,0-9)
	eval(`InsecureObject.${userControlledInput}`)

	if(secureObject.admin == true){
		pwned() // Application hacked successfully
	}
{% endhighlight %}



Until you knew about prototype pollution this code looks unexploitable, but it is vulnerable.


Let's See How

As we can see both `secureObject` and `InsecureObject` are instances of same parent and as we knew we can set a parent property from child with the help of __proto__ and automatically every instance of the Object will inherit the property so now applying this knowledge here we can develop a payload of this sort -

payload --> `__proto__.admin=true`