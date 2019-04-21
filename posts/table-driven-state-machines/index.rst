<!-- 
.. title: Table-driven state machines
.. slug: table-driven-state-machines
.. date: 2013-06-21
.. tags: programming, c
.. category: 
.. link: 
.. description: 
.. type: text
-->

Some weeks ago I was writing a protocol stack for a serial RFID reader for
a commercial project I'm working on. Apart from one command, all the others
are standard serial communication where you send a fixed data structure and
receive a fixed data structure. But there's this command that does inventory,
which means, get all the RFID *tags* in the area. You request the data and it
starts sending many different packets for each *tag*.

The response format and size can vary but it always starts with a fixed
header which can be "RITM", "BITM", "EITM" or "IITM". So I basically wrote
a state machine do wait for one of these possible headers to start reading
the remaining data (size depends on the header type).

To make it easier I draw the following flowchart:

.. image:: /images/rfid_header_sm.jpg

I guess not many people will be impressed by my great drawing skills! Btw,
if anyone can suggest me a good flow charting software for free or low-cost
I'd be thankful.

The only missing edges on this diagram are the ones that connect
all states back to state *S0* when an invalid character is received.

Rudimentary state machine code
------------------------------

The code I initially wrote to solve the problem is shown below:

.. code-block:: cpp

    uint8_t rxbuf[64];
    int i;
    uint8_t b;
    uint8_t idx;

    #define S0 0
    #define S1 1
    #define S2 2
    #define S3 3
    #define S4 4

    while (1) {
        uint8_t state = S0;
        idx = 0;

        do {
            err = serial_read(&rxbuf[idx], 1);
            if (err != 1) continue;

            switch (state) {
            case S0:
                b = rxbuf[idx];
                if (isvalid(b)) {
                    state = S1;
                    idx = 1;
                } else {
                    idx = 0;
                }
                break;
            case S1:
                b = rxbuf[idx];
                if (b == 'I') {
                    state = S2;
                    idx = 2;
                } else if (isvalid(b)) {
                    state = S1;
                    rxbuf[idx-1] = rxbuf[idx];
                    idx = 1;
                } else {
                    state = S0;
                    idx = 0;
                }
                break;
            case S2:
                b = rxbuf[idx];
                if (b == 'T') {
                    state = S3;
                    idx = 3;
                } else if (isvalid(b)) {
                    state = S1;
                    rxbuf[0] = rxbuf[idx];
                    idx = 1;
                } else {
                    state = S0;
                    idx = 0;
                }
                break;
            case S3:
                b = rxbuf[idx];
                if (b == 'M') {
                    state = S4;
                } else if (isvalid(b)) {
                    state = S1;
                    rxbuf[0] = rxbuf[idx];
                    idx = 1;
                } else {
                    state = S0;
                    idx = 0;
                }
                break;
            }
        } while (state != S4);
    }

The function *isvalid* tests if the character received is a valid
starting character for the header strings know.

.. code-block:: cpp

    static uint8_t isvalid(uint8_t b)
    {
        return (b == 'R' || b == 'B' || b == 'E' || b == 'I');
    }

It worked as expect but it should be clear that it's messy and has a spaghetti
structure which could quickly grow into unmaintainable code. The problem
is having a *case* for each state and inside it a nested if/elseif test for
the received character.

Towards a better state machine structure
----------------------------------------

While reading |http://www.amazon.com/dp/1556220782/?tag=utzig-20| (great book!)
I stumbled across some designs for state machines and in particular one
table-driven which doesn't require object orientation and polymorphism. Although
the book only gives a rough example, it is clear enough. I previously saw a very
similar approach used in lexers where a given input regular expression generates
a table-driven state machine.

.. |http://www.amazon.com/dp/1556220782/?tag=utzig-20| raw:: html

    <a href="http://www.amazon.com/dp/1556220782/?tag=utzig-20" target="_blank">Programming Game AI by Example</a>

To get started the first thing I did was to define a *struct* to accomodate the
necessary data. Here's how it looks:

.. code-block:: cpp

    typedef struct
    {
        int8_t cur_state;
        uint8_t (*valid)(char);
        int8_t next_state;
        uint8_t index;
    } transition_t;

