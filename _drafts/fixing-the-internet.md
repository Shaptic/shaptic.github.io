---
layout:     single
title:      "A Fresh Perspective on the Architecture of the Internet"
banner:     "A Fresh Perspective on the Architecture of the Internet"
excerpt:    "Almost 30 years after the invention of the Internet, we are
            plagued with censorship, security problems, and privacy invasion.
            What can we do?"

categories: networking
header:
  overlay_image: /assets/img/internet-header.jpg
  overlay_filter: 0.7
---

# The Internet in Today's Digital Age #
The invention of the Internet has undoubtedly changed the world. It has given
us all access to an endless library of instant information, the ability to
communicate across the world in microseconds, and opened the door to a
digital Renaissance. 

{% include toc icon="globe" title="Quick Links" %}

As the saying goes, "with great power comes great responsibility," and the
Internet is no exception. Privacy issues aside, the Internet has become highly
vulnerable to censorship. Many of us access the same handful of websites every
day, and each of these has very tight control over what their users see. For
example:

  - Google has announced plans to [hide "fake news" from its search
    results](https://www.wsj.com/articles/google-pulled-into-debate-over-fake-
    news-on-the-web-1479159867), which is a pretty arbitrary label that can
    range from truly falsified articles to... well pretty much anything Google
    wants to hide. That's a dangerous power to put in the hands of a website
    that, for many, is the gateway to the rest of the Internet. Obviously, if
    they announce their plans to do it in the future, their ability to filter
    search results existed for a long time.

  - Facebook tilts the posts you see on your Timeline to align with your
    political leanings, engrossing the **2 billion active users** in
    personalized political echo-chambers, limiting exposure to alternate
    viewpoints.

  - Internet service providers (ISPs) can [block
    you](https://en.wikipedia.org/wiki
    /List_of_websites_blocked_in_the_United_Kingdom) from
    [reaching](http://gadgets.ndtv.com/internet/news/indian-isps-appear-to-be-
    blocking-access-to-internet-archive-1735151)
    [arbitrary](https://www.benchmarkemail.com/help-FAQ/answer/Is-a-major-ISP-
    blocking-your-emails) websites at their discretion, as can [_entire
    countries_](https://en.wikipedia.org/wiki/Great_Firewall) in times of peace
    and [of
    war](http://www.nytimes.com/2011/01/29/technology/internet/29cutoff.html).

The list of potential points of censorship goes on and on. Imagine a world
where Twitter hides positive tweets associated with a #hashtag to spread an
overall negative sentiment, or where Netflix hides documentaries that it thinks
might change the way you view a topic. Imagine a world in which Google filters
out any positive news about a political figure _entirely_, hides content from a
controversial content provider, or only shows finale spoilers when you search
for trivia on your favorite TV show.

For many people, these massive websites are the first hop on their way to the
Internet, and this kind of censorship potential poses some serious dangers to
the freedom of knowledge that the Internet promised.

What's at the core of this problem? One word: **centralization**.

## What is Centralization? ##
To perform a Google search, you send data from your computer, to your ISP, to
Google, and back (with dozens of other devices along the way). Thanks to
improved security (like when you see `https://` in your address bar), the
actual content of your search is private between you, Google, and the
government, but still presents the single point of failure within Google
itself. Google can track (and sell) what you've searched over time and modify
your results accordingly.

A **server** stores and provides data, like music, to anyone who asks for it.
Think Tumblr, Spotify, etc. A **client** is just a user that _asks_ the server
for some data and then does something with it (like plays a song).
{: .notice--info}

If your favorite video game's server is down, you can't play; if Spotify is
down, you can't listen to music; if Netflix is down, you can't watch TV; if
Amazon is down 
[over 100,000 websites might become unusable](https://techcrunch.com/2017/02/28/amazon-aws-s3-outage-is-breaking-things-for-a-lot-of-websites-and-apps/).

At the core of all of these problems is the principle of centralization: all of
them require a single, _central_ location to find their data. For the
companies, that's a good thing: they have full control over everything and can
track everything that happens with their service. For us, the consumers, it's a
bad thing: it's a single point of failure for our services.

# Decentralizing the Internet #
The Internet was naturally decentralized in its inception, but gradually grew
to the state it's in today, where a few dozen websites dominate 90% of the
average person's Internet usage.<sup><a href="#footnote-1" style="font-size:
smaller;">[1]</a></sup> What if, though, instead of accessing Spotify directly
to listen to music, we all, as users of the Spotify service, shared the music
amongst ourselves instead?
{: #footnote-1-root}

For Spotify, there are huge financial benefits in overall Internet usage (also
known as _bandwidth_) costs. Instead of providing every single person that
wants to listen to Taylor Swift's new single a copy of the song, we all just
share the hit amongst themselves; Spotify would only need to distribute a
handful of copies to get the "sharing" process started. They'd reduce their
Internet usage by a massive amount!

Your Spotify app _already downloads songs_ (even without Premium) in order to
avoid constantly asking the server for a new copy, and now we are just sharing
them with other users. This obviously isn't a form of "illegal file sharing,"
because nobody can actually play these songs except for Spotify users.

## BitTorrent ##
This sharing process is very similar to the way BitTorrent works, but the
"source" of the content is guaranteed to be legal. The stigma around the word
"torrent" is kind of like the one around "socialism": many people instantly
jump to negative conclusions and make false assumptions. Torrenting is **not**
inherently illegal. These are the steps to set up a torrent:

  - Create or find a file that you want to share.
  - Create a torrent, which is basically a tiny file describing your _shared_
    file.
  - Distribute the torrent file to people who want it.
  - These people are referred to the original file that you shared.
  - Everyone that's downloading (or has downloaded) the file all share pieces
    of the file amongst themselves, so you alone are no longer responsible for
    sharing the file to everyone. This reduces the overall "sharing cost" on
    any single user.

The only step of this that _can_ be illegal is the first one. If you rip a
movie from a DVD and share it, that's illegal, and this is what torrents are
primarily used for, hence the stigma. If you made your own song and shared it,
that's obviously not illegal. The _technology itself_ is extremely powerful and
can decentralize content consumption, solving many of the problems presented by
the current centralized model.

#### Footnotes ####
<sup><a href="#footnote-1-root">[1]</a></sup>  I made these numbers up, but
just think about how many truly _unique_ websites you visit a day. Remember,
YouTube is owned by Google, Amazon owns the Washington Post, etc.
{: #footnote-1}

