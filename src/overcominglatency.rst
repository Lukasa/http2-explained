.. overcominglatency

Things done to overcome latency pains
=====================================

As always when faced with problems, people gather to find workarounds. Some of
the workarounds are clever and useful, some of them are just awful kludges.

Spriting
--------

Spriting is the term often used to describe when you put a lot of small images
together into a single large image. Then you use javascript or CSS to "cut out"
pieces of that big image to show smaller individual ones.

A site would use this trick for speed. Getting a single big image is much
faster in HTTP 1.1 than getting a 100 smaller individual ones.

Of course this has its downsides for the pages of the site that only want to
show one or two of the small pictures and similar. It also makes all pictures
get evicted from the cache at the same time instead of possibly letting the
most commonly used ones remain.

Inlining
--------

Inlining is another trick to avoid sending individual images, and this is by
instead using data: URLs embedded in the CSS file. This has similar benefits
and drawbacks as the spriting case.

.. code-block:: css

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }
    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

Concatenation
-------------

Similar to the previous two tricks, a big site today can end up with a lot of
different javascript files all over. Front-end tools will help developers to
merge everyone of them into a single huge lump so that the browser will get a
single big one instead of dozens of smaller files. Too much data is sent when
only little info is needed. Too much data needs to be reloaded when a change is
needed.

The annoyance to developers and the hoops everyone has to go through to do this
is of course "just" pain for the humans involved and not seen in any
performance numbers...

Sharding
--------

The final trick I'll mention that sites owners do to perform better in browsers
is often referred to as "sharding". It basically means spreading out your
service on as many different hosts as possible. It sounds crazy at first
thought, but there's a simple reason!

The HTTP 1.1 spec initially said that a client was allowed to use maximum two
TCP connections for each host. So, in order to not violate the spec clever
sites simply invented new host names and voilá, you could get more connections
to your site and the page load times shrunk.

Over time, that limitation was removed from the spec and today clients easily
use 6-8 connections per host name but they still have a limit so sites continue
to use this technique to bump the number of connections. As the number of
objects are ever increasing – as I showed before – the large number of
connections are then used just to make sure HTTP performs well and makes your
site fast. It is not unusual for sites to use well over 50 or even up to a 100
connections now using this technique.

Another reason is also to put images or similar resource on a separate host
name that doesn't use any cookies, as the size of cookies these days can be
quite significant. By using cookie-free image hosts you can sometimes increase
performance simply by allowing much smaller HTTP requests!

The picture below shows how it looks in a packet trace when browsing one of
Sweden's top web sites and how requests are distributed over several host
names.
