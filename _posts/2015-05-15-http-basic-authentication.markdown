---
layout:     post
title:      "Basic Access Authentication"
subtitle:   "How to secure access to your Kingdom"
date:       2015-05-15 12:00:00
author:     "Suraj"
header-img: "img/747.jpg"
---

<p>Motivation: Why we need them?</p>
<ul>
	<li>In order to make a HTTP transation secure, we need some basic authentication (BA) mechanism</li>
	<li>These basic authentication methods, allows a HTTP user agent to provide a user name and password while making a HTTP request
	</li>
</ul>
<p>Pros:</p>
<ul>
	<li>No cookies required</li>
	<li>No session identifiers or login pages are required</li>
	<li>Since, BA uses static, standard HTTP headers, we need not to perform any handshakes in anticipation</li>
</ul>
<p>Cons:</p>
<ul>
	<li>BA provides no confidentiality</li>
	<li>They are simply encoded and not hashed or encrypted. Therfore, they should always be used over HTTPS</li>
	<li>Since BA Headers need to be used in each request, so a client caching and invalidation mechanism need to be constructed</li>
</ul>