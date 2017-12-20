---
layout:  single
title:   "Optimizing Sessionless Sessions"
banner:  "Optimizing Sessionless Sessions"
excerpt: "Designing an scalable and secure authentication mechanism."
categories: web
header:
  overlay_image: /assets/img/code-full.jpg
  overlay_filter: 0.6
  caption: "Photo credit: [**Pexels**](https://www.pexels.com/)"
---


<!-- {::options toc_levels="1..2" /} -->
{% include toc title="Contents" %}


I'm currently developing an app as a side-project. This is my first time developing a full-fledged application that requires both a back-end server to store data (for which I've chosen [Flask](http://flask.pocoo.org/)) and a front-end client that users interact with (for which I've chosen [Qt](https://www.qt.io/). I've reached the stage at which I need to add proper support for "users," so different users will query different data from the server (like a Twitter feed is different depending on who you're logged in as). 

This post is a look into the research and design that I put into the authentication scheme for the app.


# Authentication Methods #
For authenticating and tracking users and their respective logins, there are many standard approaches:

  - HTTP's "basic authentication" support
  - Session management with cookies
  - OAuth 2.0, or other token-based authentication methods 
  - Token in HTTP headers (e.g. OAuth 2.0)
  - Additional query parameters

I'm only going to touch on the first two methods, and go into detail of the token-based authentication method I'll be using. 

# Session Cookies #
In general, sessions are used to store per-user information, such as the contents of your shopping cart, online documents, or other long-term data.
{: .notice--info}

This is probably the most wide-spread approach for most applications. In this implementation, the server issues a unique ID as a cookie when you make a request:

```http
    HTTP/1.1 200 OK
    Content-Type: text/html; charset=utf-8
    Content-Length: 84
    Set-Cookie: session=7dbbb1c5-89f4-4b0a-8196-0e91031042a8.uYGdzyNi03c_0pLtftdJNL733sE; Expires=Sat, 20-Jan-2018 12:52:37 GMT; HttpOnly; Path=/
    Date: Wed, 20 Dec 2017 12:52:37 GMT
```

Here, the cookie is `session=7dbbb1c5-89f4-...`, etc. If you include this data with any subsequent requests to the server, it will know that you are the same person as before. 


### Why They're No Good ###
The primary downside to this method is that now the server needs to remember the session cookies that it has distributed (i.e. for every user). This is an increase in necessary resources over time.

For my purposes, I wasn't concerned with this, as I don't expect my application to have that many users. The thing that _did_ concern me was the fact that Qt [doesn't have support](https://stackoverflow.com/a/15453429) for modifying cookies from QML JavaScript code... This would mean I'd need to (re)write all of my requests in C++, which I really don't feel like doing. This led to an interesting investigation into alternative authentication methods that didn't rely on session cookies!


# Token Authentication #
This investigation led me to research a _purely stateless_ method, which authenticates via a cryptographically-signed token.

**Terminology**  
Here, _stateless_ means that the server doesn't remember anything between requests. This is an improvement over [session cookies](#session-cookies) because the server doesn't need to track the cookie data. It comes with its own tradeoff, though, since the state needs to be "rebuilt" for every request instead of stored once.
{: .notice--info}

Upon logging in, the server responds with a unique token that can only be associated with that particular user. Before getting technical, assume that a user logs in like so:

    Username: george
    Password: hunter2

And the server responds with a unique token:

    Token: 2retnuhegroeg

This can _only_ be associated with this particular user.<sup><a href="#footnote-1">[1]</a></sup> Obviously you wouldn't use this as a token, since it clearly exposes the username and password if anyone else were to see it, but the point stands. Any request can include this value as a token and the server will know that this person is logged in. We can achieve the same effect in a secure way using cryptography.
{: #footnote-1-root}

### Message Authentication Codes ###
This section gets pretty technical, but I will try to make things simple for those not familiar with cryptography.
{: .notice--warning}

An [HMAC](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code), or hash-based message authentication code, is used to create a one-way association between a value with another unique value. One-way means that the unique value is _no longer associatable with the original value_. To create an HMAC, you start with the value you wish to convert and some secret value, then run them through a hashing algorithm.

> For example, using [this tool](https://www.freeformatter.com/hmac-generator.html), the HMAC of the value "george" with the secret key "secret" using the MD5 hashing algorithm is  
> `b94624b9274af8d0843d8dea6108577a`

This is something that will always be true, and the original value is completely indeterminable from the HMAC (well, aside from the fact that I just told you they were associated).<sup><a href="#footnote-2">[2]</a></sup>
{: #footnote-2-root}

From now on, we'll refer to this resulting unique, one-way HMAC value as the **token**.
{: .notice--info}

Now, how do we use these for sessions? We will create these tokens based on the login data! The user logs in as usual sending their username and password, and the server responds with the user ID and the token:<sup><a href="#footnote-3">[3]</a>,<a href="#footnote-4">[4]</a></sup>
{: #footnote-3-root}

$$
  t = \text{HMAC}_{s}\left(u \,\|\, p\right)
$$

Where $$ t $$ is the resulting token, $$ s $$ is the secret key that only the server knows, and $$ u, p $$ are the username and password, respectively.

**An Aside on User IDs**  
Whether using this authentication method or not, the user is stored in a database. Think of a database as a big table where each row is a user and the row number is the resulting user ID. In our running example, pretend that this is the user's row: `5 | "george" | "hunter2"`, so **the user ID is 5**.
{: .notice--info}

Then, with _every subsequent request_, we send the token and the user ID (instead of the login info) as part of our request using the `X-Auth-Token` header (which is just part of the HTTP request):

```http
X-Auth-Token: id=5;token=090b4d082bd7adc1b223086d1d9f8dc4
```

Both of these pieces of information are critical for the server to believe we are who we say we are. The token alone isn't enough; it's a one-way value, so it _only_ proves that we logged in successfully at some point in time. Hence, the user ID is also necessary so that the server also knows _who_ we are. Then, the server can then verify the token by calculating it themselves. They know the credentials associated with the ID, and they know the secret, so they can follow the same formula and make sure the tokens match.

Why is this secure? Because **users can't fake tokens** due to the fact that they don't know the server's secret.

# Conclusion #
With this method, we can have fully memory-less sessions. This improves scalability, and also let's us do some fancy cryptography. It comes with the price that we now have to query the database whenever we need something (like the user information), so we can't do any session caching, but it also comes with the benefit that, well, we don't have to do any session caching.

Most importantly, though, it means I don't need to rewrite my requests and callbacks in C++ in my Qt front-end.

If you want to use a robust, standardized solution, you can look into something like OAuth. For me, though, this simple solution is more than enough to provide authentication for my application.

-------------------------------------------------------------------------------

# Footnotes & References #
{: style="font-size: smaller;"}

<sup><a href="#footnote-1-root">[1]</a></sup>  Obviously this isn't strictly true. Consider `Username: georgeh` and `Password: unter2`; this combination would likewise result in the same token. It's just an analogy without getting mathy.
{: #footnote-1 style="font-size: medium;"}

<sup><a href="#footnote-2-root">[2]</a></sup>  For the value to be truly indeterminable, you need to use a cryptographically-secure hashing algorithm like SHA256 (rather than MD5, which is known to be breakable). Here, the resulting HMAC would've been too long and scary-looking, so I used MD5 for a simpler example.
{: #footnote-2 style="font-size: medium;"}

<sup><a href="#footnote-3-root">[3]</a></sup>  The $$ \| $$ operator here is concatenation, just slapping the two values together.
{: #footnote-3 style="font-size: medium;"}

<sup><a href="#footnote-3-root">[4]</a></sup>  You should _never_ store a password in plaintext, so the token should actually use the _hashed_ password $$ H(p) $$.
{: #footnote-4 style="font-size: medium;"}
