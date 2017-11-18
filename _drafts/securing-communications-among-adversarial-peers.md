---
layout:   single
banner:   "Securing Communications Among Adversarial Peers"
excerpt : "A technical musing on the inherent privacy limitations of a 
           peer-to-peer architecture."
categories: networking
header:
  overlay_image: /assets/img/keys-full.jpg
  overlay_filter: 0.1
  caption: "Photo credit: [**Pexels**](https://www.pexels.com/)"
---

# Internet Security #
There is a lot of implicit trust that goes into securing communications on the Internet. When you use HTTPS in your browser, you are trusting a [certificate authority](https://en.wikipedia.org/wiki/Certificate_authority) to make sure that you really _are_ giving your address and SSN to _Amazon.com_ and not an impostor. Basically, Amazon says "hey, I'm Amazon and here's my signature;" then, your browser compares it to the CAs signature.<sup><a href="#footnote-1">[1]</a></sup> If a CA validates a certificate for someone it shouldn't have (like someone calling themselves _Amaz**0**n.com_, your browser (and you, most likely) will implicitly trust them.
{: #footnote-1-root}

{% include toc title="Contents" %}

When you use an SSH client, you make a similar "leap-of-faith" when you trust the initial fingerprint. In this case, an active [man-in-the-middle attacker](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) can intercept and forward the communication between you and who you think you're talking to.

In both of these cases, attackers "get in the way" of the communication between the client and the server to intercept and modify the traffic.

# Transitioning to P2P #
When transitioning from a traditional client-server to a peer-to-peer model, you encounter similar issues.

![adversarial peer to peer]({{ "assets/img/adversarial-p2p.png" | absolute_url }}){: .align-right style="width: 150px; height: auto;"}

Suppose you have the network architecture on the right, and no node is aware of any edges other than the ones to its neighbors. Now, _A_ wants to talk to _Z_. The red node is adversarial, so if it sees something from _A_ going to _Z_, it will modify it.

Now, _A_ is not sure what the path of a packet destined for _Z_ will be. How can _A_ ensure that _Z_ gets an unchanged message?? If it follows the left path, it'll be safe, but if it goes right, it'll be modified by the red node.

### Hash Chaining? ### 
![hash chain graph]({{ "assets/img/hashchain.png" | absolute_url }}){: .align-right style="width: 40%; height: auto;"}

Suppose we bind the identity of the sender to the message itself. Since we're working with networked peers, the IP address and port pair is unique per-peer. Then, we create a hash chain like the one here. Is it possible for an attacker to imitate this hash and also be a middle man between to imitate this hash by an attacker?

Obviously, they can craft any message they want and unfortunately imitate the sender. The key is, though, _they won't see replies_! The _Z_ node will respond with the _A_ node as its destination, and the route the response takes won't necessarily go through the adversarial node!

### Perks of Non-Determinism ###


# Footnotes & References #
{: style="font-size: smaller;"}

<sup><a href="#footnote-1-root">[1]</a></sup>  Hey pedants, I know this isn't really how SSL works; it's an analogy to appeal to a wider audience and not clutter the post with technical details. Gimme a break. :)
{: #footnote-1 style="font-size: medium;"}
