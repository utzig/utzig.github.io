<!-- 
.. title: Arrow and cryptographic protocols
.. slug: arrow-and-cryptographic-protocols
.. date: 2013-04-21
.. tags: crypto, tv-series
.. category: 
.. link: 
.. description: 
.. type: text
-->

This weekend I spent a lot of time watching Arrow_.
I started watching it last year but then lost interest and now decided to
get up to speed. I watched episodes 11 to 19 (most recent). It actually
got quite good.

.. _Arrow: http://www.imdb.com/title/tt2193021/?ref_=sr_1

The first 10 or so episodes are kind of self-contained. It's
like watching *Law and Order* were you can watch any episode and you don't
loose much. The exception are the flashbacks where the story of Oliver
Queen during the five years he spent as a castaway on an chinese Island and
was considered dead are told. Btw, the name Green Arrow was never used in
the series. The vigilante's name in the series is **The Hood**.

But from episode 12 and forward the story gets more serialized so that
every episode is a continuation of the previous one.

Although I'm liking the series, there are so many annoyances that requires
you to develop new levels of *suspension of disbelief*. Let me be clear:
I have no problem with super-heroes.
But there are no super-heroes in Arrow. There are only
heroes that are all normal people (with special training, of course). But than
you see every hero and villain killing many bandits, police offices and FBI
agents armed with machine guns using only rudimentary weapons like bow and
arrow or throwing knives... I roll my eyes everytime...

The best parts of the show are the ones told in flashbacks which tell the
story of Oliver Queen after a shipwreck and his years living with *soldiers*.

But the real reason for me to write this post is to mention about the computer
related scenes. Oh gosh... they are so bad that they are funny. One scene really
deserves special mention. It's on episode 11 and `Felicity Smoak`_ is hacking a pen
drive and then what follows is this screen of code:

.. _Felicity Smoak: http://arrow.wikia.com/wiki/Felicity_Smoak

.. code-block:: cpp
   :number-lines:

    for( i = 0; i < argc; i++)
       if( argv[i][0] == '-')
          switch( argv[i][1])
             {
             case 'j': case 'J':
                julian = 1;
                break;
             case 'q': case 'Q':
                quiet = 1;
                break;
             case 's': case 'S':
                sat_no = atoi( argv[i] + 2);
                break;
             case 'd': case 'D':
                n_days = atoi( argv[i] + 2);
                break;
             case 'f': case 'F':
                ofile = fopen( argv[i] + 2, "wb");
                break;
             case 'r': case 'R':
                data_file = fopen( argv[i] + 2, "ab");

And then she describes it as: **Military grade cryptographic security protocol**!!!

.. image:: /images/laugh.jpg

The code seems to come from this file:

https://bitbucket.org/nloadholtes/lunar/src/fd2d71716eda/jevent.cpp

Also funny was that the screens of code load slowly like if running on very
old computers... Go watch it on youtube:

`Felicity Smoak and security lesson!?`_

.. _Felicity Smoak and security lesson!?: http://www.youtube.com/watch?v=o-7ypLaVxsw#t=0m50s

Btw, every single people on the show uses Nokia Lumia's and LeNovo computers running
Windows 8... go figure!
