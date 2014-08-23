.. http2firefox

http2 in Firefox
================

Firefox has been tracking the drafts very closely and has provided http2 test
implementations for many months. During the development of the http2 protocol,
clients and servers have to agree on what draft version of the protocol they
implement which makes it slightly annoying to run tests. Just be aware so that
your client and server agree on what protocol draft they implement.

First, enable it
----------------

Enter ``'about:config'`` in the address bar and search for the option named
"network.http.spdy.enabled.http2draft". Make sure it is set to ``true``.

In the Firefox 34 Nightly versions since about August 8, http2 is enabled by
default!

TLS-only
--------

Remember that Firefox only implements http2 over TLS. You will only ever see
http2 in action with Firefox when going to https:// sites that offer http2
support.

Transparent!
------------

There is no UI element anywhere that tells that you're talking http2. You just
can't tell that easily. One way to figure it out, is to enable
"Web developer->Network" and check the response headers and see what you got
back from the server. The response is then "HTTP/2.0" something and Firefox
inserts its own header called "X-Firefox-Spdy:" as shown in the screenshot
above.

The headers you see in the Network tool when talking http2 have been converted
from http2's binary format into the old-style HTTP 1.x look-alike headers.
