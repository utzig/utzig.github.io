<!-- 
.. title: Asus 1025c
.. slug: asus-1025c-review
.. date: 2013-02-21 00:00:00 UTC-03:00
.. tags: netbook
.. category: 
.. link: 
.. description: 
.. type: text
-->

Last November I did not resist the temptation and bought a Netbook.
For some time I had been wishing to buy a **very portable** computer
that could be carried anywhere without much effort. I'd been keep an eye on
the Asus 1025c for a few months due to its relatively well powered
processor (for an Atom, obviously) and low-price.

So, on Black Friday, the day of making new debts, which we proudly copied
from the Yankees, although ours is a kinda watered down version, I saw it
for 75% of the normal price and voilá, here it is.

The specs of this **beast** are:

- Atom N2600 1.6GHz
- 2GB RAM
- 320GB HDD
- 10" screen
- Atheros AR9485 WiFi

It came with Windows 7 Starter so guess what's the first thing to do? Not
hard, huh.

I ended up installing Linux (Arch), FreeBSD and NetBSD. Despite the quite
new hardware every OS works fairly well. Linux had a lot of problems
with the WiFi chip initially but in the latest kernel versions it works
really well.

The low-point has been the video support. For the N2x00 Atom family, Intel
decided to license the PowerVR SGX 545 core from Imagination Technologies.
So basically there's no 2D and 3D acceleration. Intel even released a closed
source driver which require some old versions of many packages which there's
no way to use with a rolling release like Arch Linux.

In general I'm impressed with the performance of the machine. It's not close
to a Core i5/7 but it's better than I expected. As long as the quad-core is
exploited with things like *make -j4* it is good to go. The two main problems
with system limitations were: running Eclipse for anything; and synthesizing
VHDL/Verilog designs. The Xilinx tools take a really long time to synthesize
anything. But they are slow even on an i5 anyway!
