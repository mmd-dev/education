# Preface

The first thing users of the UNIX or Linux operating
systems come face to face with is the _shell_. "Shell" is the
UNIX term for a user interface to the system—something
that lets you communicate with the computer via the
keyboard and the display. Shells are just separate
programs that encapsulate the system, and, as such, there
are many to choose from.

Systems are usually set up with a "standard" shell that
new users adopt without question. However, some of
these standard shells are rather old and lack many
features of the newer shells. This is a shame, because
shells have a large bearing on your working environment.
Since changing shells is as easy as changing hats, there is
no reason not to change to the latest and greatest in shell
technology.

Of the many shells to choose from, this book introduces
the Bourne Again shell ( _bash_ for short), a modern
general-purpose shell. Other useful modern shells are the
Korn shell ( _ksh_ ) and the "Tenex C shell" ( _tcsh_ ); both are
also the subjects of O'Reilly handbooks.


# bash Versions

This book is relevant to all versions of _bash_ , although
older versions lack some of the features of the most
recent version.[1]You can easily find out which version
you are using by typing **echo $BASH_VERSION**. The
earliest public version of _bash_ was 1.0, and the most
recent is 3.0 (released in July 2004). If you have an older
version, you might like to upgrade to the latest one.
Chapter 12shows you how to go about it.

[1]Throughout this book we have clearly marked with

footnotes the features that are not present in the earlier
versions.


# Summary of bash

# Features

_bash_ is a backward-compatible evolutionary successor to
the Bourne shell that includes most of the C shell's major
advantages as well as features from the Korn shell and a
few new features of its own. Features appropriated from
the C shell include:

- Directory manipulation, with the **pushd** , **popd** ,
    and **dirs** commands.
- Job control, including the **fg** and **bg** commands
    and the ability to stop jobs with CTRL-Z.
- Brace expansion, for generating arbitrary strings.
- Tilde expansion, a shorthand way to refer to
    directories.
- Aliases, which allow you to define shorthand
    names for commands or command lines.
- Command history, which lets you recall
    previously entered commands.

_bash_ 's major new features include:


- Command-line editing, allowing you to use _vi_ - or
    _emacs_ -style editing commands on your command
    lines.
- Key bindings that allow you to set up customized
    editing key sequences.
- Integrated programming features: the
    functionality of several external UNIX
    commands, including _test_ , _expr_ , _getopt_ , and _echo_ ,
    has been integrated into the shell itself, enabling
    common programming tasks to be done more
    cleanly and efficiently.
- Control structures, especially the **select** construct,
    which enables easy menu generation.
- New options and variables that give you more
    ways to customize your environment.
- One dimensional arrays that allow easy
    referencing and manipulation of lists of data.
- Dynamic loading of built-ins, plus the ability to
    write your own and load them into the running
    shell.


# Intended Audience

This book is designed to address casual UNIX and Linux
users who are just above the "raw beginner" level. You
should be familiar with the process of logging in, entering
commands, and doing simple things with files. Although
Chapter 1reviews concepts such as the tree-like file and
directory scheme, you may find that it moves too quickly
if you're a complete neophyte. In that case, we
recommend the O'Reilly handbook, _Learning the UNIX
Operating System_ , by Jerry Peek, Grace Todino, and John
Strang.

If you're an experienced user, you may wish to skip
Chapter 1altogether. But if your experience is with the C
shell, you may find thatChapter 1reveals a few subtle
differences between the _bash_ and C shells.

No matter what your level of experience is, you will
undoubtedly learn many things in this book that will
make you a more productive _bash_ user—from major
features down to details at the "nook-and-cranny" level
that you may not have been aware of.

If you are interested in shell programming (writing shell
_scripts_ and _functions_ that automate everyday tasks or
serve as system utilities), you should also find this book
useful. However, we have deliberately avoided drawing a
strong distinction between interactive shell use (entering


commands during a login session) and shell
programming. We see shell programming as a natural,
inevitable outgrowth of increasing experience as a user.

Accordingly, each chapter depends on those previous to
it, and although the first three chapters are oriented
toward interactive use only, subsequent chapters describe
interactive, user-oriented features in addition to
programming concepts.

This book aims to show you that writing useful shell
programs doesn't require a computing degree. Even if you
are completely new to computing, there is no reason why
you shouldn't be able to harness the power of _bash_ within
a short time.

Toward that end, we decided not to spend too much time
on features of exclusive interest to low-level systems
programmers. Concepts like file descriptors and special
file types might only confuse the casual user, and
anyway, we figure those of you who understand such
things are smart enough to extrapolate the necessary
information from our cursory discussions.


# Code Examples

This book is full of examples of shell commands and
programs designed to be useful in your everyday life as a
user, not just to illustrate the feature being explained. In
Chapter 4and onwards, we include various programming
problems, which we call _tasks_ , that illustrate particular
shell programming concepts. Some tasks have solutions
that are refined in subsequent chapters. The later chapters
also include programming exercises, many of which build
on the tasks in the chapter.

