---
title: Research Ideas
aliases:
  - "/ideas.html"
---

<!-- TODO: Link to impactful research -->

We need people to attack the system, quantify defenses, etc. Here are some
example projects:

*   What algorithm should we use to assign Guard flags such that a) we assign
    the flag to as many relays as possible, yet b) we minimize the chance that
    Alice will use an attacker's node as a guard? See the [blog
    post](https://blog.torproject.org/research-problem-better-guard-rotation-parameters)
    for details.
*   For various diversity metrics, how has the diversity of the Tor network
    changed over time? How robust is it to change or attack? These results can
    help us make better design decisions. See the [blog
    post](https://blog.torproject.org/research-problem-measuring-safety-tor-network)
    for details.
*   If we prevent the really loud users from using too much of the Tor network,
    how much can it help? We've instrumented Tor's entry relays so they can
    rate-limit connections from users, and we've instrumented the directory
    authorities so they can change the rate-limiting parameters globally across
    the network. Which parameter values improve performance for the Tor network
    as a whole? How should relays adapt their rate-limiting parameters based on
    their capacity and based on the network load they see, and what
    rate-limiting algorithms will work best? See the [blog
    post](https://blog.torproject.org/research-problem-adaptive-throttling-tor-clients-entry-guards)
    for details.
*   Right now Tor clients are willing to reuse a given circuit for ten minutes
    after it's first used. The goal is to avoid loading down the network with
    too many circuit creations, yet to also avoid having clients use the same
    circuit for so long that the exit node can build a useful pseudonymous
    profile of them. Alas, ten minutes is probably way too long, especially if
    connections from multiple protocols (e.g. IM and web browsing) are put on
    the same circuit. If we keep fixed the overall number of circuit extends
    that the network needs to do, are there more efficient and/or safer ways
    for clients to allocate streams to circuits, or for clients to build
    preemptive circuits? Perhaps this research item needs to start with
    gathering some traces of what requests typical clients try to launch, so
    you have something realistic to try to optimize.
*   The "website fingerprinting attack": make a list of a few hundred popular
    websites, download their pages, and make a set of "signatures" for each
    site. Then observe a Tor client's traffic. As you watch him receive data,
    you quickly approach a guess about which (if any) of those sites he is
    visiting. First, how effective is this attack on the deployed Tor design?
    The problem with all the previous attack papers is that they look at timing
    and counting of IP packets on the wire. But OpenSSL's TLS records, plus
    Tor's use of TCP pushback to do rate limiting, means that tracing by IP
    packets produces very poor results. The right approach is to realize that
    Tor uses OpenSSL, look inside the TLS record at the TLS headers, and figure
    out how many 512-byte cells are being sent or received. Then start
    exploring defenses: for example, we could change Tor's cell size from 512
    bytes to 1024 bytes, we could employ padding techniques like [defensive
    dropping](http://freehaven.net/anonbib/#timing-fc2004), or we could add
    traffic delays. How much of an impact do these have, and how much usability
    impact (using some suitable metric) is there from a successful defense in
    each case?
*   More coming soon. See also the "Research" section of the
    [volunteer](https://www.torproject.org/getinvolved/volunteer.html.en#Research)
    page for other topics.

Anti-censorship
---------------

* Study how censors crawl [BridgeDB](https://bridges.torproject.org) by setting
  up several bridges and distributing them over all of BridgeDB's distribution
  mechanisms (using tor's `BridgeDistribution` option).  Continuously measure
  the reachability of your bridges and determine when and how they are getting
  blocked.
  - We also want to understand how and whether benign third parties are using or
    scraping Tor bridges. We have a [research project
    sketch](https://bugs.torproject.org/30636#comment:33) to get you started.
* Measure the reachability of our [Snowflake](https://snowflake.torproject.org)
  broker and bridge.  Where in the world are they blocked?  And how?  This will
  help us understand what countries and networks started paying attention to
  Snowflake, and let volunteers know where Snowflake proxies will be
  ineffective.
* Reverse-engineer and study a deep packet inspection (DPI) system
  [as outlined here](https://github.com/net4people/bbs/issues/15).
  Understanding the capabilities of DPI systems lets us calibrate our threat
  models and build obfuscation protocols that defend against concrete rather
  than speculative attacks.
* Understand what's happening with reported Shadowsocks blocking in China since
  mid-September. Is it active probing?  Or large-scale flow analysis?  Something
  else?
* Understand the WebRTC fingerprint of Pion-WebRTC, in the style of [an earlier
  analysis](https://trac.torproject.org/projects/tor/wiki/doc/Snowflake/Fingerprinting).
* Evaluate the effectiveness of obfs4's simplistic "slap some padding onto
  application data" approach and design a better traffic analysis defense. What
  is the the "right" packet sending schedule (assuming no restrictions on
  efficiency)?  Where a schedule is an algorithm that outputs a sequence like
  "wait 50 ms, send 200 bytes, wait 4100 ms, send 1460 bytes, ..." Start by
  proposing a strawman schedule to give us something to work with.  Ideally, we
  would like to see an adversarial analysis: let one team develop a scheduler
  (using their own collected traffic), and a different team evaluate it (using
  their own collected traffic).
