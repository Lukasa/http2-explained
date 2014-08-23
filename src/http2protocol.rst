.. http2protocol

The http2 protocol
==================

Enough about the background, the history and politics behind what took us here.
Let's dive into the specifics of the protocol. The bits and the concepts that
create http2.

Binary
------

http2 is a binary protocol.

Let that sink in for a minute. If you're a person that's been involved in
internet protocols before, chances are that you react strongly with instinct
against this and prepare your refute arguments to spell out how useful it is
with protocols that are made with text/ascii and that you've done HTTP 1.1
requests numerous times by telnetting to servers and entered the requests by
hand.

http2 is binary to make the framing much easier. Figuring out the start and the
end of frames is one of the really complicated things in HTTP 1.1 and actually
in text based protocols in general. By moving away from optional white spaces
and different ways to write the same thing, implementations become simpler.

Also, it makes it much easier to separate the actual protocol parts from the
framing - which in HTTP1 is confusingly intermixed.

The facts that the protocol features compression and often will run over TLS
also diminish the value of text since you won't see text over the wire anyway.
We simply have to get used to the idea to use a Wireshark inspector or similar
to figure out exactly what's going on at protocol level in http2.

Debugging of this protocol will instead probably have to be done with tools
like curl or by analyzing the network stream with Wireshark's http2 dissector
and similar.

The binary format
-----------------

http2 sends frames. There's a dozen different frames that can be sent and they
all have the same setup:

Type, Length, Flags, Stream Identifier and
frame payload.

There are twelve different frames in the current http2 draft and the two
perhaps most fundamental ones that map HTTP 1.1 features are DATA and HEADERS.
I'll describe some of the frames in closer detail further on.

Multiplexed streams
-------------------

The Stream Identifier mentioned in the previous section describing the binary
frame format, makes each frame sent over http2 get associated with a "stream".
A stream is a logical association. An independent, bi-directional sequence of
frames exchanged between the client and server within an http2 connection.

A single http2 connection can contain multiple concurrently open streams, with
either endpoint interleaving frames from multiple streams. Streams can be
established and used unilaterally or shared by either the client or server and
they can be closed by either endpoint. The order in which frames are sent
within a stream is significant. Recipients process frames in the order they are
received.

Multiplexing the streams means that packages from many streams are mixed over
the same connection. Two (or more) individual trains of data are made into a
single one and then split up again on the other side. Here are two trains:

So they're crammed into and over the same connection in a multiplexed manner:

In http2 we will see tens and hundreds of simultaneous streams. The cost of
creating a new one is very low.

Priorities and dependencies
---------------------------

Each stream also has a priority, which is used to tell the peer which streams
to consider most important.

The exact details on how priorities work in the protocols have changed several
times and are still being debated. The point is however that a client can
specify which streams that are most important and there's a dependency
parameter so that one stream can be made dependent on another.

The priorities can be changed dynamically in run-time, which should enable
browsers to make sure that when users scroll down a page full of images it can
specify which images that are most important, or if you switch tabs it can
prioritize a new set of streams then suddenly come into focus.

Header compression
------------------

HTTP is a state-less protocol. In short that means that every request needs to
bring with it as much details as the server needs to serve that request,
without the server having to store a lot of info and meta-data from previous
requests. Since http2 doesn't change any such paradigms, it too has to do this.

This makes HTTP repetitive. When a client asks for many resources from the same
server, like images from a web page, there will be a large series of requests
that all look almost identical. A series of almost identical something begs for
compression.

While the number of objects per web page increases as I've mentioned earlier,
the use of cookies and the size of the requests have also kept growing over
time. Cookies also need to be included in all requests, mostly the same over
many requests.

The HTTP 1.1 request sizes have actually gotten so large over time so they
sometimes even end up larger than the initial TCP window, which makes them very
slow to send as they need a full round-trip to get an ACK back from the server
before the full request has been sent. Another argument for compression.

Compression is a tricky subject
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

HTTPS and SPDY compressions were found to be vulnerable to the BREACH [#f1]_
and CRIME attacks [#f2]_. By inserting known text into the stream and figuring
out how that changes the output, an attacker can figure out what's being sent.

Doing compression on dynamic content for a protocol without then becoming
vulnerable for one of these attacks requires some thoughts and careful
considerations. This is what the HTTPbis team tries to do.

Enter HPACK [#f3]_, *Header Compression for HTTP/2*, which - as the name
suitably suggests - is a compression format especially crafted for http2
headers and is strictly speaking being specified in a separate internet draft.
The new format, together with other counter-measures such as a bit that asks
intermediaries not to compress a specific header and optional padding of frames
should make it harder to exploit this compression.

In the words of Roberto Peon (one of the creators of HPACK) "HPACK was designed
to make it difficult for a conforming implementation to leak information, to
make encoding and decoding very fast/cheap, to provide for receiver control
over compression context size, to allow for proxy re-indexing (i.e. shared
state between frontend and backend withing a proxy), and for quick comparisons
of huffman-encoded strings."

Reset - change your mind
------------------------

One of the drawbacks with HTTP 1.1 is that when a HTTP message has sent off
with a Content-Length of a certain size, you can't easily just stop it. Sure
you can often (but not always – I'll skip the lengthy reasoning of exactly why
here) disconnect the TCP connection but that then comes as the price of having
to negotiate a new TCP handshake again.

You would much rather just stop the message and start anew. This can be done
with http2's RST_STREAM frame which thus will help preventing wasting bandwidth
and this without having to tear down any connection.

Server push
-----------

This is the feature also known as "cache push". The idea here is that if the
client ask for resource X the server may know that the client then also most
likely want resource Z and sends that to the client without it asking for it
first. It helps the client to put Z into its cache so that it'll be there when
it wants it.

Server push is something a client explicitly must allow the server to do and
even if a client does that, it can at its own choice swiftly terminate a pushed
stream with RST_STREAM should it not want a particular one.

Flow Control
------------

Each individual stream over http2 has its own advertised flow window that the
other end is allowed to send data for. If you happen to know how SSH works,
this is very similar in style and spirit.

For every stream both ends have to tell the peer that it has more room to fit
incoming data in, and the other end is only allowed to send that much data
until the window is extended. Only DATA frames are flow controlled.

.. rubric:: Footnotes

.. [#f1] http://en.wikipedia.org/wiki/BREACH_%28security_exploit%29
.. [#f2] http://en.wikipedia.org/wiki/CRIME
.. [#f3] http://tools.ietf.org/html/draft-ietf-httpbis-header-compression-09
