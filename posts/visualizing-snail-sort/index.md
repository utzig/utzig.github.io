<!-- 
.. title: Visualizing Snail Sort
.. slug: visualizing-snail-sort
.. date: 2016-12-31
.. tags: code, puzzle
.. category: 
.. link: 
.. description: 
.. type: text
-->

<script src="{{ site.url }}/js/d3.v3.min.js"></script>

Following my previous post on solving a Lattice Path "puzzle", I will
again write about a puzzle (hopefully for the last time!). This post
really is mostly just an excuse to use [D3](http://d3js.org/)!

This is a puzzle found on the [CodeWars](http://www.codewars.com/) website.
The objective is to write a function that given a square matrix, returns
a vector with the elements in snail sorted order. This might be better
understood by looking at this picture:

![Snail Sort]({{ site.url }}/images/snail.png)

This is a cool and simple algorithm to code so I would advise you to give
it a try before reading the rest of the post.

My approach to the problem was what to me seemed to be the most intuitive
answer. I kept track of the current top, bottom, left and right wall
coordinates and moved through the external walls, top, right, bottom, left
in sequence updating the external coordiantes till bottom reached top and
left reached right. I reckon this is not the most succint algorithm but at
least it seemed ok to me. My solution was:

{% highlight ruby linenos=table %}

def snail(array)
  return [] if array.length == 0 || array[0].length == 0

  top = left = 0
  bottom = right = array.length - 1

  result = []

  while left <= right && top <= bottom  
    left.upto(right) { |col| result << array[top][col] } 
    top += 1

    top.upto(bottom) { |row| result << array[row][right] }
    right -= 1

    right.downto(left) { |col| result << array[bottom][col] }
    bottom -= 1

    bottom.downto(top) { |row| result << array[row][left] }
    left += 1
  end

  result
end

{% endhighlight %}

To make it even easier to understand, I made a visualization of the
algorithm:

<svg id="snail-1"
     width="300"
     height="300"
     style="margin-left:auto;margin-right:auto;display:block;"></svg>
<script src="{{ site.url }}/js/snail-1.js"></script>

So, the cool thing is that when you submit a solution on CodeWars
you get access to the whole history of solutions submitted by everyone
and they are even sorted by how "beautiful" they look.

The solution that was considered the most beautiful was this one:

{% highlight ruby linenos=table %}

def snail(array)
  array.empty? ? [] : array.shift + snail(array.transpose.reverse)
end

{% endhighlight %}

Seriously, when I looked at this my first reaction was "What the f\*ck!".

Have fun!
