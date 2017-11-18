---
layout: single
title:  "How Joe Shmoe and I Can Track Your Every Move Through Your Devices"
excerpt: "Or, abusing MAC addresses for fun and profit (well, no profit unless
    you're a massive corporation or the government...)."
categories: privacy
header:
  overlay_image: /assets/img/coffeeshop-header-sized.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Pexels**](https://www.pexels.com/)"
---

{% include toc title="Quick Links" %}

If you don't want to read this post at all: [here's the
tl;dr](https://en.wikipedia.org/wiki/Mac_address#Spying). This post dives into
the details of how this is possible, and what the average person can do to make
it less possible.

# The Internet in 2017 #
It's hard to imagine a world without our devices. Anything with an electrical
current running through it these days has Internet capabilities: our phones,
media players, laptops, cameras, thermometers, refrigerators, toasters, ... you
get the drift.

This proliferation of technology provides us with a whirlwind of convenience,
accessibility, customization, and more. But, it comes at a very severe cost
that isn't obvious on the surface: **privacy**. Now I'm not talking about your
always-on GPS constantly transmitting exact coordinates (because disabling
location means you can't use Snapchat filters), [full device
information](https://panopticlick.eff.org/) being available to any website,
[location and other
metadata](https://en.wikipedia.org/wiki/Exif#Privacy_and_security) in any
online photo uploads, ["shadow profiles" created by social
networks](https://www.dailydot.com/news/facebook-shadow-profiles-privacy-faq/)
to track those of you that don't even _use_ them, [tracking
cookies](https://en.wikipedia.org/wiki/Tracking_cookies#Tracking) that are
["only"](http://i3.kym-cdn.com/photos/images/newsfeed/000/594/192/596.gif)
used for targeted advertisement, or any of the other privacy concerns that have
risen in the recent data-mining craze.

I'm talking about a _universal_ method to track a person's location regardless
of the type of device they use, what software they have installed, or what
websites they visit. I'm talking about **open WiFi networks.**

## Who Doesn't Love Free WiFi? ##
When was the last time you checked your email on your phone or caught up on
some studying on your laptop at a coffee shop while waiting for your double-fat
whole soy milk upside-down whatever using their free WiFi? Well, your device
remembered that WiFi network so that you can join it automatically the next
time your there. To do this, it actually actively searches for it any time you
aren't connected to WiFi.

If it's done intelligently, the device will identify the network by what's
called a [MAC address](https://en.wikipedia.org/wiki/Mac_address) (**M**edia
**A**ccess **C**ontrol address). This is a value that _uniquely identifies any
WiFi-enabled device_ -- in this case, the router in the coffee shop. So this
value coupled with the WiFi network name will be automatically joined if your
device picks it up. If it's not that smart, it'll just try any WiFi network
that matches the name<sup><a href="#footnote-1">[1]</a></sup>.
{: #footnote-1-root}

In addition to this automatic search & connect, most mobile devices also try
out _any_ open WiFi networks to try to get Internet access and save that
precious data plan.

### The Implications of a Universally Unique Identifier ###
**Warning**: This section is a bit more technical, but will highlight the
implications of this device-specific, unique ID<sup><a
href="#footnote-2">[2]</a></sup> known as the MAC address.
{: .notice--warning #footnote-2-root}

A MAC address is just a value made of 6 pairs of [hexadecimal
numbers](https://en.wikipedia.org/wiki/Hexadecimal). The first 3 pairs are
usually assigned and associated with a particular hardware manufacturer, such
as Apple or Samsung:

$$ \underbrace{\texttt{A4:B8:05}}_{
        \rlap{\text{one of the Apple Inc. prefixes}}
    }\texttt{:44:D3:AD} $$

![iphone-mac]({{ "assets/img/iphone-mac.png" | absolute_url }}){: .align-right style="width: 50%; height: auto;"}

These MAC addresses are used for routing Internet traffic -- it's (part of) how
Facebook knows to send your ex-girlfriend's new profile picture to _you_, not
your neighbor. Facebook itself doesn't see the MAC address, but your router
does and keeps track of it.

**So what?** Well, any time you're connected to a WiFi network, anyone else on
that same network can _also_ find out your MAC address. This isn't a big deal
on the surface, because that information is necessary for them to be able to
communicate with you, but remember, it's a _unique ID for your device_.

For example, on my home network, I can find all of my devices:

{% highlight bash %}
    $ sudo nmap -sn 192.168.0.0/24 | grep "MAC Address" | awk '{print $3}'
    EC:08:xx:xx:xx:xx   # my router
    B8:27:xx:xx:xx:xx   # my laptop
    D4:63:xx:xx:xx:xx   # my TV
{% endhighlight %}

For simplicity, let's call the MAC address the "device ID" from now on.
{: .notice--info}

Now imagine you're sitting in a coffee shop on their WiFi and find the device
IDs of everyone else in the coffee shop. Then, you write them down on a napkin,
pack your bags, and head to the coffee shop down the street. Here, you do the
same thing, but this time, you hang out for a bit and enjoy your latte.
Eventually, you scan for device IDs again and guess what you see... A new one
that _matches one on your napkin_. That means... _someone from the last coffee
shop is here, too_. You don't know who they are yet (but you do know what kind
of device it is! remember the prefix from before?), but you _have_ tracked
their location!

Let's extrapolate this scenario further. You've created 25 clones of yourself
(while holding your laptop, so that got cloned too! 🙌) and are now hanging out
at 26 coffee shops at once. You are sipping your lattes and scanning for
devices. You'll see the coffee shop patrons, obviously, but because the
passerby's phones will occasionally try to connect to these free coffee shop
WiFis, you'll see their device IDs too. At the end of the day, you and your
clones gather together and compare notes. You notice that there is a device
that goes near coffee shop #1 to #7 to #8. Boom, you're now tracking someone's
location. How do you know the location? Well, either your clones wrote down
where they were, or you just [ask Google to tell
you](https://developers.google.com/maps/documentation/geolocation/), because of
course Google has an extensive database of open WiFi in the entire country. You
can thank Street View drivers and Android users for that.

## Putting It All Together ##
Instead of unrealistic clones that are limited by the speed of a human's brain,
let's harness the power of computers to gather this information for us. What if
we had a program that would automatically scan & connect to all open WiFi
networks, found all of the device IDs, and transmitted them to a central
server. Then, we set up [tiny devices](https://www.raspberrypi.org/products
/raspberry-pi-zero-w/) all over the place to create a sort of surveillance
network. Then, we can correlate device IDs at the central server in order to
track individual devices across the network (and, the exact GPS location via
Google).

In fact, I wrote a set of scripts to do this. You can [find it here [Linux
only]](https://github.com/Shaptic/Big-Brother). Here is how it looks:

#### On the correlation server ####
    Waiting for surveillance trackers...

    Tracker #1 connected from (latitude, longitude)...
    Tracker #2 connected from (latitude, longitude)...
    Tracker #2 transmitted device dump (14 devices):
      - AA:BB:CC:DD:EE:FF [last seen @ 10:22am]
      - ...
    Tracker #3 connected from (latitude, longitude)...
    Tracker #1 transmitted device dump (3 devices):
      - AA:BB:CC:DD:EE:FF [last seen @ 10:30am]
      - ...
    Correlated device! AA:BB:CC:DD:EE:FF tracked from #2 to #1. This is:
      (47.610190, -122.342558) -> (47.611043, -122.344688)

[(here's that path on Google Maps)](https://www.google.com/maps/dir/47.610190,+-122.342558/47.611043,+-122.344688/)
{: style="font-size: 70%;"}

#### On an individual tracker ####
    Detected open WiFi network: "Google Starbucks"... connected.
    Found 14 unique devices:
      - AA:BB:CC:DD:EE:FF
      - ...
    Detected open WiFi network: "I-Dont-Use-A-Password"... connected.
    ...
    Transmitting findings to surveillance server... done.

Creepy, right? And I'm just an average person. Imagine if I had the resources
of a massive corporation or the government.

![iPhone connecting to Google Starbucks]({{ "/assets/img/iphone-wifi.png" | absolute_url }}){: .align-left style="width: 50%; height: auto;"}

Someone like Google doesn't even need to go so far as to set up devices, they
just need to _become the free WiFi provider_. Case in point... You can actually
see your browser transmit this information when you click "Accept & Connect"
(or whatever variation there-of), in your browser:

    http://sbux-portal.appspot.com/splash?mac=[your-device-id]&apname=[wifi-device-id]

You really think they're providing this out of the goodness of their virtual
hearts? The government goes a step further: they can just use their [arsenal of
hacking tools](https://wikileaks.org/vault7/releases/#Cherry%20Blossom) to take
over _any_ wireless hotspot and transmit this same data, as evidenced by their
_Cherry Blossom_ project:

> _CherryBlossom_ is focused on **compromising wireless networking devices,
> such as wireless routers** and access points (APs), to achieve these goals.
> 
> Tasks for a _Flytrap_ include (among others) the scan for _email addresses_,
> _chat usernames_, _**MAC addresses**_ and _VoIP numbers_ in passing network
> traffic to trigger additional actions, ...

# Conclusion #
At the end of the day, what can we do about it? Well, it's near-impossible to
ensure your privacy across the Internet at large (especially not from the
government) but for this particular [attack
vector](https://en.wikipedia.org/wiki/Vector_(malware)), here is what you can
try:

  - Don't use open wireless networks.
  - Only have WiFi enabled when you're using it.
  - Disable automatic wireless networking joining.
  - If your device supports it, change your MAC address frequently (MAC
    spoofing).

If you decide that connecting to an open WiFi network is worth it, _never_ do
anything involving secure data (passwords, account numbers, etc.). Anything you
do on a wireless network can be seen, intercepted, and even modified by other
users.

> Anyone who steps back for a minute and observes our modern digital world
> might conclude that we have destroyed our privacy in exchange for convenience
> and false security.  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-- _John Twelve Hawks_

#### Footnotes ####
<sup><a href="#footnote-1-root">[1]</a></sup>  This is actually an exploitation
technique called a [Man-in-the-Middle attack](https://blog.lookout.com/spoofed-
wifi-60-minutes) where you create a custom WiFi network and call it something
generic (like "Starbucks") and then devices will automatically try to connect
to it because they recognize the name.
{: #footnote-1}

<sup><a href="#footnote-2-root">[2]</a></sup>  MAC addresses can be spoofed
(faked), and not every manufacturer generates a unique ID for each their
devices, but this applies as a general rule.
{: #footnote-2}
