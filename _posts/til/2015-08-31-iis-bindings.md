---
layout: post
title:  "TIL#4 - IIS bindings"
date:   2015-08-31 19:45:08
categories: TIL
---

Many of us are familiar with IIS’s site bindings screen:

![Binding]({{ site.url }}/img/iis_bindings/iisbinding.png)

These bindings are the mechanism by which IIS decides which site should handle an incoming request. A binding is a combination of:

* Protocol (like http or https)
* Host name (like maps.google.com, not the same thing as domain name which is google.com. There's a good explanation [here](https://www.mojoportal.com/adding-a-host-name-to-the-hosts-file-for-local-testing)
* IP address (because a machine can have multiple IP addresses)

Whenever IIS sees an incoming request it goes through all the bindings to figure out which website it should send the request to. This means that bindings have to be *unique*. Uniqueness can be achieved by varying host name, port, ip address, etc. In my experience, I have typically seen the port number being varied.

## Interesting notes

* 	You can set up any number of bindings to point to a single website. This means that you can make the same website available over multiple ports: ![Multiple ports]({{ site.url }}/img/iis_bindings/multiport.png)
	So, for me, localhost and localhost:9080 both lead to this: ![Multiple ports]({{ site.url }}/img/iis_bindings/iis7welcome.png)

* 	If no hostname is specified then all hostnames lead to the same website. That means if you have DNS setup with wildcards then you can do something like: http://worst.site.ever.domain.tld where domain.tld is your domain name.
	
	Clearly its a very good idea to specify the host name for production websites.