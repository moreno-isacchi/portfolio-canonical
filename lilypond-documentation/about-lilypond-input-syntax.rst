*****************************
About LilyPond input syntax
*****************************

Reading a musical score, a human interprets a series of symbols, commonly referred to as notes, which are defined by their pitch and duration. Each one of these notes has a meaning by itself and can thus be considered a token. Although a token is a meaningful unit, a series of tokens (notes) has to be read in the correct order to form the complete musical piece, as a token can influence the following one. Thus, a human reads a musical score from left to right, from top to bottom, and translates each note into a sound. As strange as it may seem, LilyPond behavior is not that different. Unlike WYSIWYG musical engraving software, LilyPond compiles a source ``.ly`` file to create a ``.pdf`` output containing the engraved music. That is, it parses an input file in a precise order, one token after another, and engraves them on a score, taking into account the context and the interaction of tokens.
A LilyPond source file contains different sections and commands, but at its core it always has a musical expression. The smallest musical expression possible is a single note, which, as mentioned, is defined by its pitch and duration. This document focuses precisely on those two key elements.

Pitches
^^^^^^^
According to scientific pitch notation (SPN), a method commonly used to indicate musical pitches, a pitch is specified by a note name (for example A, B, C, and so on) combined with a number indicating the pitch's octave.

LilyPond follows a similar logic, and it uses lowercase letters ``a`` through ``g`` to indicate note names. To set octaves, instead of digits like in SPN, LilyPond uses one or multiple instances of a special character—a single quote (``'``) or a comma (``,``)—placed immediately after a note name. Each ``'`` raises the pitch by an octave, each ``,`` lowers it by the same interval.

In this way, a plain note name is engraved as a pitch belonging to the Octave 3, the octave just below Middle C (C4). 
A ``c`` followed by a single ``'`` will be engraved as C4, followed by ``''`` C5, and so on. Similarly, a ``c`` followed by a ``,`` will be engraved as C2, followed by ``,,`` as C1, and so on.
For example, to engrave the pitches D4 A3 C5 A6 C5 D5 E5 F5, the input is:
::

  {
    d'4 a c'' a'''
    c'' d'' e'' f''
  }

Relative mode
-------------

In relative mode, LilyPond calculates the octave of a pitch in a different way. It determines the octave of a pitch by choosing the pitch that is the closest in height to the preceding one. In other words, it selects the octave so that the interval between two following pitches is less than a fifth. Then, it raises or lowers the octave according to the ``'`` or ``,`` marks, if present.

Relative mode is used as follows:
::

  \relative c' {
    d4 a c' a''
    c,, d e f
  }

This example will engrave D4 A3 C5 A6 C5 D5 E5 F5, the same pitches as the previous example. It's easy to see how the relative block makes syntax easier, avoiding multiple sets of ``'`` or ``,``. Note that the octave of the first pitch after the brace, in this case a ``d``, is calculated by taking into account the pitch stated after the command ``\relative``, in this case ``c'`` (which serves only as a reference pitch, and thus is not engraved).

In Western music, especially vocal classical music, where big intervals are rare, ``\relative`` is often more comfortable to write than the default absolute mode. 
Reducing the amount of ``'`` and ``,``, ``\relative`` contributes to keeping the code clean. However, there are indeed circumstances in which the contrary is true, and absolute mode is preferable (for example, complex polyphonic music).

Durations
^^^^^^^^^

To define the duration of notes, LilyPond uses numbers and dots. Numbers have to be placed immediately after the note name or the octave mark, if present. The duration of notes is commonly expressed in fractions, and in LilyPond the reciprocal value of that fraction is used to specify the length of a note.  For example, a half note C, whose numerical duration can be expressed as 1/2, in LilyPond is written as ``c2``. Numbers from ``1`` to ``1024`` are available, although in Western classical and modern music values above 128 are rarely used. Values longer than a whole note can be expressed with ``\breve`` and ``\longa`` commands attached to the note name.

Dots
----

Dots can be used to lengthen a note duration by its half. They have to be placed immediately after the duration number. Multiple dots can be placed on the same note, lengthening its duration recursively by a half.

Omissions
---------

While it is true that a musical token needs both pitch and duration, it is common practice to omit one of them. A token whose duration is not explicitly stated inherits the previous token duration. In this way, a series of notes whose duration is the same can be written just by specifying their name. This can be seen in the two previous examples. There, only the first note has its duration explicitly stated (the ``d4``), and the following notes have the same duration. Note that dots apply to an explicitly stated duration only, therefore it is not possible to attach a dot directly to a note name. That is, ``c4 c c c.`` will produce a compilation error. The correct way to write these four notes is ``c4 c c c4.``.

It is also possible to omit the note name, and write exclusively the duration. This is helpful when a particular pitch is repeated multiple times, as shown in the following example:
::

  {
    c4 4 4 4
    d8 8 8 8 8 8 8 8
  }

Here a C3 quarter note is repeated four times, then a D3 eighth note is repeated eight times.

There are no strict rules about whether and when to use omissions, but it is good practice to stay consistent and favor code clarity. Consider the following example:
::

  {
    c8 d8 8 d e f8 8 f  
    c8 d d d e f f f 
  }

These two lines contain exactly the same notes, both in pitch and duration. The first one has inconsistent omissions and, while LilyPond wouldn't have any particular problem parsing it, to a human reader it can be unclear and harder to understand than the second one.

For more details, see `Duration syntax <duration-syntax.rst>`_.

