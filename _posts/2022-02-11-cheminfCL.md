---
title:  "Cheminformatics in the Command Line: Ampersands"
---
###  Not a logical operator, but a control operator

Lately in my PhD research, I have been using [SMARTS notation](https://www.daylight.com/dayhtml/doc/theory/theory.smarts.html) within queries for substructure matching.

In SMARTS, the ampersand '&' character is used as a **logical operator** to form expressions combining so-called atom and bond primitives.

Put simply: an ampersand combines properties specified for an atom or bond into one expression.  

For a very simple example, let's say I wanted to find all C's bonded to a specific number of hydrogen atoms. I would express my query as the following SMARTS string:

 `[#6&Hx]`

 where x is the no. of hydrogen atoms.

{% gist 58eaed597a54d0fb78878fdd6b2a1c94 %}

All well and good while prototyping code in Jupyter notebooks.

However I started executing my scripts in the command line using Python's `argparse` module, and I wanted to specify SMARTS strings as inputs.

These SMARTS contain ampersands. In my haste, I ran it in the CL like this:

```
$ python myscript.py -smarts [#6&H2]

> -bash: H2]-: command not found
```

Oops...

Of course, the ampersand has its own meaning in `bash`. More specifically, it is a **control operator** for job execution, as explained in the [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.pdf):

>"If a command is terminated by the control operator ‘&’, the shell executes the command
asynchronously in a subshell. This is known as executing the command in the background,
and these are referred to as asynchronous commands."

The very simple solution of course is to simply add quotation marks around the SMARTS string CL input:

```
$ python myscript.py -smarts '[#6&H2]'
```

A timely reminder to get more coffee...

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fadelenel.ai%2FcheminfCL%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
