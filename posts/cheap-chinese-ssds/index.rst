<!-- 
.. title: Cheap Chinese SSDs
.. slug: cheap-chinese-ssds
.. date: 2013-04-29
.. tags: storage
.. category: 
.. link: 
.. description: 
.. type: text
-->

Since I bought a Netbook last year, I wasn't very happy with it's HDD. The reason
was that I had to disable the power management or at least configure it to not
allow the spin down of the drive.

.. code-block:: sh

    $ sudo hdparm -B 255 /dev/sda

The problem was so annoying that I could not even edit a text file on Vim without
the disc spinning down constantly and hanging text entry for some seconds. Disabling
power management was effective to solve the problem but then the battery would
discharge faster. Also since the Netbook is so small and light, using it at any
surface that is not very flat and stable causes slight movements which could
damage the drive. It all bored me.

So I wanted to install a SSD because it would solve all problems. No more
disc spinning down, no need to disable power management and no requirement
of having a stable surface to use it. The problem was the price. A SSD drive
purchased here in Brazil would end up costing more that the Netbook itself!

I did some research and ended up buying a cheap chinese SSD on Aliexpress. The link
to the seller/product I purchased follows:

`Kingspec 2.5 SATA 32GB`_

.. _Kingspec 2.5 SATA 32GB: http://www.aliexpress.com/item/2-5-Inch-SATA-II-SSD-32GB-4-Channel-Solid-State-Disk-MLC-For-Notebook-computer/571854249.html

After almost two months I'm happy so far. I was afraid it would end up giving
some problem after some use but until now is like it was on first day. The system
boots in something like 3 seconds (after BIOS). Everything got boosted up.
The main problem is the size which prevents the use of some software. But I could
build a Linux distro based on buildroot for a project I'm working on, I built
cross toolchains for ARM/AVR/MSP430 and build llvm (which took 8 hours!!!). It suits
me well since I can do the things I do day to day.

When I bought it in January it cost me US$ 43. Now it's selling for US$ 39. I wonder
how low this price will get in the next 3 months, 1 year. Even being a very simple
SSD, lacking *important* features like TRIM, still was a great purchase. A now with
more and more embedded boards coming with a SATA controller...
