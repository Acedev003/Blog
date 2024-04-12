---
layout: post
title: Within A Game - Building a 7 Segment Display 
description: 
tags: minetest circuits within-a-game
---

So after a long time, thought to dust off this little page over here. One of the very few games I have in my device is <a href="https://www.minetest.net/">Minetest</a>, which is an open-source voxel game engine similiar to the likes of Minecraft - albeit a 100 times faster (C++ > Java). Minecraft allows for building circuitry and electrical contraptions via a sort of pixie dust called as **redstone**. Likewise, in minetest we have <a href="https://mesecons.net/">mesecons</a> - an even better circuity mod with built in logic gates such as AND, OR, NOT, NAND, FPGA's etc along with controllers, sensors etc.

<hr>
Starting off, what is a 7 segment display? As the name suggests, it is an electronic display device with 7 individual segments (labelled from **a** to **g**) of LED's arranged as the letter 8. Lighting up specific segments of this display allows us to represent various letters such as the digits 1-9, certain alphabets etc. 

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/7seg.jpg' | relative_url }}" style="width:300px;height:300px">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/7seg_numbering.png' | relative_url }}" style="height:300px">
</div>


Building one within minetest is quite simple. Slap on a bunch of lamps, wire them appropriately, add switches and voila, you have a working 7 segment display. But what fun exists to turn on a bunch of light segments with 7 switches.  What if we could let the digits change automatically, cycling through various digits and alphabets. Even better, what if we could run them without any hacks such as programmable controllers - just pure raw logic gates 🤠.

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/minetest_7seg.png' | relative_url }}" style="width:200px;height:400px">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/minetest_7seg_wiring.png' | relative_url }}" style="width:300px;height:400px">
</div>

<hr>

The heart of any computing device is it's clock which sends pulses at regular intervals. It's these pulses which prompt the various operations within the device, synchronizing its various components. A CPU, for example, relies on the clock to fetch and execute instructions, ensuring tasks are carried out in the correct sequence and timing. For us, this clock will be used to run a binary counter, which in turn will be used to drive the 7 segment display. Building a clock in minetest is relatively simple - you could just use the default built-in blinky plant, but it switches at an extremely high frequency of 0.3 Hz. As I didn't have the patience to bear this speed, a simple contraption using delayers was built.

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/clock.gif' | relative_url }}" style="width:400px;height:250px;max-height:300px">
</div>

Now that we have a clock that is fast enough, the next step  is to construct a counter that increments values and loops back to the initial value after reaching a certain count. Here we have settled for a 4-bit counter, which gives us a total of <b>2<sup>4</sup> = 16</b> values (0-9,A-F). The counter we have here is modelled as a asynchronous ripple counter built out of T flip-flops. <a href="https://www.youtube.com/watch?v=eEeBh8jfDjg&t=5s">This</a> video gives a good overview on the working principle of these counters. Have to admit that I ended up using a microcontroller implementation contrary to just pure logic gates in implementing the flip-flop for want of space 😅. In the end we have the following:

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/counter.gif' | relative_url }}" style="width:400px;height:250px;max-height:300px">
</div>

With the counter completed, our next objective is to create a digital converter capable of receiving any 4-bit binary value and producing its corresponding 7-segment activation, indicating which segment should be turned on or off. This involves generating a table that lists the activations (0 or 1) of each segment for every possible 4-bit binary input. From this table, we construct a 4-variable <a href="https://en.wikipedia.org/wiki/Karnaugh_map">Karnaugh Map</a> for each segment, resulting in a Boolean expression with a 4-variable input for each of the 7 segments. The following <a href="http://www.32x8.com/index.html">online tool</a> was used to create the K-Map for each segment. Along with the input-segment mapping table, a K-Map computed 4 variable mapping for segment 'A' is also given below.

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/truth-table.png' | relative_url }}" style="width:350px;height:350px;max-height:350px">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/seg_a.png' | relative_url }}" style="width:200px;height:350px;max-height:350px">
</div>

With all of this done, the final piece is to assemble this converter in minetest. After an hour of placing wires and blocks, this is what we finally end at:

<div class="img_parent">
<img src="{{ 'assets/images/2024-04-11-Building-a-7-Segment-Display-within-a-game/final.gif' | relative_url }}" style="width:500px;max-height:450px">
</div>

<hr>

WorldEdit files for the above  can be found at <a href="https://github.com/Acedev003/Minetest-Projects/tree/main/7 Seg Counter"> github.com/Acedev003/Minetest-Projects</a>. Another amazing reference for the concepts built here is the amazing video series by Ben Eater in <a href="https://www.youtube.com/playlist?list=PLowKtXNTBypGqImE405J2565dvjafglHU">Youtube</a>. Till then, cya'll . . .<br><br>
Jolly Exploring!