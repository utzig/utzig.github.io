<!-- 
.. title: Solving Lattice Path
.. slug: solving-lattice-path
.. date: 2014-06-11
.. tags: project-euler, math, puzzle
.. category: 
.. link: 
.. description: 
.. type: text
-->

**SPOILER**: if you want to or are working on the exercises from
`Project Euler`_, you should stop reading now!

According to Wikipedia, `Project Euler`_ is a website dedicated to a
series of computational problems intended to be solved with computer programs
[1]_.

.. _Project Euler: http://projecteuler.net/
.. [1] http://en.wikipedia.org/wiki/Project_Euler

The problems are mostly math related and one main characteristic of most problems
is that they are usually hard to solve through brute force because they usually
have polynomial/exponential complexity and questions always ask for very large input
or number of inputs.

Problem 15, Lattice Paths, is one that I found really interesting because it's
a geometric problem and with an intuitive solution. Let me copy here the text from
the problem.

    Starting in the top left corner of a 2×2 grid, and only being able to move to the
    right and down, there are exactly 6 routes to the bottom right corner.

.. image:: /images/lattice_path.png

..

    How many such routes are there through a 20×20 grid?

So my first reaction was: *I'll just do a brute force algorithm and see what happens!*

I wrote an algorithm that uses recursion to find all possible paths. This is how it
ended up:

.. code-block:: ruby
   :number-lines:

    class LatticePath
      def initialize(size)
        @size = size
      end

      def total
        moves(0, 0)
      end

      private

      def moves(rows, cols)
        return 1 if rows == @size && cols == @size
        return 0 if rows > @size || cols > @size
        return moves(rows + 1, cols) + moves(rows, cols + 1)
      end
    end

    sides = (ARGV[0] && ARGV[0].to_i) || 20 # default value if none specified
    lattice_path = LatticePath.new(sides)
    puts lattice_path.total

Well, how bad could it be? Turn outs very bad.

Running this for less than 13 sides was OK. Running this for 14 sides was
enough to take quite some time. Running for more than 14, it just takes too
long. I suppose it would take years for 20 sides. It obviously explodes in
complexity very quickly.

Looking up for ways to solve this or similar problems, I ended up watching this
video from `Khan Academy`_ where Salman basically gives it away!

.. _Khan Academy: https://www.khanacademy.org/

Path Counting Brain Teaser
--------------------------

.. raw:: html

    <iframe width="560" height="420"
        src="https://www.youtube.com/embed/9QduzzW10uA?color=white&theme=light">
    </iframe>

I will not try to re-explain what's in the video so you should watch it first.
I'll follow from where the video ends, and will develop an algorithm that only
requires an array of size equal to one _side_ of the matrix to solve this problem.

Basically after filling in values for each cell, what we get is an matrix like
this:

.. image:: /images/full_grid.png

And looking more carefully, we can see that this matrix is symmetric. The coloring
I used on the pictures should make that clear. We are interested in the value on
the main diagonal so we can remove everything below it.

.. image:: /images/half_grid.png

The next step was to recognize that each cell depends only on the cell to the left
and above it. The element in the diagonal is always the element above it multiplied
by 2 and the other elements in that row are the sum of the element in the cell to
the left with the element in the cell above them. The picture below should make
it clearer.

.. image:: /images/grid_formula.png

Now even more interesting is observing that every element only depends on the
elements exactly above them and once a new row is calculated from the row above
it, the former is not necessary anymore. So in the end the whole problem can be
solved by a simple array of size equal to the number of cell on one side of the
square matrix!

And here's my optimized solution to the problem:

.. code-block:: ruby
   :number-lines:

    #!/usr/bin/env ruby

    def lattice_path(sides)
      a = Array.new(sides) { 1 }

      1.upto(sides - 1) do |col|
        a[col] *= 2
        (col + 1).upto(sides - 1) do |n|
          a[n] = a[n-1] + a[n]
        end
      end

      a[sides - 1]
    end

    sides = (ARGV[0] && ARGV[0].to_i) || 20 # default value if none specified
    puts lattice_path(sides + 1)

Have fun!