Basically to decide the new state what is used is a current state and the last
character received. But using the last character directly has many drawbacks
so I decided to use a function (*valid*) to decide the next state. The *index*
variable is custom data I need to get the position where the character will
be inserted in the input buffer. This is not really part of the state machine.

Here's what the declaration of the final state machine looks like:

.. code-block:: cpp

    #define S0 0
    #define S1 1
    #define S2 2
    #define S3 3
    #define S4 4

    static transition_t trs[] = {
        /* STATE 0 */
        {    S0,      initial_char,   S1,  0   },
        {    S0,          _true,      S0,  0   },
        /* STATE 1 */
        {    S1,         I_char,      S2,  1   },
        {    S1,      initial_char,   S1,  0   },
        {    S1,          _true,      S0,  0   },
        /* STATE 2 */
        {    S2,         T_char,      S3,  2   },
        {    S2,      initial_char,   S1,  0   },
        {    S2,         _true,       S0,  0   },
        /* STATE 3 */
        {    S3,         M_char,      S4,  3   },
        {    S3,      initial_char,   S1,  0   },
        {    S3,         _true,       S0,  0   },
    };

    uint8_t trs_size = sizeof(trs) / sizeof(trs[0]);

The element in the second column is a function which will evaluate if this is
the valid row. For example, when in state *S1* if *I_char* returns
*true* (by *C's* definition) then it goes to state *S2*. Else if
*initial_char* returns *true* it stays in state *S1*. The function *_true*
always returns a *true* value.

The order of the row declarations is important because they are tested
from the starting row towards the ending row.

The variable *trs_size* is the number of elements in the state machine
description used in the main algorithm to iterate over all elements.

These are the definitions of the fuctions used in the declaration of the
state machine:

.. code-block:: cpp

    static uint8_t initial_char(char c)
    {
        return (c=='R' || c=='B' || c=='E' || c=='I');
    }
    static uint8_t I_char(char ch) { return (ch == 'I'); }
    static uint8_t T_char(char ch) { return (ch == 'T'); }
    static uint8_t M_char(char ch) { return (ch == 'M'); }
    static uint8_t _true (char ch) { return 1; }

The function *initial_char* returns *true* if any of the characters
valid to initiate a header is found. There is also one function for each of
the extra possible characters. *_true* always returns *1*.

And here is the final state machine code!

.. code-block:: cpp

    uint8_t state = S0;
    do {
        err = serial_read(&b, 1);
        if (err != 1) continue;

        for (i = 0; i < trs_size; ++i) {
            if (state == trs[i].cur_state) {
                if (trs[i].valid((char) b)) {
                    state = trs[i].next_state;
                    rxbuf[trs[i].index] = b;
                    break;
                }
            }
        }
    } while (state != S4);

In the end the code is actually bigger and probably less efficient. But
it should be a lot easier to maintain. If I copy/paste it on another
project I will only need to modify the state machine array declaration
(and probably also the custom data).

Things that could be made better
--------------------------------

The is (at least) one thing which could be made better in the newer code.
Look at these function definitions below:

.. code-block:: cpp

    static uint8_t I_char(char ch) { return (ch == 'I'); }
    static uint8_t T_char(char ch) { return (ch == 'T'); }
    static uint8_t M_char(char ch) { return (ch == 'M'); }

As it must be quite obvious, they all look pretty much the same. Only the
character tested changes. There should be a way of fixing this with
preprocessor macros. I have in mind something like this:

.. code-block:: cpp

    #define CHAR_FUN(c) static uint8_t c##_char(char ch) { return (ch == '##c##'); }

    CHAR_FUN (I)
    CHAR_FUN (T)
    CHAR_FUN (M)

The problem in this code is that *preprocessor's* concatenation doesn't work
inside single quotes. So it generates the correct function name but the test
will look like *ch == '##c##'* instead of the correct character.

I'm not sure yet of how to do it but I'll keep trying!

UPDATE (22 Jun 2013)
--------------------

I just found a way of solving the problem above using *stringification* as
provided by the preprocessor. Not great but it does the job.

.. code-block:: cpp

    #define CHAR_FUN(c) static uint8_t c##_char(char ch){char *s=#c;return(ch==s[0]);}
    CHAR_FUN (I)
    CHAR_FUN (T)
    CHAR_FUN (M)