Feel free to use any code you see in this book and to pass
it along to friends and colleagues. We especially
encourage you to modify and enhance it yourself.

If you want to try examples but you don't use _bash_ as
your login shell, you must put the following line at the
top of each shell script:

```
#!/bin/bash
```
If _bash_ isn't installed as the file _/bin/bash_ , substitute its
pathname in the above.


# Chapter Summary

If you want to investigate specific topics rather than read
the entire book through, here is a chapter-by-chapter
summary:

Chapter 1introduces _bash_ and tells you how to install it
as your login shell. Then it surveys the basics of
interactive shell use, including overviews of the UNIX
file and directory scheme, standard I/O, and background
jobs.

Chapter 2 discusses the shell's command history
mechanism (including the _emacs_ - and _vi_ -editing modes),
history substitution and the **fc** history command, and key
bindings with _readline_ and **bind**.

Chapter 3 covers ways to customize your shell
environment without programming by using the startup
and environment files. Aliases, options, and shell
variables are the customization techniques discussed.

Chapter 4 is an introduction to shell programming. It
explains the basics of shell scripts and functions, and
discusses several important "nuts-and-bolts"
programming features: string manipulation operators,
brace expansion, command-line arguments (positional
parameters), and command substitution.


Chapter 5continues the discussion of shell programming
by describing command exit status, conditional
expressions, and the shell's flow-control structures: **if** ,
**for** , **case** , **select** , **while** , and **until**.

Chapter 6goes into depth about positional parameters and
command-line option processing, then discusses special
types and properties of variables, integer arithmetic, and
arrays.

Chapter 7gives a detailed description of _bash_ I/O. This
chapter covers all of the shell's I/O redirectors, as well as
the line-at-a-time I/O commands **read** and **echo**. It also
discusses the shell's command-line processing mechanism
and the **eval** command.

Chapter 8covers process-related issues in detail. It starts
with a discussion of job control, then gets into various
low-level information about processes, including process
IDs, signals, and traps. The chapter then moves to a
higher level of abstraction to discuss coroutines and
subshells.

Chapter 9 discusses various debugging techniques, like
trace and verbose modes, and the "fake" signal traps. It
then presents in detail a useful shell tool, written using
the shell itself: a _bash_ debugger.

Chapter 10gives information for system administrators,
including techniques for implementing system-wide shell
customization and features related to system security.


Chapter 11 discusses ways to make _bash_ scripts more
maintainable.

Chapter 12shows you how to go about getting _bash_ and
how to install it on your system. It also outlines what to
do in the event of problems along the way.

Appendix A compares _bash_ to several similar shells,
including the standard Bourne shell, the POSIX shell
standard, the Korn shell ( _ksh_ ), the public-domain Korn
shell ( _pdksh_ ), and the Z Shell ( _zsh_ ).

Appendix B contains lists of shell invocation options,
built-in commands, built-in variables, conditional test
operators, options, I/O redirection, and _emacs-_ and
_vi_ -editing mode commands.

Appendix Cgives information on writing and compiling
your own loadable built-ins.

Appendix D looks at the basics of programmable
completion.


# Conventions Used in

# This Handbook

We leave it as understood that when you enter a shell
command, you press RETURN at the end. RETURN is
labeled ENTER on some keyboards.

Characters called CTRL- _X_ , where _X_ is any letter, are
entered by holding down the CTRL (or CTL, or
CONTROL) key and pressing that letter. Although we
give the letter in uppercase, you can press the letter
without the SHIFT key.

Other special characters are LINEFEED (which is the
same as CTRL-J), BACKSPACE (same as CTRL-H),
ESC, TAB, and DEL (sometimes labeled DELETE or
RUBOUT).

This book uses the following font conventions:

**_Italic_**

```
Used for UNIX filenames, commands not built into
the shell (which are files anyway), and shell
functions. Italic is also used for dummy parameters
that should be replaced with an actual value, to
distinguish the vi and emacs programs from their
bash modes, and to highlight special terms the first
time they are defined.
```

**Bold**

```
Used for bash built-in commands, aliases, variables,
and options, as well as command lines when they are
within regular text. Bold is used for all elements
typed in by the user within regular text.
```
**Constant Width**

```
Used in examples to show the contents of files or the
output from commands.
```
**Constant Bold**

```
Used in examples to show interaction between the
user and the shell; any text the user types in is shown
in Constant Bold. For example:$ pwd/home/
cam/adventure/carrol $
```
**_Constant Italic_**

```
Used in displayed command lines for dummy
parameters that should be replaced with an actual
value.
```
**Square Brackets**

```
Used inChapter 2to show the position of the cursor
on the command line being edited. For
example:grep -l Alice < ~cam/book/
[a]iw
```

