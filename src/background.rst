.. _background

Background
==========

This is a document describing http2 from a technical and protocol level. It
started out as a presentation I did in Stockholm in April 2014. I've since
gotten a lot of questions about the contents of that presentation from people
who couldn't attend, so I decided to convert it into a full-blown document with
all details and proper explanations. At this moment in time when this is
written (August 13, 2014), the final http2 specification has not been completed
nor shipped. The current version is called draft-14 [#f1]_ and with luck
this is pretty much the format that will become the RFC. This document
describes the current situation, which may or may not change in the final
specification. All and any errors in the document are my own and the results of
my shortcomings. Please point them out to me and I might do updates with
corrections.

*This is document version 1.6.*

Author
------

My name is Daniel Stenberg and I work for Mozilla. I've been working with open
source and networking for over twenty years in numerous projects. Possibly I'm
best known for being the lead developer of curl and libcurl. I've been involved
in the IETF HTTPbis working group for several years and there I've kept
up-to-date with the refreshed HTTP 1.1 work as well as being involved in the
http2 standardization work.

Email:
    daniel@haxx.se

Twitter:
    `@bagder`_

Web:
    `daniel.haxx.se`_

Blog:
    `daniel.haxx.se/blog`_

.. _@bagder: https://twitter.com/bagder
.. _daniel.haxx.se: http://daniel.haxx.se/
.. _daniel.haxx.se/blog: http://daniel.haxx.se/blog

Help!
-----

If you find mistakes, omissions, errors or blatant lies in this document,
please send me a refreshed version of the affected paragraph and I'll make
amended versions. I will give proper credits to everyone who helps out! I hope
to make this document better over time.

This document is available at http://daniel.haxx.se/http2

License
-------

This document is licensed under the Creative Commons Attribution 4.0 license:
http://creativecommons.org/licenses/by/4.0/

.. image:: http://i.creativecommons.org/l/by/3.0/88x31.png

.. rubric:: Footnotes

.. [#f1] http://tools.ietf.org/html/draft-ietf-httpbis-http2-14
