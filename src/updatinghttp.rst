.. updatinghttp

Updating HTTP
=============

Wouldn't it be nice to make an improved protocol? It would include...

1. Make a protocol that's less RTT sensitive
2. Fix pipelining and the head of line blocking problem
3. Stop the need for and desire to keep increasing the number of connections to
   each host
4. Keep all existing interfaces, all content, the URI formats and schemes
5. This would be made within the IETF's HTTPbis working group

IETF and the HTTPbis working group
----------------------------------

The Internet Engineering Task Force (IETF) is an organization that develops and
promotes internet standards. Mostly on the protocol level. They're widely known
for the RFC series of documents documenting everything from TCP, DNS, FTP to
best practices, HTTP and numerous protocol variants that never went anywhere.

Within IETF dedicated "working groups" are formed with a limited scope to work
toward a goal. They establish a "charter" with some set guidelines and
limitations for what they should produce. Everyone and anyone is allowed to
join in the discussions and development. Everyone who attends and says
something has the same weight and chance to affect the outcome and everyone is
counted as humans and individuals, little care is given to which companies the
individuals work for.

The HTTPbis working group was formed during the summer of 2007 and was set out
to do an updated of the HTTP 1.1 specification – hence the "bis" part of the
name. Within this group the discussions about a next-version HTTP really
started during late 2012. The HTTP 1.1 updating work was completed early 2014.

The final inter-op meeting for the HTTPbis WG before the final version of the
http2 spec is expected to ship, will be held in New York City in the beginning
of June 2014. The IETF procedures to actually get an official RFC out of it
might then very well take the rest of the year or more.

Some of the bigger players in the HTTP field have been missing from the working
group discussions and meetings. I don't want to mention any particular company
or product names here, but clearly some actors on the Internet today seem to be
confident that IETF will do good without these companies being involved...

The “bis” part of the name
~~~~~~~~~~~~~~~~~~~~~~~~~~

The group is named HTTPbis where the "bis" part comes from the Latin adverb for
"two". Bis is commonly used as a suffix or part of the name within the IETF for
an update or the second take on a spec. Like in this case for HTTP 1.1.

http2 started from SPDY
-----------------------

SPDY [#f1]_ is a protocol that was developed and spearheaded by Google. They
certainly developed it in the open and invited everyone to participate but it
was obvious that they had huge benefits by being in control over both a popular
browser implementation and a significant server population with well-used
services.

When the HTTPbis group decided it was time to start working on http2, SPDY had
already proven that it was a working concept. It had shown it was possible to
deploy on the Internet and there were numbers published that proved how it
performed. The http2 work then subsequently started off from the SPDY/3 draft
that was basically made into the http2 draft-00 with a little search and
replace.

.. rubric:: Footnotes

.. [#f1] http://en.wikipedia.org/wiki/SPDY