We use UNIX as a shorthand for "UNIX and Linux."
Purists will correctly insist that Linux is not
UNIX—but as far as this book is concerned, they
behave identically.


# We'd Like to Hear from

# You

Please address comments and questions concerning this
book to the publisher:

O'Reilly Media, Inc.
1005 Gravenstein Highway North
Sebastopol, CA 95472
(800) 998-9938 (in the United States or Canada)
(707) 829-0515 (international or local)
(707) 829-0104 (fax)

We have a web page for this book, where we list errata,
examples, and any additional information. You can
access this page at:

[http://www.oreilly.com/catalog/bash3](http://www.oreilly.com/catalog/bash3)

To comment or ask technical questions about this book,
send email to:

bookquestions@oreilly.com

For more information about our books, conferences,
Resource Centers, and the O'Reilly Network, see our web
site at:

[http://www.oreilly.com](http://www.oreilly.com)


# Using Code Examples

This book is here to help you get your job done. In
general, you may use the code in this book in your
programs and documentation. You do not need to contact
us for permission unless you're reproducing a significant
portion of the code. For example, writing a program that
uses several chunks of code from this book does not
require permission. Selling or distributing a CD-ROM of
examples from O'Reilly books _does_ require permission.
Answering a question by citing this book and quoting
example code does not require permission. Incorporating
a significant amount of example code from this book into
your product's documentation _does_ require permission.

We appreciate, but do not require, attribution. An
attribution usually includes the title, author, publisher,
and ISBN. For example " _Learning the bash Shell_ , Third
Edition, by Cameron Newham and Bill Rosenblatt.
Copyright 2005 O'Reilly Media, Inc., 0-596-00965-8."


# Safari Enabled

When you see a Safari® Enabled icon on the cover of
your favorite technology book, that means the book is
available online through the O'Reilly Network Safari
Bookshelf.

Safari offers a solution that's better than e-books. It's a
virtual library that lets you easily search thousands of top
tech books, cut and paste code samples, download
chapters, and find quick answers when you need the most
accurate, current information. Try it for free at
[http://safari.oreilly.com.](http://safari.oreilly.com.)


# Acknowledgments for

# the First Edition

This project has been an interesting experience and
wouldn't have been possible without the help of a number
of people. Firstly, I'd like to thank Brian Fox and Chet
Ramey for creating _bash_ and making it the polished
product it is today. Thanks also to Chet Ramey for
promptly answering all of my questions on _bash_ and
pointing out my errors.

Many thanks to Bill Rosenblatt for _Learning the korn
Shell_ , on which this book is based; Michael O'Reilly and
Michael Malone at iiNet Technologies for their useful
comments and suggestions (and my net.connection!);
Chris Thorne, Justin Twiss, David Quin-Conroy, and my
mum for their comments, suggestions, and corrections;
Linus Torvalds for the Linux operating system which
introduced me to _bash_ and was the platform for all of my
work on the book; Brian Fox for providing a short history
of _bash_ ; David Korn for information on the latest Korn
shell. Thanks also to Depeche Mode for "101" as a
backdrop while I worked, Laurence Durbridge for being a
likable pest and never failing to ask "Finished the book
yet?" and Adam (for being in my book).

The sharp eyes of our technical reviewers picked up
many mistakes. Thanks to Matt Healy, Chet Ramey, Bill


Reynolds, Bill Rosenblatt, and Norm Walsh for taking
time out to go through the manuscript.

The crew at O'Reilly were indispensable in getting this
book out the door. I'd like to thank Lenny Muellner for
providing me with the formatting tools for the job, Chris
Reilley for the figures, and Edie Freedman for the cover
design. On the production end, I'd like to thank David
Sewell for his copyediting, Clairemarie Fisher O'Leary
for managing the production process, Michael Deutsch
and Jane Ellin for their production assistance, Ellen
Siever for tools support, Kismet McDonough for
providing quality assurance, and Seth Maislin for the
index.

I'm grateful to Frank Willison for taking me up on my
first piece of email to ORA: "What about a book on
_bash_ ?"

Last but by no means least, a big thank you to my editor,
Mike Loukides, who helped steer me through this project.


# Acknowledgments for

# the Second Edition

Thanks to all the people at O'Reilly. Gigi Estabrook was
the editor for the second edition. Nicole Gipson Arigo
was the production editor and project manager. Nancy
Wolfe Kotary and Ellie Fountain Maden performed
quality control checks. Seth Maislin wrote the index. Edie
Freedman designed the cover, and Nancy Priest designed
the interior format of the book. Lenny Muellner
implemented the format in troff. Robert Romano updated
the illustrations for the second edition.


# Acknowledgments for

# the Third Edition

Thanks to the production people at O'Reilly and to the
indexer.

Thanks to Chet Ramey for once again swiftly answering
my queries on _bash_ and for providing helpful comments
on the book. I'd also like to thank Ian Macdonald for his
feedback on Programmable Completion.
