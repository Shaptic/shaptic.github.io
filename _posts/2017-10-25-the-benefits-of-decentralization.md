---
layout:   single
banner:   "The Benefits of Decentralization"
excerpt:  "An analysis of peer-to-peer technologies and how they can cut costs
           and scale Internet services at the same time."

categories: networking
header:
  overlay_image: /assets/img/internet-header.jpg
  overlay_filter: 0.7
---

# The Internet of Today #
The invention of the Internet has undoubtedly changed the world. It has given us all access to an endless library of instant information, the ability to communicate across the world in microseconds, and opened the door to a digital Renaissance.

{% include toc icon="globe" title="Quick Links" %}

As the number of people using the Internet grows, so do the demands on the infrastructure. This is largely because of **centralization**: the Internet has grown to have a lot of direct user-to-provider communication. This creates a host of problems -- everything is centralized around a single point of failure (if Spotify goes down, so does your music), scaling is hard as resource demands grow proportionately with the number of users, and bandwidth costs skyrocket during peak usage times.

**Some Terminology**  
A **server** stores and provides data, like music, to anyone who asks for it. Think Tumblr, Spotify, etc. A **client** is just a user that _asks_ the server for some data and then does something with it (like plays a song).
{: .notice--info}

## How Do Providers Scale Content Distrbution to Meet Demand? ##
![Centralized View]({{ "assets/img/ncdn-sized.png" | absolute_url }}){: .align-right}

When a new season of House of Cards drops on Netflix, everyone rushes to watch it. What does this mean for Netflix's infrastructure? They need to have more servers come up to handle the influx of people watching the episodes and higher Internet bandwidth to provide the episode contents to the viewers. Essentially, a single Netflix server provides hundreds of copies of the same data to an influx of viewers all watching the same thing. The wasted cost of such a system is astronomically out of proportion. When another person decides to watch, Netflix will give them another copy.

![CDN View]({{ "assets/img/cdn-sized.png" | absolute_url }}){: .align-left}

Of course, this naive method has plenty of improvements. The introduction of [content delivery networks](https://en.wikipedia.org/wiki/Content_delivery_network) (CDNs) like Netflix's [OpenConnect](https://openconnect.netflix.com/en/) are an attempt to mitigatie this duplication of work by putting the content closer to the users requesting it. Despite that, this is just a band-aid applied to a fundamentally flawed content delivery system -- it's like stopping a bleeding wound by throwing tissues at it. We need something better. Something more scalable. Something _decentralized_.

# Content Decentralization #
If you and five of your neighbors are all watching the same episode on Netflix, why don't you all just distribute it amongst yourselves? Obviously, Netflix has to provide the original, but if one neighbor has already downloaded the first 5 seconds, why don't they share it with the other neighbors directly? Then, Netflix only has to provide the clip _once_, not five times.

![Peer to Peer]({{ "assets/img/p2p.png" | absolute_url }}){: .align-right}

As the number of viewers grows, everyone has more means to access pieces of the files; no _one_ provider has to bear the load of providing the episode to an ever-growing viewer count (as would be the case in the current Netflix model). Scale this out to Netflix's [52 million users](https://www.statista.com/statistics/250934/quarterly-number-of- netflix-streaming-subscribers-worldwide/), and you get a distribution network that simply cannot be matched by the existing client-server model. We can generalize this technology to many forms of content delivery, letting companies scale their services better, save money on resources, and interconnecting us all that much more.

With each new user, things get _more_ efficient, not less. Spotify actually adopted this model while they were still scaling worldwide and had **[35%](https://www.slideshare.net/ricardovice/spotify-p2p-music-streaming) of its traffic** offloaded to its users<sup><a href="#footnote-1">[1]</a></sup>. These benefits can **not** be ignored by designers of scalable systems.
{: #footnote-1-root}

# Conclusion #
The Internet of today hinges on services being available all the time, and "all the time" relies on big names like Amazon to provide the services. When [Amazon S3 went down on the East Coast](https://aws.amazon.com/message/41926/), a _massive_ chunk of the Internet was offline and inaccessible. These single points of failure are not only dangerous, but they are also an inefficient use of our infrastructure. With a decentralized, peer-to-peer model, we can eliminate waste while simultaneously improving user experiences.

# Footnotes #
{: style="font-size: smaller;"}
<sup><a href="#footnote-1-root">[1]</a></sup>  You can read the <a href="http://www.csc.kth.se/~gkreitz//spotify-p2p10/spotify-p2p10.pdf">technical paper</a> here.
{: #footnote-1 style="font-size: medium;"}

