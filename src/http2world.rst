.. http2world

A http2 world
=============

So what will things look like when http2 gets adopted? Will it get adopted?

How will http2 affect ordinary humans?
--------------------------------------

http2 is not yet widely deployed nor used. We can't tell for sure exactly how
things will turn out. We have seen how SPDY has been used and we can make some
guesses and calculations based on that and other past and current experiments.

http2 reduces the number of necessary network round-trips and it avoids the
head of line blocking dilemma completely with multiplexing and fast discarding
of unwanted streams.

It allows a large amount of parallel streams that go way over even the most
sharding sites of today.

With priorities used properly on the streams, chances are much better that
clients will actually get the important data before the less important data.

All this taken together, I'd say that the chances are very good that this will
lead to faster page loads and to more responsive web sites. Shortly put: a
better web experience.

How much faster and how much improvement we will see, I don't think we can say
yet. First, the technology is still very early and then we haven't even started
to see clients and servers trim implementations to really take advantage of all
the powers this new protocol offers.

How will http2 affect web development?
--------------------------------------

Over the years web developers and web development environments have gathered a
full toolbox of tricks and tools to work around problems with HTTP 1.1, some of
them outlined in the beginning of this document.

Lots of those workarounds that tools and developers now use by default and
without thinking will probably hurt http2 performance or at least not really
take advantage of http2's new super powers. Spriting and inlining should most
likely not be done with http2. Sharding will probably be detrimental to http2
as it will probably benefit from using fewer connections.

A problem here is of course that web sites and web developers need to develop
and deploy for a world that in the short term at least, will have both HTTP1.1
and http2 clients as users and to get maximum performance for all users can be
challenging without having to offer two different front-ends.

For these reasons alone, I suspect there will be some time before we will see
the full potential of http2 being reached.

http2 implementations
---------------------

Trying to document specific implementations in a document such as this is of
course completely futile and doomed to fail and only feel outdated within a
really short period of time. Instead I'll explain the situation in broader
terms and refer readers to the `list of implementations`_ on the http2 web
site.

There was a large amount of implementations already early on, and the amount
has increased over time during the http2 work. At the time of this writing
there are 19 implementations and several of them implement the latest drafts.

Firefox has been the browser that's been on top of the bleeding edge drafts,
Twitter has kept up and offered its services over http2. Google started during
April to offer draft-13 support on a few test hosts running their services and
since late May they offer draft-12 in the latest version of Chrome.

curl and libcurl has its implementation based on the separate http2 library
called nghttp2 and supports plain-text http2 as well as the TLS based using
one out of several different TLS libraries. Nghttp2 and curl were among the
first implementers that presented draft-14 support.

.. _list of implementations: https://github.com/http2/http2-spec/wiki/Implementations

Common critiques of http2
-------------------------

During the development of this protocol the debate has been going back and
forth and of course there is a certain amount of people who believe this
protocol ended up completely wrong. I wanted to mention a few of the more
common complaints and mention the arguments against them:

"The protocol is designed or made by Google"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It also has variations implying that the world gets even further dependent or
controlled by Google by this. This isn't true. The protocol is developed within
the IETF as the same fashion protocols have been developed for over 30 years.
However, we all recognize and acknowledge Google's awesome work with SPDY that
not only proved that it is possible to deploy a new protocol this way but also
helped giving numbers on what can be gained.

"The protocol is only useful for browsers and big services"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is sort of true. One of the primary drivers behind the http2 development
is the fixing of HTTP pipelining. If your use case originally didn't have any
need for pipelining then chances are http2 won't do a lot of good for you. It
certainly isn't the only improvement in the protocol but a big one.

As soon as services start realizing the full power and abilities the
multiplexed streams over a single connection brings, I suspect we will see more
application use of http2.

Small REST APIs and simpler programmatic uses of HTTP 1.x may not find the step
to http2 to offer very big benefits. But also, there should be very few
downsides with http2 for most users.

"Its use of TLS makes it slower"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is true to some extent. The TLS handshake does add a little extra, but
there are existing and ongoing efforts on reducing the necessary round-trips
even more for TLS. The overhead for doing TLS over the wire instead of
plain-text is not insignificant and clearly notable so more CPU and power will
be spent on the same traffic pattern as a non-secure protocol. How much and
what impact it will have is a subject of opinions and measurements. See for
example `istlsfastyet.com`_ for one source of info.

http2 does not make TLS use mandatory so we shouldn't conflate the terms.

Lots of users on the Internet today already want and prefer TLS to be used more
widely anyway and we should help to protect users' privacy.

.. _istlsfastyet.com: https://istlsfastyet.com/

"Not being ASCII is a deal-breaker"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes, we like being able to see protocols in the clear since it makes debugging
and tracing easier. But text based protocols are also more error prone and open
up for much more parsing and parsing problems.

If you really can't take a binary protocol, then you couldn't handle TLS and
compression in HTTP 1.x either and its been there and used for a very long
time.

Will http2 become widely deployed?
----------------------------------

It is much too early for me to tell for sure, but I can still guess and
estimate and that's what I'll do here.

The nay-sayers will say "look at how good IPv6 has done" as an example of a new
protocol that's taken decades to just start to get widely deployed. http2 is
not an IPv6 though. This is a protocol on top of TCP using the ordinary HTTP
update mechanisms and port numbers and TLS etc. It will not require most
routers or firewalls to change at all.

Google proved to the world with their SPDY work that a new protocol like this
can be deployed and used by browsers and services with multiple implementations
in a fairly short amount of time. While the amount of servers on the Internet
that offer SPDY today is in the 1% range, the amount of data those servers deal
with is much larger. Some of the absolutely most popular web sites today offer
SPDY.

http2, based on the same basic paradigms as SPDY, I would say is likely to be
deployed even more since it is an IETF protocol. SPDY deployment was always
held back a bit by the "it is a Google protocol" stigma.

There are several big browsers behind the roll-out. At least representatives
from Firefox, Chrome and Internet Explorer have expressed they will ship http2
capable browsers.

There are several big server operators that are likely to offer http2 soon,
including Google, Twitter and Facebook and we expect to see http2 support going
into popular server implementations such as the Apache HTTP Server and nginx.

