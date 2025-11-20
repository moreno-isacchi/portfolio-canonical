***********************
Engrave your first song
***********************

This tutorial is a step-by-step guide to engrave "Frère Jacques", a French public domain song. You will learn how to set up a minimal LilyPond file, write the melody, add the lyrics, and eventually compile your file to obtain a ``.pdf`` file.

Prepare the score
^^^^^^^^^^^^^^^^^

Create a new file and add the following minimal structure. This will set up the melody and lyrics sections.
::

  \version "2.24.3"

  \score {
    <<
      \new Staff \relative c' {

      }
      \addlyrics {
    
      }
    >>
  }

Add clef, key and time signature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Just after the ``\new Staff \relative c' {`` line, add these three lines to set the treble clef, the key and the time signature.
::

  \clef treble
  \key g \major
  \time 4/4

Insert the melody
^^^^^^^^^^^^^^^^^

Now you can insert actual music in your score. Just after the ``\key g \major`` line, add these lines, corresponding to the melody.
::

  g'4 a b g |
  g a b g |
  b c d2 |
  b4 c d2 |
  d8 e d c b4 g |
  d'8 e d c b4 g |
  g d g2 |
  g4 d g2 \bar "|."

Add the lyrics
^^^^^^^^^^^^^^

In the lyrics block (immediately after ``\addlyrics {``), copy the lyrics of the song, already hyphenated to match the melody.
::

  Frè -- re Jac -- ques,
  Frè -- re Jac -- ques,
  Dor -- mez vous,
  Dor -- mez vous,
  Son -- nez les ma -- ti -- nes,
  Son -- nez les ma -- ti -- nes,
  Ding, dang, dong,
  Ding, dang, dong.

Compile your file
^^^^^^^^^^^^^^^^^

The source file is now finished. To compile your ``.ly`` file, open a terminal and type:
::

  lilypond <your-source-file.ly>
 
The terminal will then show a few lines about the ongoing compilation process, which will produce a ``.pdf`` file in the same directory as your source file.

You have now engraved your first LilyPond score.