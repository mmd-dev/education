# Chapter 10. bash

# Administration

There are two areas in which system administrators use
the shell as part of their job: setting up a generic
environment for users and for system security. In this
chapter, we'll discuss _bash_ 's features that relate to these
tasks. We assume that you already know the basics of
UNIX system administration.[1]

# Installing bash as the

# Standard Shell

As a prelude to system-wide customization, we want to
emphasize that _bash_ can be installed as if it were the
standard Bourne shell, _/bin/sh_. Indeed, some systems,
such as Linux, come with _bash_ installed instead of the
Bourne shell.

If you want to do this with your system, you can just save
the original Bourne shell to another filename (in case
someone needs to use it) and either install _bash_ as _sh_ in
the _/bin_ directory, or better yet install _bash_ in the _/bin_
directory and create a symbolic link from _/bin/sh_ to _/bin/
bash_ using the command **ln -s /bin/bash /bin/sh**. The
reason we think that the second option is better is because


_bash_ changes its behavior slightly if started as _sh_ , as we
will see shortly.

As detailed inAppendix A, _bash_ is backward-compatible
with the Bourne shell, except that it doesn't support **^** as a
synonym for the pipe character ( **|)**. Unless you have an
ancient UNIX system, or you have some very, very old
shell scripts, you needn't worry about this.

But if you want to be absolutely sure, simply search
through all shell scripts in all directories in your **PATH**.
An easy way to perform the search is to use the _file_
command, which we saw inChapter 5andChapter 9. _file_
prints "executable shell script" when given the name of
one.[2]Here is a script that looks for **^** in shell scripts in
every directory in your **PATH** :

```
IFS=:
for d in $PATH; do
echo checking $d:
cd $d
scripts=$(file * | grep 'shell script' | cut -d: -f1)
for f in $scripts; do
grep '\^' $f /dev/null
done
done
```
The first line of this script makes it possible to use
**$PATH** as an item list in the **for** loop. For each directory,
it **cd** s there and finds all shell scripts by piping the _file_
command into _grep_ and then, to extract the filename only,
into _cut_. Then for each shell script, it searches for the **^**
character.[3]


If you run this script, you will probably find several
occurrences of **^—** but these carets should be used within
regular expressions in _grep_ , _sed_ , or _awk_ commands, not
as pipe characters. As long as carets are never used as
pipes, it is safe for you to install _bash_ as _/bin/sh_.

As we mentioned earlier, if _bash_ is started as _sh_ (because
the executable file has been renamed _sh_ or there is a link
from _sh_ to _bash_ ) its startup behavior will change slightly
to mimic the Bourne shell as closely as possible. For
login shells it only attempts to read _/etc/profile_ and
_~/.profile_ , ignoring any other startup files like
_~/.bash_profile_. For interactive shells it won't read the
initialization file _~/.bashrc_ .[4]

## POSIX Mode

Besides its native operating mode, _bash_ can also be
switched into POSIX mode. The POSIX (Portable
Operating System Interface) standard, described in detail
in Appendix A, defines guidelines for standardizing
UNIX. One part of the POSIX standard covers shells.

_bash_ is nearly 100% POSIX-compliant in its native mode.
If you want strict POSIX adherence, you can either start
_bash_ with the **—posix** option, or set it from within the
shell with **set -o posix**.

Only in very rare circumstances would you ever have to
use POSIX mode. The differences, outlined inAppendix


A, are small and are mostly concerned with the command
lookup order and how functions are handled. Most _bash_
users should be able to get through life without ever
having to use this option.


## Command-Line Options

_bash_ has several command-line options that change the
behavior of and pass information to the shell. The options
fall into two sets: single character options, like we've seen
in previous chapters of this book, and _multicharacter_
options, which are a relatively recent improvement to
UNIX utilities.[5]Table 10-1lists all of the options.[6]

**Table 10-1. bash command-line options**

```
Option Meaning
```
```
-c string
```
```
Commands are read from string , if
present. Any arguments after string
are interpreted as positional
parameters, starting with $0.
```
#### -D

```
A list of all double-quoted strings
preceded by $ is printed on the
standard ouput. These are the
strings that are subject to language
translation when the current locale
```

**Option Meaning**

```
is not C or POSIX. This also turns
on the -n option.
```
-i

```
Interactive shell. Ignores signals
TERM, INT, and QUIT. With job
control in effect, TTIN, TTOU,
and TSTP are also ignored.
```
-l
Makes _bash_ act as if invoked as a
login shell.

-o _option_
Takes the same arguments as **set
-o**.

#### -O, +O

_shopt-option_

```
shopt-option is one of the shell
options accepted by the shopt
builtin. If shopt-option is present,
-O sets the value of that option;
+O unsets it. If shopt-option is not
supplied, the names and values of
the shell options accepted by shopt
are printed on the standard output.
```

**Option Meaning**

```
If the invocation option is +O , the
output is displayed in a format that
may be reused as input.
```
-s

```
Reads commands from the
standard input. If an argument is
given to bash , this flag takes
precedence (i.e., the argument
won't be treated as a script name
and standard input will be read).
```
-r
Restricted shell. See the Section
10.3.1later in this chapter.

-v
Prints shell input lines as they're
read.

#### -

```
Signals the end of options and
disables further option processing.
Any options after this are treated as
filenames and arguments. — is
synonymous with -.
```

**Option Meaning**

—debugger

```
Arranges for the debugger profile
to be executed before the shell
starts. Turns on extended
debugging mode and shell function
tracing.[7]
```
—dump-strings Does the same as **-D**.

—dump-po-strings

```
Does the same as - D but the output
is in the GNU gettext po (portable
object) file format.
```
—help
Displays a usage message and
exits.

—login
Makes _bash_ act as if invoked as a
login shell. Same as - **l**.

—noediting

```
Does not use the GNU readline
library to read command lines if
interactive.
```

**Option Meaning**

—noprofile

```
Does not read the startup file /etc/
profile or any of the personal
initialization files.
```
—norc

```
Does not read the initialization file
~/.bashrc if the shell is interactive.
This is on by default if the shell is
invoked as sh.
```
—posix

```
Changes the behavior of bash to
follow the POSIX guidelines more
closely where the default operation
of bash is different.
```
—quiet
Shows no information on shell
startup. This is the default.

—rcfile _file_ ,
—init-file _file_

```
Executes commands read from file
instead of from the initialization
file ~/.bashrc if the shell is
interactive.
```

```
Option Meaning
```
```
—verbose Equivalent to -v.
```
```
—version
Shows the version number of this
instance of bash and then exits.
```
```
[7]Only available in bash version 3.0 and later.
```
The multicharacter options have to appear on the
command line before the single-character options. In
addition to these, any **set** option can be used on the
command line. Like shell built-ins, using a **+** instead of **-**
turns an option off.

Of these options, the most useful are **-i** (interactive), **-r**
(restricted), **-s** (read from standard input), **-p** (privileged),
and **-m** (enable job control). Login shells are usually run
with the **-i** , **-s** , and **-m** flags. We'll look at restricted and
privileged modes later in this chapter.

[1] A good source of information on system

administration is _Essential System Administration_ by
Æleen Frisch (O'Reilly ).


[2] The exact message varies from system to system;

make sure that yours prints this message when given the
name of a shell script. If not, just substitute the message
your _file_ command prints for "shell script" in the
following code.

[3]The inclusion of _/dev/null_ in the _grep_ command is a

kludge that forces _grep_ to print the names of files that
contain a match, even if there is only one such file in a
given directory.

[4] _bash_ also enters POSIX mode when started as _sh_.

Versions of _bash_ prior to 2.0 don't—POSIX mode has to
be explicitly set with the **—posix** command-line option.

[5] Multicharacter options are far more readable and

easier to remember than the old, and usually cryptic,
single character options. All of the GNU utilities have
multicharacter options, but many applications and utilities
(certainly those on old UNIX systems) allow only
single-character options.

[6]SeeAppendix Bfor a list of options for versions of

_bash_ prior to 2.0.


# Environment

# Customization

Like the Bourne shell, _bash_ uses the file _/etc/profile_ for
system-wide customization. When a user logs in, the shell
reads and runs _/etc/profile_ before running the user's
_.bash_profile_.

We won't cover all the possible commands you might
want to put in _/etc/profile_. But _bash_ has a few unique
features that are particularly relevant to system-wide
customization; we'll discuss them here.

We'll start with two built-in commands that you can use
in _/etc/profile_ to tailor your users' environments and
constrain their use of system resources. Users can also
use these commands in their _.bash_profile_ , or at any other
time, to override the default settings.

## umask

**umask** , like the same command in most other shells, lets
you specify the default permissions that files have when
users create them. It takes the same types of arguments
that the _chmod_ command does, i.e., absolute (octal
numbers) or symbolic permission values.


The **umask** contains the permissions that are turned off
by default whenever a process creates a file, regardless of
what permission the process specifies.[8]

We'll use octal notation to show how this works. As you
probably know, the digits in a permission number stand
(left to right) for the permissions of the owner, owner's
group, and all other users, respectively. Each digit, in
turn, consists of three bits, which specify read, write, and
execute permissions from left to right. (If a file is a
directory, the "execute" permission becomes "search"
permission, i.e., permission to **cd** to it, list its files, etc.)

For example, the octal number 640 equals the binary
number 110 100 000. If a file has this permission, then its
owner can read and write it; users in the owner's group
can only read it; everyone else has no permission on it. A
file with permission 755 gives its owner the right to read,
write, and execute it and everyone else the right to read
and execute (but not write).

022 is a common **umask** value. This implies that when a
file is created, the "most" permission it could possibly
have is 755—which is the usual permission of an
executable that a compiler might create. A text editor, on
the other hand, might create a file with 666 permission
(read and write for everyone), but the **umask** forces it to
be 644 instead.


## ulimit

The **ulimit** command was originally used to specify the
limit on file creation size. But _bash_ 's version has options
that let you put limits on several different system
resources.Table 10-2lists the options.

**Table 10-2. ulimit resource options**

```
Option Resource limited
```
```
-a All limits (for printing values only)
```
```
-c Core file size (1 Kb blocks)
```
```
-d Process data segment (Kb)
```
```
-f File size (1 Kb blocks)
```
```
-l
Maximum size of a process that can be locked
in memory (Kb)[9]
```

**Option Resource limited**

-m Maximum resident set size

-n File descriptors

-p Pipe size (512 byte blocks)

-s Process stack segment (Kb)

-t Process CPU time (seconds)

-u
Maximum number of processes available to a
user

-v Virtual memory (Kb)

[9]Not available in versions of _bash_ prior to 2.0.


Each takes a numerical argument that specifies the limit
in units shown in the table. You can also give the
argument "unlimited" (which may actually mean some
physical limit), "hard" and "soft", which refer to the
current hard and soft limits (see below), or you can omit
the argument, in which case it will print the current limit.
**ulimit -a** prints limits (or "unlimited") of all types.[10]
You can specify only one type of resource at a time. If
you don't specify any option, **-f** is assumed.

Some of these options depend on operating system
capabilities that don't exist in older UNIX versions. In
particular, some older versions have a fixed limit of 20
file descriptors per process (making **-n** irrelevant), and
some don't support virtual memory (making **-v**
irrelevant).

The **-d** and **-s** options have to do with _dynamic memory
allocation_ , i.e., memory for which a process asks the
operating system at runtime. It's not necessary for casual
users to limit these, though software developers may want
to do so to prevent buggy programs from trying to
allocate endless amounts of memory due to infinite loops.

The **-v** and **-m** options are similar; **-v** puts a limit on all
uses of memory, and **-m** limits the amount of physical
memory that a process is allowed to use. You don't need
these unless your system has severe memory constraints
or you want to limit process size to avoid thrashing.


The **-u** option is another option which is useful if you
have system memory constraints or you wish just wish to
stop individual users from hogging the system resources.

You may want to specify limits on file size ( **-f** and **-c** ) if
you have constraints on disk space. Sometimes users
actually mean to create huge files, but more often than
not, a huge file is the result of a buggy program that goes
into an infinite loop. Software developers who use
debuggers like _sdb_ , _dbx_ , and _gdb_ should not limit core file
size, because core dumps are necessary for debugging.

The **-t** option is another possible guard against infinite
loops. However, a program that is in an infinite loop but
isn't allocating memory or writing files is not particularly
dangerous; it's better to leave this unlimited and just let
the user kill the offending program.

In addition to the types of resources you can limit, **ulimit**
lets you specify hard or soft limits. Hard limits can be
lowered by any user but only raised by the super user
( **root** ); users can lower soft limits and raise them—but
only as high as the hard limit for that resource.

If you give **-H** along with one (or more) of the options
above, **ulimit** will set hard limits; **-S** sets soft limits.
Without either of these, **ulimit** sets the hard and soft
limit. For example, the following commands set the soft
limit on file descriptors to 64 and the hard limit to
unlimited:


```
ulimit -Sn 64
ulimit -Hn unlimited
```
When **ulimit** prints current limits, it prints soft limits
unless you specify **-H**.


## Types of Global

## Customization

The best possible approach to globally available
customization would be a system-wide environment file
that is separate from each user's environment file—just
like _/etc/profile_ is separate from each user's _.bash_profile_.
Unfortunately, _bash_ doesn't have this feature.

Nevertheless, the shell gives you a few ways to set up
customizations that are available to all users at all times.
Environment variables are the most obvious; your _/etc/
profile_ file will undoubtedly contain definitions for
several of them, including **PATH** and **TERM**.

The variable **TMOUT** is useful when your system
supports dialup lines. Set it to a number _N_ , and if a user
doesn't enter a command within _N_ seconds after the shell
last issued a prompt, the shell will terminate. This feature
is helpful in preventing people from "hogging" the dialup
lines.

You may want to include some more complex
customizations involving environment variables, such as
the prompt string **PS1** containing the current directory (as
seen inChapter 4).


You can also turn on options, such as _emacs_ or _vi_ editing
modes, or **noclobber** to protect against inadvertent file
overwriting. Any shell scripts you have written for
general use also contribute to customization.

Unfortunately, it's not possible to create a global alias.
You can define aliases in _/etc/profile_ , but there is no way
to make them part of the environment so that their
definitions will propagate to subshells. (In contrast, users
can define global aliases by putting their definitions in
_~/.bashrc_ .)

However, you can set up global functions. These are an
excellent way to customize your system's environment,
because functions are part of the shell, not separate
processes.

[8]If you are comfortable with Boolean logic, think of the

**umask** as a number that the operating system logically
ANDs with the permission given by the creating process.

[10]The "hard" and "soft" arguments are not available in

_bash_ prior to version 2.05a.


# System Security

# Features

UNIX security is a problem of legendary notoriety. Just
about every aspect of a UNIX system has some security
issue associated with it, and it's usually the system
administrator's job to worry about this issue.

_bash_ has two features that help solve this problem: the
_restricted shell_ , which is intentionally "brain damaged,"
and _privileged mode_ , which is used with shell scripts that
run as if the user were **root**.

## Restricted Shell

The restricted shell is designed to put the user into an
environment where her ability to move around and write
files is severely limited. It's usually used for "guest"
accounts.[11]You can make a user's login shell restricted
by putting **rbash** in the user's _/etc/passwd_ entry.[12]

The specific constraints imposed by the restricted shell
disallow the user from doing the following:

- Changing working directories: **cd** is inoperative.
    If you try to use it, you will get the error message
    **bash: cd: restricted**.


- Redirecting output to a file: the redirectors **>** , **>|** ,
    **<>** , and **>>** are not allowed.
- Assigning a new value to the environment
    variables **ENV** , **BASH_ENV** , **SHELL** , or
    **PATH**.
- Specifying any commands with slashes (/) in
    them. The shell will treat files outside of the
    current directory as "not found."
- Using the **exec** built-in.
- Specifying a filename containing a **/** as an
    argument to the. built-in command.
- Importing function definitions from the shell
    environment at startup.
- Adding or deleting built-in commands with the **-f**
    and **-d** options to the **enable** built-in command.
- Specifying the **-p** option to the **builtin** command.
- Turning off restricted mode with **set +r**.

These restrictions go into effect after the user's
_.bash_profile_ and environment files are run. In addition, it
is wise to change the owner of the users' _.bash_profile_
and _.bashrc_ to root, and make these files read-only. The
users' home directory should also be made read-only.


This means that the restricted shell user's entire
environment is set up in _/etc/profile_ and _.bash_profile_.
Since the user can't access _/etc/profile_ and can't overwrite
_.bash_profile_ , this lets the system administrator configure
the environment as he sees fit.

Two common ways of setting up such environments are
to set up a directory of "safe" commands and have that
directory be the only one in **PATH** , and to set up a
command menu from which the user can't escape without
exiting the shell.


## A System Break-In Scenario

Before we explain the other security features, here is
some background information on system security that
should help you understand why they are necessary.

Many problems with UNIX security hinge on a UNIX file
attribute called the _suid_ (set user ID) bit. This is like a
permission bit (see **umask** earlier in this chapter): when
an executable file has it turned on, the file runs with an
effective user ID equal to the owner of the file, which is
usually **root**. The effective user ID is distinct from the
real user ID of the process.

This feature lets administrators write scripts that do
certain things that require **root** privilege (e.g., configure
printers) in a controlled way. To set a file's _suid_ bit, the
superuser can type **chmod 4755** _filename_ ; the **4** is the
_suid_ bit.

Modern system administration wisdom says that creating
_suid_ shell scripts is a very, very bad idea.[13]This has
been especially true under the C shell, because its _.cshrc_
environment file introduces numerous opportunities for
break-ins. _bash_ 's environment file feature creates similar
security holes, although the security feature we'll see
shortly make this problem less severe.


We'll show why it's dangerous to set a script's _suid_ bit.
Recall that inChapter 3, we mentioned that it's not a good
idea to put your personal _bin_ directory at the front of your
**PATH**. Here is a scenario that shows how this placement
combines with _suid_ shell scripts to form a security hole: a
variation of the infamous "Trojan horse" scheme. First,
the computer cracker has to find a user on the system
with an _suid_ shell script. In addition, the user must have a
**PATH** with her personal _bin_ directory listed before the
public _bin_ directories, and the cracker must have write
permission on the user's personal _bin_ directory.

Once the cracker finds a user with these requirements, he
follows these steps:

- Looks at the _suid_ script and finds a common
    utility that it calls. Let's say it's grep.
- Creates the Trojan horse, which is this case is a
    shell script called grep in the user's personal bin
    directory. The script looks like this:

```
cp /bin/bash filename
chown root
filename
chmod 4755
filename
/bin/grep "$@
rm ~/bin/grep
```
```
filename should be some unremarkable filename
in a directory with public read and execute
permission, such as /bin or /usr/bin. The file,
```

```
when created, will be that most heinous of
security holes: an suid interactive shell.
```
- Sits back and waits for the user to run the suid
    shell script—which calls the Trojan horse, which
    in turn creates the suid shell and then
    self-destructs.
- Runs the _suid_ shell and creates havoc.


## Privileged Mode

The one way to protect against Trojan horses is _privileged
mode_. This is a **set -o** option ( **set -o privileged** or **set -p** ).

In privileged mode, when an _suid bash_ shell script is
invoked, the shell does not run the user's environment
file—i.e., it doesn't expand the user's **BASH_ENV**
environment variable.

Since privileged mode is an option, it is possible to turn it
off with the command **set +o privileged** (or **set +p** ). But
this doesn't help the potential system cracker: the shell
automatically changes its effective user ID to be the same
as the real user ID—i.e., if you turn off privileged mode,
you also turn off _suid_.

Privileged mode is an excellent security feature; it solves
a problem that originated when the environment file idea
first appeared in the C shell.

Nevertheless, we still strongly recommend against
creating _suid_ shell scripts. We have shown how _bash_
protects against break-ins in one particular situation, but
that certainly does not imply that _bash_ is "safe" in any
absolute sense. If you really must have _suid_ scripts, you
should carefully consider all relevant security issues.


Finally, if you would like to learn more about UNIX
security, we recommend _Practical UNIX and Internet
Security_ , by Gene Spafford and Simson Garfinkel
(O'Reilly ).

[11]This feature is not documented in the manual pages

for old versions of _bash_.

[12]If this option has been included when the shell was

compiled. SeeChapter 11for details on configuring _bash_.

[13]In fact, most versions of UNIX intentionally disable

the _suid_ feature for shell scripts.


# Chapter 11. Shell

# Scripting

For the majority of this book, we've looked at the various
elements that make up _bash_ and how you can use them in
writing shell scripts. If you've used other programming
languages you will know that there is a difference
between writing a piece of code that gets a job done and
writing a piece of code that does the job but is also
maintainable and conforms to what we could call "good
practice."

This chapter will give a brief introduction to some aspects
of good practice and writing maintainable shell scripts
along with helpful tips and tricks that you can use to
make writing scripts easier.

# What's That Do?

Six months ago you coded up a 100 line shell script. It
made perfect sense then, but now you look at it and
wonder, "Just what does that do?" This is a common
pitfall among programmers—especially those writing in a
shell language. Unfortunately, shells have developed with
more than their fair share of obscure punctuation. This is
a blessing for keeping typing to a minimum but doesn't


help readability. It's important to make your code as
readable as possible.

## Comments

The first rule of shell scripting is to _comment your code_.
You should do this right from the start, even if the script
is only a couple of lines long. Shell scripts have a habit of
growing from a couple of lines to many hundreds of lines
as more features are added, so it's best to get into the
habit of commenting your code right at the beginning.

To start with, consider having a main header or banner
for your scripts. The information in the header should, at
a minimum, say what the script does. Here is an example
of a script header:

```
#!/bin/bash
#####################################################
# Name: graphconv.sh
#
# Converts graphics files from one format to another.
#
# Usage: graphconv.sh <input-file> <output-file>
#
# Author: C. Newham
# Date: 2004/12/02
#####################################################
```
This main header gives the name of the script, a brief
summary of what it does, usage information, the name of
the author, and when the script was written.


If you are using a source control system (e.g., CVS), you
can dispense with the author and date as these will be
stored when the script is archived. If you aren't using such
a system, we strongly advise that you not only include the
above information but also place in the header additional
data such as modification dates and authors.

Whatever system you use, make sure that you make the
format of the banner a standard across all of your scripts.

Every function should also have a header. If it is a
standalone function, it should have a main header, as
given above. If it is a function used locally in a script, it
should have a simpler banner stating what it does, what
parameters it expects, and what it returns, e.g.:

```
# Changes the filename extension
#
# param: $infile - the original filename
#
# returns: the modified name with new extension.
#
function change_filename( )
...
```
Comments should also be used frequently in your code to
say what the code is doing. While we aren't about to
dictate style, comments within the flow of the code are
generally better on a line by themselves, while variable
declaration comments are better on the same line as the
variable:

```
startup_dir=/home/startup/ # directory with startup files
file_limit=50 # maximum number of files to process
```

...
if [ -d "$startup_dir" ]
then
# the startup directory exists so read any initialisation file.
echo "initialising file processing..."


## Variables and Constants

Headers and comments are just one way to document
your code. Another is by the use of descriptive variable
names. Good variable names should give an indication of
what the variable represents. Names like "x", "resn" or
"procd" will only have meaning at the time that you write
the script. Six months down the track and they will be a
mystery.

Good names should be short but descriptive. The three
examples above might have been more meaningfully
written as "file_limit", "resolution", and "was_processed".
Don't make the names too long; the name
"horizontal_resolution_of_the_picture" just clutters a
script and takes away any advantage in making the name
so descriptive.

Constants should be in uppercase and should normally be
declared as read-only:

```
declare -r CAPITAL_OF_ENGLAND="London"
```
You should always avoid "magic numbers" sprinkled
throughout the code by using constants. For example:

```
if [[ $process_result == 68 ]]
...
```
should be replaced with:


```
declare -ir STAGE_3_FAILURE=68
...
if [[ $process_result == $STAGE_3_FAILURE ]]
...
```
Not only does this make the code more readable but it
makes changing the value easier, especially if it is used
numerous times in the script.


# Starting Up

In Chapter 6 we talked about using **getopts** to obtain
options and arguments passed in to a shell script. This
command makes it easy for the script programmer to
process what the user has provided, but what about the
other half of the deal? The programmer must make an
effort to make life as easy for the user as possible.
Nothing makes a user more irate than a script that doesn't
take standard arguments, doesn't provide a usage
message, doesn't process the arguments in the expected
way, and forces the user into a way of thinking that the
programmer thinks is the right way. Having to examine
the source code for a script to find out what is an
acceptable argument or option is usually the last straw!

The Free Software Foundation has published a set of
guidelines for writing GNU software that suggests
standard ways in which UNIX utilities should operate.[1]
When writing your own shell scripts, it is worthwhile to
follow the guidelines because your script will then look
familiar to users who have used other command-line
programs.

At a minimum your script should provide single letter
options (such as - _h_ ) and long options with the double
dash (such as — _help_ ). It should also provide two options _:
—help_ and _—version_. From the GNU manual:


**_—version_**

```
This option should direct the program to print
information about its name, version, origin, and legal
status, all on standard output, and then exit
successfully. Other options and arguments should be
ignored once this is seen, and the program should not
perform its normal function.
```
**_—help_**

```
This option should output brief documentation for
how to invoke the program, on standard output, then
exit successfully. Other options and arguments
should be ignored once this is seen, and the program
should not perform its normal function.
```
```
Near the end of the —help option's output there
should be a line that says where to mail bug reports.
It should have this format:
```
- Report bugs to _mailing-address_.

```
Table 11-1lists a few of the common single-letter
and long options that you may consider using for
your own scripts. This list is by no means exhaustive
and is intended merely for guidance.
```
**Table 11-1. Possible options**


**Long option Option Examples where used**

—all -a
_du_ , _ls_ , _nm_ , _stty_ , _uname_ ,
_unexpand_

—append -a etags, tee, time

—binary -b cpio, diff

—blocks -b head, tail

—date -d touch

—directory -d cpio

—exclude-from -X tar

—file -f fgrep

—help -h man


**Long option Option Examples where used**

—long -l ls

—line -l wc

—links -L cpio, ls

—output -o cc, sort

—quiet -q who

—recursive -r rm

—recursive -R ls

—silent -s Synonym for -quiet

—unique -u _sort_


```
Long option Option Examples where used
```
```
—verbose -v cpio, tar
```
```
—width -w pr, sdiff
```
For commands that take one or more input files and
produce an output file it is considered good practice to
make only the input files normal arguments (i.e _.,
command filename_ ) and have the output file specified by
an option (i.e., _command -o filename_ ).

Another thing to watch out for is assuming that a
particular environment variable needed by your script has
been set in the users' environment. If your script is relying
on the user to have set an environment variable, it is
probably better to redesign your script to allow the value
to be passed in as an argument.

[1]The document is available at _[http://www.gnu.org/prep/](http://www.gnu.org/prep/)_

_standards/_.


# Potential Problems

Here are some useful things to watch out for when
writing shell scripts. Being aware of them will not only
save you time in tracking down bugs but will also make
your scripts more robust, more readable, and above all,
more maintainable.

- Don't create massive scripts or functions that try
    to do everything. Split functionality up into
    smaller units and place them in functions. This
    not only makes the code easier to read but makes
    it easier to debug.
- Always place the shell execution directive (e.g.,
    _#!/bin/bash_ ) at the top of your scripts to ensure
    they will be run by _bash_.
- Don't use reserved words for variable names.
    This can become very confusing:

```
let let="echo"
let echo="hello"
echo "$echo world"
```
- Be careful with whitespace. Attempting the
    following assignment will not give the expected
    result:

```
cat = 5
```

- Don't use the same names for variables and
    functions:

```
function letter
{
echo $1etter
}
letter=letter
letter letter
```
```
This causes more confusion that it's worth. While
this example is contrived, be on your guard for
more subtle examples. To guard against this, try
and name your functions using verbs, e.g.,
function print_letter.
```
- Be careful when using the test operator **[** ... **]**. The
    following two **if** statements are not the same,
    although they look very similar:

```
if [ "$var" = 42 ]
if [ "$var" -eq 42 ]
```
```
The first is a string comparison, the second an
integer comparison. We suggest using (( ... )) for
arithmetic comparisons in if statements.
```

# Don't Use bash

Sometimes you might start writing a script and after
several hours of work find that you've created a monster
with many hundreds of lines of complicated code. This is
not always a bad thing, but it is a good idea to always be
thinking about whether the job could be done in a better
way.

Usually the choice of programming language should take
place at the design stage. If you are starting from scratch
on a Unix system you will have many options, including
C and C++, _perl_ , _python_ , and a host of others. They all
have their advantages and disadvantages, and no one
language will be the best solution for every problem.

If you find that your script has a huge amount of
processing to do quickly or if the script requires
mathematical capabilities beyond simple integer
arithmetic, it might be worthwhile considering C or C++
for the job. If you are looking for better portability across
systems, _python_ or _perl_ might be a better match to the
task.

However, even if _bash_ is not suitable in the final solution
to a problem, you might find it makes an excellent
language for mocking up your solution and trying out
various options.


# Chapter 12. bash for

# Your System

The first 10 chapters of this book looked at nearly all
aspects of _bash_ , from navigating the filesystem and
editing the command-line to writing shell scripts and
functions using lesser-known features of the shell. This is
all very well and good, but what if you have an old
version of _bash_ and want the new features shown in this
book (or worse yet, you don't have _bash_ at all)?

In this chapter we'll show you how to get the latest
version of _bash_ and install it on your system, and we'll
discuss potential problems you might encounter along the
way. We'll also look briefly at the examples that come
with _bash_ and how you can report bugs to the _bash_
maintainer.

# Obtaining bash

If you have a direct connection to the Internet, you should
have no trouble obtaining _bash_ ; otherwise, you'll have to
do a little more work.

The _bash_ home page is located at [http://www.gnu.org/](http://www.gnu.org/)
software/bash/bash.htmland you can find the very latest


details of the current distribution and where to obtain it
from there.

You can also get _bash_ on CD-ROM by ordering it
directly from the Free Software Foundation, either via the
web ordering page athttp://order.fsf.orgor from:

```
The Free Software Foundation (FSF)
59 Temple Place - Suite 330
Boston, MA 02111-1307 USA
Phone: +1-617-542-5942
Fax: +1-617-542-2652
Email: order@fsf.org
```

# Unpacking the Archive

Having obtained the archive file by one of the above
methods, you need to unpack it and install it on your
system. Unpacking can be done anywhere—we'll assume
you're unpacking it in your home directory. Installing it
on the system requires you to have root privileges. If you
aren't a system administrator with root access, you can
still compile and use _bash_ ; you just can't install it as a
system-wide utility. The first thing to do is uncompress
the archive file by typing **gunzip bash-3.0.tar.gz** .[1]Then
you need to "untar" the archive by typing **tar -xf
bash-3.0.tar**. The **-xf** means "extract the archived
material from the specified file." This will create a
directory called _bash-3.0_ in your home directory.

The archive contains all of the source code needed to
compile _bash_ and a large amount of documentation and
examples. We'll look at these things and how you go
about making a _bash_ executable in the rest of this chapter.

[1] _gunzip_ is the GNU decompression utility. _gunzip_ is

popular but relatively new and some systems don't have
it. If your system doesn't, you can obtain it by the same
methods as you obtained _bash_. _gunzip_ is available from
the FSF. **gzip -d** does the same thing as _gunzip_.


# What's in the Archive

The _bash_ archive contains a main directory ( _bash-3.0_ for
the current version) and a set of files and subdirectories.
Among the first files you should examine are:

**MANIFEST**

```
A list of all the files and directories in the archive
```
**COPYING**

```
The GNU Copyleft for bash
```
**NEWS**

```
A list of bug fixes and new features since the last
version
```
**README**

```
A short introduction and instructions for compiling
bash
```
You should also be aware of two directories:

**doc**

```
Information related to bash in various formats
```

**examples**

```
Examples of startup files, scripts, and functions
```
The other files and directories in the archive are mostly
things that are needed during the build. Unless you are
going to go hacking into the internal workings of the
shell, they shouldn't concern you.

## Documentation

The _doc_ directory contains a few articles that are worth
reading. Indeed, it would be well worth printing out the
manual entry for _bash_ so you can use it in conjunction
with this book. The _README_ file gives a short summary
of the files.

The document you'll most often use is the manual page
entry ( _bash.1_ ). The file is in _troff_ format—that used by
the manual pages. You can read it by processing it with
the text-formatter _nroff_ and piping the output to a pager
utility: **nroff -man bash.1 | more** should do the trick.
You can also print it off by piping it to the lineprinter
( _lp_ ). This summarizes all of the facilities your version of
_bash_ has and is the most up-to-date reference you can get.
This document is also available through the _man_ facility
once you've installed the package, but sometimes it's nice
to have a hard copy so you can write notes all over it.


Of the other documents, _FAQ_ is a Frequently Asked
Questions document with answers, _readline.3_ is the
manual entry for the _readline_ facility, and _article.ms_ is an
article about the shell that appeared in _Linux Journal_ , by
the current _bash_ maintainer, Chet Ramey.


## Configuring and Building

## bash

To compile _bash_ "straight out of the box" is easy;[2]you
just type **configure** and then **make**! The _bash configure_
script attempts to work out if you have various utilities
and C library functions, and whereabouts they reside on
your system. It then stores the relevant information in the
file _config.h_. It also creates a file called _config.status_ that
is a script you can run to recreate the current
configuration information. While the _configure_ is
running, it prints out information on what it is searching
for and where it finds it.

The _configure_ script also sets the location that _bash_ will
be installed; the default is the _/usr/local_ area ( _/usr/local/
bin_ for the executable, _/usr/local/man_ for the manual
entries etc.). If you don't have root privileges and want it
in your own home directory, or you wish to install _bash_ in
some other location, you'll need to specify a path to
configure. You can do this with the **—exec-prefix** option.
For example:

```
$ configure --exec-prefix=/usr
```
specifies that the _bash_ files will be placed under the _/usr_
directory. Note that configure prefers option arguments
be given with an equals sign (=).


After the configuration finishes and you type **make** , the
_bash_ executable is built. A script called _bashbug_ is also
generated, which allows you to report bugs in the format
the _bash_ maintainers want. We'll look at how you use it
later in this chapter.

Once the build finishes, you can see if the _bash_
executable works by typing **./bash**. If it doesn't, turn to
theSection 11.3inChapter 11.

To install _bash_ , type **make install**. This will create all of
the necessary directories ( _bin_ , _info_ , _man_ and its
subdirectories) and copy the files to them.

If you've installed _bash_ in your home directory, be sure to
add your own _bin_ path to your **PATH** and your own _man_
path to **MANPATH**.

_bash_ comes preconfigured with nearly all of its features
enabled, but it is possible to customize your version by
specifying what you want with the **—enable-** _feature_ and
**—disable-** _feature_ command-line options to _configure_.

Table 12-1 is a list of the configurable features and a
short description of what those features do.

**Table 12-1. Configurable features**


**Feature Description**

alias Support for aliases.

arith-for-command

```
Support for the alternate form
of the `for' command that
behaves like the C language
for statement.
```
array-variables
Support for one dimensional
arrays.

bang-history

```
C-shell-like history expansion
and editing.
```
brace-expansion Brace expansion.

command-timing
Support for the **time**
command.

cond-command
Support for the **[[** conditional
command.


**Feature Description**

cond-regexp

```
Support for matching POSIX
regular expressions using the
=~ binary operator in the [[
conditional command.
```
directory-stack

```
Support for the pushd , popd ,
and dirs directory
manipulation commands.
```
disabled-builtins

```
Whether a built-in can be run
with the builtin command,
even if it has been disabled
with enable -n.
```
dparen-arithmetic Support for **((...))**.

help-builtin Support for the **help** built-in.

history
History via the **fc** and **history**
commands.


**Feature Description**

job-control

```
Job control via fg , bg , and
jobs if supported by the
operating system.
```
multibyte

```
Support for multibyte
characters if the operating
system provides the necessary
support.
```
net-redirections

```
Special handling of filenames
of the form /dev/tcp/HOST/
PORT and /dev/udp/HOST/
PORT when used in
redirections.
```
process-substitution

```
Whether process substitution
occurs, if supported by the
operating system.
```
prompt-string-decoding

```
Whether backslash escaped
characters in PS1, PS2, PS3,
and PS4 are allowed.
```

**Feature Description**

progcomp

```
Programmable completion
facilities. If readline is not
enabled, this option has no
effect.
```
readline

```
readline editing and history
capabilities.
```
restricted

```
Support for the restricted
shell, the -r option to the shell,
and rbash.
```
select The **select** construct.

usg-echo-default

xpg-echo-default

```
Make echo expand
backslash-escaped characters
by default, without requiring
the -e option. This sets the
default value of the xpg_echo
shell option to on , which
makes the bash echo behave
more like the version specified
```

```
Feature Description
```
```
in the Single Unix
Specification, Version 2.
```
The options **disabled-builtins** and **xpg-echo-default** are
disabled by default. The others are enabled.

Many other shell features can be turned on or off by
modifying the file _config-.top.h_. For further details on this
file and configuring _bash_ in general, see _INSTALL_.

Finally, to clean up the source directory and remove all of
the object files and executables, type **make clean**. Make
sure you run **make install** first, otherwise you'll have to
rerun the installation from scratch.


## Testing bash

There are a series of tests that can be run on your newly
built version of _bash_ to see if it is running correctly. The
tests are scripts that are derived from problems reported
in earlier versions of the shell. Running these tests on the
latest version of _bash_ shouldn't cause any errors.

To run the tests just type **make tests** in the main _bash_
directory. The name of each test is displayed, along with
some warning messages, and then it is run. Successful
tests produce no output (unless otherwise noted in the
warning messages).

If any of the tests fail, you'll see a list of things that
represent differences between what is expected and what
happened. If this occurs you should file a bug report with
the _bash_ maintainer. See theSection 12.4.2later in this
chapter for information on how to do this.


## Potential Problems

Although _bash_ has been installed on a large number of
different machines and operating systems, there are
occasionally problems. Usually the problems aren't
serious and a bit of investigation can result in a quick
solution.

If _bash_ didn't compile, the first thing to do is check that
_configure_ guessed your machine and operating system
correctly. Then check the file _NOTES_ , which contains
some information on specific UNIX systems. Also look
in _INSTALL_ for additional information on how to give
_configure_ specific compilation instructions.


## Installing bash as a Login

## Shell

Having installed _bash_ and made sure it is working
correctly, the next thing to do is to make it your login
shell. This can be accomplished in two ways.

Individual users can use the _chsh_ (change shell) command
after they log in to their accounts. _chsh_ asks for their
password and displays a list of shells to choose from.
Once a shell is chosen, _chsh_ changes the appropriate entry
in _/etc/passwd_. For security reasons, _chsh_ will only allow
you to change to a shell if it exists in the file _/etc/shells_ (if
_/etc/shells_ doesn't exist, _chsh_ asks for the pathname of the
shell).

Another way to change the login shell is to edit the
password file directly. On most systems, _/etc/passwd_ will
have lines of the form:

```
cam:pK1Z9BCJbzCrBNrkjRUdUiTtFOh/:501:100:Cameron Newham:/home/cam:/bin/bash
cc:kfDKDjfkeDJKJySFgJFWErrElpe/:502:100:Cheshire Cat:/home/cc:/bin/bash
```
As **root** you can just edit the last field of the lines in the
password file to the pathname of whatever shell you
choose.


If you don't have root access and _chsh_ doesn't work, you
can still make _bash_ your login shell. The trick is to
replace your current shell with _bash_ by using **exec** from
within one of the startup files for your current shell.

If your current shell is similar to _sh_ (e.g., _ksh_ ), you have
to add the line:

```
[ -f / pathname /bash ] && exec / pathname /bash --login
```
to your _.profile_ , where _pathname_ is the path to your _bash_
executable.

You will also have to create an empty file called
_.bash_profile_. The existence of this file prevents _bash_
from reading your _.profile_ and re-executing the
**exec—** thus entering an infinite loop. Any initialization
code that you need for _bash_ can just be placed in
_.bash_profile_.

If your current shell is similar to _csh_ (e.g., _tcsh_ ) things are
slightly easier. You just have to add the line:

```
if ( -f / pathname /bash ) exec / pathname /bash --login
```
to your _.login_ , where _pathname_ is the path to your _bash_
executable.


## Examples

The _bash_ archive also includes an examples directory.
This directory contains some subdirectories for scripts,
functions, and examples of startup files.

The startup files in the _startup-files_ directory provide
many examples of what you can put in your own startup
files. In particular, _bash_aliases_ gives many useful
aliases. Bear in mind that if you copy these files
wholesale, you'll have to edit them for your system
because many of the paths will be different. Refer to
Chapter 3for further information on changing these files
to suit your needs.

The _functions_ directory contains about 50 files with
function definitions that you might find useful. Among
them are:

**basename**

```
The basename utility, missing from some systems
```
**dirfuncs**

```
Directory manipulation facilities
```
**dirname**

```
The dirname utility, missing from some systems
```

**whatis**

```
An implementation of the Tenth Edition Bourne shell
whatis builtin
```
**whence**

```
An almost exact clone of the Korn shell whence
builtin
```
Especially helpful, if you come from a Korn shell
background, is _kshenv_. This contains function definitions
for some common Korn facilities such as **whence** , **print** ,
and the two-parameter **cd** builtins.

The _scripts_ directory contains over 20 examples of _bash_
scripts. The two largest scripts are examples of the
complex things you can do with shell scripts. The first is
a (rather amusing) adventure game interpreter and the
second is a C shell interpreter. The other scripts include
examples of precedence rules, a scrolling text display, a
"spinning wheel" progress display, and how to prompt the
user for a particular type of answer.

Not only are the script and function examples useful for
including in your environment, they also provide many
alternative examples that you can learn from when
reading this book. We encourage you to experiment with
them.


[2] This configuration information pertains to _bash_

version 3.0 and later. The configuration and installation
for earlier versions is fairly easy, although it differs in
certain details. For further information, refer to the
_INSTALL_ instructions that came with your version of
_bash_.


# Who Do I Turn to?

No matter how good something is or how much
documentation comes with it, you'll eventually come
across something that you don't understand or that doesn't
work. In such cases it can't be stressed enough to
_carefully read the documentation_ (in computer parlance:
RTFM).[3]In many cases this will answer your question
or point out what you're doing wrong.

Sometimes you'll find this only adds to your confusion or
confirms that there is something wrong with the software.
The next thing to do is to talk to a local _bash_ guru to sort
out the problem. If that fails, or there is no guru, you'll
have to turn to other means (currently only via the
Internet).

## Asking Questions

If you have any questions about _bash_ , there are currently
two ways to go about getting them answered. You can
email questions tobash-maintainers@gnu.orgor you can
post your question to the USENET newsgroups
_gnu.bash.bug_ or _comp.unix.shell_.

In both cases either the _bash_ maintainer or some
knowledgeable person on USENET will give you advice.


When asking a question, try to give a meaningful
summary of your question in the subject line.


## Reporting Bugs

Bug reports should be sent to bug-bash@gnu.org, and
include the version of _bash_ and the operating system it is
running on, the compiler used to compile _bash_ , a
description of the problem, a description of how the
problem was produced, and, if possible, a fix for the
problem. The best way to do this is with the _bashbug_
script, installed with _bash_.

Before you run _bashbug_ , make sure you've set your
**EDITOR** environment variable to your favorite editor
and have exported it ( _bashbug_ defaults to _emacs_ , which
may not be installed on your system). When you execute
_bashbug_ it will enter the editor with a partially blank
report form. Some of the information ( _bash_ version,
operating system version, etc.) will have been filled in
automatically. We'll take a brief look at the form, but
most of it is self-explanatory.

The **From** : field should be filled out with your email
address. For example:

```
From: confused@wonderland.oreilly.com
```
Next comes the **Subject** : field; make an effort to fill it
out, as this makes it easier for the maintainers when they
need to look up your submission. Just replace the line


surrounded by square brackets with a meaningful
summary of the problem.

The next few lines are a description of the system and
should not be touched. Next comes the **Description** :
field. You should provide a detailed description of the
problem and how it differs from what is expected. Try to
be as specific and concise as possible when describing the
problem.

The **Repeat-By** : field is where you describe how you
generated the problem; if necessary, list the exact
keystrokes you used. Sometimes you won't be able to
reproduce the problem yourself, but you should still fill
out this field with the events leading up to the problem.
Attempt to reduce the problem to the smallest possible
form. For example, if it was a large shell script, try to
isolate the section that produced the problem and include
only that in your report.

Lastly, the **Fix** : field is where you can provide the
necessary patch to fix the problem if you've investigated
it and found out what was going wrong. If you have no
idea what caused the problem, just leave the field blank.

Once you've finished filling in the form, save it and exit
your editor. The form will automatically be sent to the
maintainers.

[3]RTFM stands for "Read The F(laming) Manual."


# Appendix A. Related

# Shells

The fragmentation of the UNIX marketplace has had its
advantages and disadvantages. The advantages came
mostly in the early days: lack of standardization and
proliferation among technically knowledgeable
academics and professionals contributed to a healthy
"free market" for UNIX software, in which several
programs of the same type (e.g., shells, text editors,
system administration tools) would often compete for
popularity. The best programs would usually become the
most widespread, while inferior software tended to fade
away.

But often there was no single "best" program in a given
category, so several would prevail. This led to the current
situation, where multiplicity of similar software has led to
confusion, lack of compatibility, and—most unfortunate
of all—the inability of UNIX to capture as big a share of
the market as other operating platforms (MS-DOS,
Microsoft Windows, Novell NetWare, etc.).

The "shell" category has probably suffered in this way
more than any other type of software. As we said in the
Preface and in Chapter 1, several shells are currently
available; the differences between them are often not all
that great.


Therefore we felt it necessary to include information on
shells similar to _bash_. This appendix summarizes the
differences between _bash_ and the following:

- The standard Bourne shell, as a kind of
    "baseline"
- The IEEE POSIX 1003.2 shell Standard, to
    which _bash_ adheres and other shells will adhere
    in the future
- The Korn shell ( _ksh_ ), a popular commercial shell
    provided with many UNIX systems
- _pdksh_ , a widely used public domain Korn shell
- _zsh_ , a popular alternative to _bash_ and the Korn
    shell

# The Bourne Shell

_bash_ is almost completely backward-compatible with the
Bourne shell. The only significant feature of the latter that
_bash_ doesn't support is **^** (caret) as a synonym for the
pipe (|) character. This is an archaic feature that the
Bourne shell includes for its own backward compatibility
with earlier shells. No modern UNIX version has any
shell code that uses **^** as a pipe.


To describe the differences between the Bourne shell and
_bash_ , we'll go through each chapter of this book and
enumerate the features discussed in the chapter that the
Bourne shell does _not_ support. Although some versions of
the Bourne shell exist that include a few _bash_ features,[1]
we refer to the standard Bourne shell that has been around
for many years.

**_Chapter 1_**

```
The cd - form of the cd command; tilde ( ~ )
expansion; the jobs command; the help built-in.
```
**_Chapter 2_**

```
All. (That is, the Bourne shell doesn't support any of
the readline , history, and editing features discussed
in this chapter.)
```
**_Chapter 3_**

```
Aliases; prompt string customization; set options.
The Bourne shell supports only the following: -e , -k ,
-n , -t , -u , -v , -x , and -. It doesn't support option
names ( -o ). The shopt built-in. Environment files
aren't supported. The following built-in variables
aren't supported:
```
All variables beginning with BASH_

All variables beginning with COMP


#### CDPATH DIRSTACK

#### FCEDIT FUNCNAME

#### GROUPS HISTCMD

#### HISTCONTROL HISTFILE

#### HISTIGNORE HISTSIZE

#### HISTFILESIZE HOSTFILE

#### HOSTNAME HOSTTYPE

#### IGNOREEOF INPUTRC

#### LANG LC_ALL

#### LC_COLLATE LC_MESSAGES

#### LINENO MACHTYPE

#### MAILCHECK OLDPWD


#### OPTARG OPTERR

#### OPTIND OSTYPE

#### PIPESTATUS

#### PS3 PS4

#### POSIXLY_CORRECTPROMPT_COMMAND

#### PWD RANDOM

#### REPLY SECONDS

#### SHELLOPTS SHLVL

#### TIMEFORMAT TMOUT

```
auto_resume histchars
```
**_Chapter 4_**

```
Functions; the type command; the local command;
the ${#parameter} operator; pattern-matching
variable operators ( % , %% , # , ## ). Extended pattern
matching. Command-substitution syntax is different:
```

```
use the older ` command ` instead of $( command ).
The built-in pushd and popd commands.
```
**_Chapter 5_**

```
The! keyword; the select construct isn't supported.
The Bourne shell return doesn't exit a script when it
is sourced with. (dot).
```
**_Chapter 6_**

```
Use the external command getopt instead of getopts ,
but note that it doesn't really do the same thing.
Integer arithmetic isn't supported: use the external
command expr instead of the $(( arithmetic-exp ))
syntax. The arithmetic conditional (( arithmetic-exp
)) isn't supported; use the old condition test syntax
and the relational operators -lt , -eq , etc. Array
variables are not supported. declare and let aren't
supported.
```
**_Chapter 7_**

```
The command , builtin , and enable built-ins. The -e
and -E options to echo are not supported. The I/O
redirectors >| and <> are not supported. None of the
options to read is supported. printf is usually
available as an external command.
```
**_Chapter 8_**

```
Job control—specifically, the jobs , fg , and bg
commands. Job number notation with % , i.e., the kill
```

```
and wait commands only accept process IDs. The -
option to trap (reset trap to the default for that
signal). trap only accepts signal numbers, not logical
names. The disown built-in.
```
**_Chapter 9_**

```
The DEBUG, ERR, and RETURN fake signals are
not supported. The EXIT fake signal is supported as
signal 0.
```
**_Chapter 10_**

```
The ulimit command and privileged mode aren't
supported. The -S option to umask is not supported.
The Bourne shell's restrictive counterpart, rsh , only
inhibits assignment to PATH.
```
[1]For example, the Bourne shell distributed with System

V supports functions and a few other shell features
common to _bash_ and the Korn shell.


# The IEEE 1003.2 POSIX

# Shell Standard

There have been many attempts to standardize UNIX.
Hardware companies' monolithic attempts at market
domination, fragile industry coalitions, marketing
failures, and other such efforts are the stuff of
history—and the stuff of frustration.

Only one standardization effort has not been tied to
commercial interests: the Portable Operating System
Interface, known as POSIX. This effort started in 1981
with the _/usr/group_ (now UniForum) Standards
Committee, which produced the _/usr/group Standard_
three years later. The list of contributors grew to include
the Institute of Electrical and Electronic Engineers
(IEEE) and the International Organization for
Standardization (ISO).

The first POSIX standard was published in 1988. This
one, called IEEE P1003.1, covers low-level issues at the
system-call level. IEEE P1003.2, covering the shell,
utility programs, and user interface issues, was ratified in
September 1992 after a six-year effort. In September
2001, a joint revision of both standards was approved.
The new standard, covering all the material in the two
earlier separate documents, became known as IEEE


Standard 1003.1-2001. The latest version of the standard
is 1003.1-2004.

The POSIX standards were never meant to be rigid and
absolute. The committee members certainly weren't about
to put guns to the heads of operating system
implementers and force them to adhere. Instead, the
standards are designed to be flexible enough to allow for
both coexistence of similar available software, so that
existing code isn't in danger of obsolescence, and the
addition of new features, so that vendors have the
incentive to innovate. In other words, they are supposed
to be the kind of third-party standards that vendors might
actually be interested in following.

As a result, most UNIX vendors currently comply with
both standards. _bash_ is no exception; it is almost 100%
POSIX-compliant.

The shell part of the standard describes utilities that must
be present on all systems, and others that are optional,
depending upon the nature of the system. One such option
is the User Portability Utilities option, which defines
standards for interactive shell use and interactive utilities
like the vi editor. The standard—on the order of 2,000
pages—is available through the IEEE; for information,
contact the IEEE:

```
IEEE Customer Service
445 Hoes Lane, PO Box 1331
Piscataway, NJ 08855-1331
(800) 678-IEEE (United States and Canada)
```

```
(732) 981-0060 (international/local)
(732) 981-9667 (fax)
customer.service@ieee.org
http://www.standards.ieee.org/catalog/ordering.html
```
The committee members had two motivating factors to
weigh when they designed the shell standard. On the one
hand, the design had to accommodate, as much as
possible, existing shell code written under various
Bourne-derived shells (the Version 7, System V, BSD,
and Korn shells). These shells are different in several
extremely subtle ways, most of which have to do with the
ways certain syntactic elements interact with each other.

It must have been quite difficult and tedious to spell out
these differences, let alone to reach compromises among
them. Throw in biases of some committee members
towards particular shells, and you might understand why
it took six years to ratify the first 1003.2 standard and
further years to merge the standards.

On the other hand, the shell design had to serve as a
standard on which to base future shell implementations.
This implied goals of simplicity, clarity, and
precision—objectives that seem especially elusive in the
context of the above problems.

The designers found one way of ameliorating this
dilemma: they decided that the standard should include
not only the features included in the shell, but also those
explicitly omitted and those included but with unspecified
functionality. The latter category allows some of the


existing shells' innovations to "sneak through" without
becoming part of the standard, while listing omitted
features helps programmers determine which features in
existing shell scripts won't be portable to future shells.

The POSIX standard is primarily based on the System V
Bourne shell, which is a superset of the Version 7 shell
discussed earlier in this appendix. Therefore you should
assume that _bash_ features that aren't present in the Bourne
shell also aren't included in the POSIX standard.

The following _bash_ features are left "unspecified" in the
standard, meaning that their syntax is acceptable but their
functionality is not standardized:

- The other syntax for functions shown inChapter
    4 is supported; see the following discussion.
- The [[...]] syntax for conditional tests. The
    external **test** or [...] utility should be used instead.
- The **select** control structure.
- Code blocks ({...}) are supported, but for
    maximum portability, the curly brackets should
    be quoted (for reasons too complicated to go into
    here).
- Signal numbers are only allowed if the numbers
    for certain key signals (INT, TERM, and a few
    others) are the same as on the most important


```
historical versions of UNIX. In general, shell
scripts should use symbolic names for signals.
```
The POSIX standard supports functions, but the
semantics are weaker: it is not possible to define local
variables, and functions can't be exported.

The command lookup order has been changed to allow
certain built-in commands to be overridden by functions.
Built-in commands are divided into two sets by their
positions in the command lookup order: some are
processed before functions, some after. Specifically, the
built-in commands **break** , : (do nothing), **continue** ,
.( **source** ), **eval** , **exec** , **exit** , **export** , **readonly** , **return** , **set** ,
**shift** , **trap** , and **unset** take priority over functions.

Finally, because the POSIX standard is meant to promote
shell script portability, it avoids mentioning certain
fundamental implementation issues: in particular, there is
no requirement that multitasking be used for background
jobs, subshells, etc. This was done to allow portability to
non-multitasking systems like MS-DOS, so that shells on
these systems can be POSIX-compliant.


# The Korn Shell

One of the first major alternatives to the "traditional"
shells, Bourne and C, was the Korn shell, publicly
released in 1986 as part of AT&T's "Experimental
Toolchest." The Korn shell was written by David Korn at
AT&T. The first version was unsupported, but eventually
UNIX System Laboratories (USL) decided to give it
support when they released it with their version of UNIX
(System V Release 4) in 1989. The November 1988 Korn
shell is the most widely used version of this shell.

The 1988 release is not fully POSIX-compliant—less so
than _bash_. The latest release (1993) has brought the Korn
shell into better compliance as well as providing more
features and streamlining existing features.

The 1993 Korn shell and _bash_ share many features, but
there are some important differences in the Korn shell:

- Functions are more like separate entities than part
    of the invoking shell (traps and options are not
    shared with the invoking shell).
- Associative arrays are supported.
- Floating-point numbers and expressions are
    supported.


- Coroutines are supported. Two processes can
    communicate with one another by using the **print**
    and **read** commands.
- The command **print** replaces **echo**. **print** can
    have a file descriptor specified and can be used to
    communicate with coroutines.
- Function autoloading is supported. Functions are
    read into memory only when they are called.
- One-dimensional arrays are supported, although
    they are limited in size (4,096 elements in early
    versions of _ksh93_ , 64K elements in later
    releases).
- The history list is kept in a file rather than in
    memory. This allows concurrent instantiations of
    the shell to access the same history list, a possible
    advantage in certain circumstances.
- There is no default startup file. If the
    environment variable **ENV** is not defined,
    nothing is read.
- The **type** command is replaced with the more
    restrictive **whence**.
- The primary prompt string ( **PS1** ) doesn't allow
    escaped commands.
- There is no built-in equivalent to **enable**.


- There is no provision for key bindings and no
    direct equivalent to readline.
- There are no built-in equivalents to **pushd** , **popd** ,
    and **dirs**. They have to be defined as functions if
    you want them.
- The history substitution mechanism is not
    supported.
- Prompt strings don't allow backslash-escaped
    special characters.
- Many of the bash environment variables don't
    exist.

In addition, the startup and environment files for Korn are
different, consisting of _.profile_ and the file specified by
the **ENV** variable. The default environment file can be
overridden by using the variable **ENV**. There is no logout
file.

For a more detailed list of the differences between _bash_
and the Korn shell see the _FAQ_ file in the _doc_ directory of
the _bash_ archive.

The Korn shell is a good alternative to _bash_. Its only
major drawback is that it is upgraded only every few
years.


# pdksh

_pdksh_ (Public Domain Korn shell) is a version of the
Korn shell that is a free alternative to _bash_. _pdksh_ is
available as source code in various places on the Internet,
including the USENET newsgroup _comp.sources.unix_ ,
and the _pdksh_ home page [http://www.cs.mun.ca/](http://www.cs.mun.ca/)
~michael/pdksh/ of the current maintainer, Michael
Rendell.

_pdksh_ was originally written by Eric Gisin, who based it
on Charles Forsyth's public domain Version 7 Bourne
shell. It has all Bourne shell features plus some of the
POSIX extensions and a few features of its own.

_pdksh_ 's additional features include user-definable tilde
notation, in which you can set up **~** as an abbreviation for
anything, not just usernames.

Otherwise, _pdksh_ lacks a few features of the official Korn
version and _bash_. In particular, it lacks the following
_bash_ features:

- The built-in variable **PS4**
- The advanced I/O redirectors **>|** and **<>**
- The options **errexit** , **noclobber** , and **privileged**


One important advantage that _pdksh_ has over _bash_ is that
the executable is only about a third the size and it runs
considerably faster. Weighed against this is that it is less
POSIX-compliant, has had numerous people add code to
it (so it hasn't been as strongly controlled as _bash_ ), and
isn't as polished a product as _bash_ (for example, the
documentation isn't anywhere near as detailed or
complete).

However, _pdksh_ is a worthwhile alternative for those who
want something other than _bash_ and can't obtain the Korn
shell.


# zsh

_zsh_ is a powerful interactive shell and scripting language
with many features found in _bash_ , _ksh_ , and _tcsh_ , as well
as several unique features.

_zsh_ was originally written by Paul Falsted in the early
1990s and is now maintained by various people.

It is freely available and should compile and run on just
about any modern version of Unix. Ports for other
operating systems are also available. The _zsh_ home page
is _[http://www.zsh.org](http://www.zsh.org)_. The current version is 4.2.1.

Some of the main differences between _bash_ and _zsh_ are:

- Extended globbing capabilities
- A slightly more advanced textual completion
    system
- A powerful multi-line command line editor
- Various visual bells and whistles, such as
    command prompt color and placement

_zsh_ is a good alternative to _bash_ , especially for "power
users."


# Shell Clones and

# Unix-like Platforms

The proliferation of shells has not stopped at the
boundaries of UNIX-dom. Many programmers who got
their initial experience on UNIX systems and
subsequently crossed over into the PC world wished for a
nice UNIX-like environment. It's not surprising then that
several UNIX shell-style interfaces to small-computer
operating systems have appeared, Bourne shell
emulations among them.

In the past several years, not only shell clones have
appeared, but entire Unix "environments." Two of them
use shells that we've already discussed. Two others
provide their own shell reimplementations. Providing lists
of major and minor differences is counterproductive.
Instead, this section describes each environment in turn
(in alphabetical order), along with contact and Internet
download information.

## Cygwin

Cygnus Consulting (now part of Red Hat) created the
_cygwin_ environment. First creating _cgywin.dll_ , a shared
library that provides Unix system call emulation, they


ported a large number of GNU utilities to various
versions of Microsoft Windows. The greatest
functionality comes under Windows/NT, Windows 2000,
and Windows XP, although the environment can and does
work under Windows 95/98/ME, as well.

The _cygwin_ environment uses _bash_ for its shell, GCC for
its C compiler, and the rest of the GNU utilities for its
Unix toolset. A sophisticated _mount_ command provides a
mapping of the Windows C:\path notation to Unix
filenames.

The _cygwin_ project can be found at
[http://www.cygwin.com.](http://www.cygwin.com.)


## DJGPP

The DJGPP suite provides 32-bit GNU tools for the
MS-DOS environment. To quote the web page:

```
DJGPP is a complete 32-bit C/C++ development
system for Intel 80386 (and higher) PCs running
MS-DOS. It includes ports of many GNU
development utilities. The development tools
require a 80386 or newer computer to run, as do
the programs they produce. In most cases, the
programs it produces can be sold commercially
without license or royalties.
```
The name comes from the initials of D.J. Delorie, who
ported the GNU C++ compiler, g++ to MS-DOS, and the
text initials of g++, GPP. It grew into essentially a full
Unix environment on top of MS-DOS, with all the GNU
tools and _bash_ as its shell. Unlike _cygwin_ or UWIN (see
later in this Appendix), you don't need a version of
Windows, just a full 32-bit processor and MS-DOS.
(Although, of course, you can use DJGPP from within a
Windows MS-DOS window.) The web site is
[http://www.delorie.com/djgpp/.](http://www.delorie.com/djgpp/.)


## MKS Toolkit

Perhaps the most established Unix environment for the
PC world is the MKS Toolkit from Mortice Kern
Systems:

```
MKS Canada - Corporate Headquarters
410 Albert Street
Waterloo, ON N2L 3V3
Canada
+1 519 884-2251
+1 519 884-8861 (fax)
+1 800 265-2797 (sales)
http://www.mks.com
```
The MKS Toolkit comes in various versions depending
upon the development environment and the number of
developers who will be using it. It includes a shell that is
POSIX-compliant, along with just about all the features
of the 1988 Korn shell, as well as over 300 utilities, such
as _awk_ , _perl_ , _vi_ , _make_ , etc. Their library supports over
1,500 Unix APIs, making it extremely complete and easy
to port to the Windows environment. More information is
available at _[http://www.mkssoftware.com/products/tk/](http://www.mkssoftware.com/products/tk/)
ds_tkdev.asp_.


## AT&T UWIN

The UWIN package is a project by David Korn and his
colleagues to make a Unix environment available under
Microsoft Windows. It is similar in structure to _cygwin_ ,
discussed earlier. A shared library, _posix.dll_ , provides
emulation of the Unix system call APIs. The system call
emulation is quite complete. An interesting twist is that
the Windows registry can be accessed as a filesystem
under _/reg_. On top of the Unix API emulation, _ksh93_ and
over 200 Unix utilities (or rather, re-implementations)
have been compiled and run. The UWIN environment
relies on the native Microsoft Visual C/C++ compiler,
although the GNU development tools are available for
download and use with UWIN.

The project can be found athttp://www.research.att.com/
sw/tools/uwin/. The web site describes what is available,
with links for downloading binaries, as well as
information on commercial licensing of the UWIN
package. Also included are links to various papers on
UWIN, additional useful software, and links to other,
similar packages.


# Appendix B. Reference

# Lists

# Invocation

Table B-1 and Table B-2list the options you can use
when invoking current versions of _bash_ and the older 1. _x_
version, respectively.[1]The multicharacter options must
appear on the command line before the single-character
options. In addition to these, any **set** option can be used
on the command line; seeTable B-7. Login shells are
usually invoked with the options **-i** (interactive), **-s** (read
from standard input), and **-m** (enable job control).

**Table B-1. Command-line options**

```
Option Meaning
```
```
-c string
```
```
Commands are read from string , if
present. Any arguments after string
are interpreted as positional
parameters, starting with $0.
```

**Option Meaning**

#### -D

```
A list of all double-quoted strings
preceded by $ is printed on the
standard ouput. These are the
strings that are subject to language
translation when the current locale
is not C or POSIX. This also turns
on the -n option.
```
-i

```
Interactive shell. Ignores signals
TERM, INT, and QUIT. With job
control in effect, TTIN, TTOU,
and TSTP are also ignored.
```
-l
Makes _bash_ act as if invoked as a
login shell.

-o _option_
Takes the same arguments as **set
-o**.

#### -O, +O

_shopt-option_

```
shopt-option is one of the shell
options accepted by the shopt
builtin. If shopt-option is present,
```

**Option Meaning**

```
-O sets the value of that option;
+O unsets it. If shopt-option is not
supplied, the names and values of
the shell options accepted by shopt
are printed on the standard output.
If the invocation option is +O , the
output is displayed in a format that
may be reused as input.
```
-s

```
Reads commands from the
standard input. If an argument is
given to bash , this flag takes
precedence (i.e., the argument
won't be treated as a script name
and standard input will be read).
```
-r Restricted shell. SeeChapter 10.

-v
Prints shell input lines as they're
read.


**Option Meaning**

#### -

```
Signals the end of options and
disables further option processing.
Any options after this are treated as
filenames and arguments. — is
synonymous with -.
```
—debugger

```
Arranges for the debugger profile
to be executed before the shell
starts. Turns on extended
debugging mode and shell function
tracing.[2]
```
—dump-strings Does the same as **-D**.

—dump-po-strings

```
Does the same as - D but the output
is in the GNU gettext po (portable
object) file format.
```
—help
Displays a usage message and
exits.


**Option Meaning**

—login
Makes _bash_ act as if invoked as a
login shell. Same as - **l**.

—noediting

```
Does not use the GNU readline
library to read command lines if
interactive.
```
—noprofile

```
Does not read the startup file /etc/
profile or any of the personal
initialization files.
```
—norc

```
Does not read the initialization file
~/.bashrc if the shell is interactive.
This is on by default if the shell is
invoked as sh.
```
—posix

```
Changes the behavior of bash to
follow the POSIX guidelines more
closely where the default operation
of bash is different.
```

```
Option Meaning
```
```
—quiet
Shows no information on shell
startup. This is the default.
```
```
—rcfile file ,
—init-file file
```
```
Executes commands read from file
instead of the initialization file
~/.bashrc , if the shell is interactive.
```
```
—verbose Equivalent to -v.
```
```
—version
Shows the version number of this
instance of bash and then exits.
```
```
[2]Only available in bash version 3.0 and later.
```
**Table B-2. Old command-line options**


**Option Meaning**

**-c** _string_

```
Commands are read from string , if
present. Any arguments after string
are interpreted as positional
parameters, starting with $0.
```
-i

```
Interactive shell. Ignores signals
TERM, INT, and QUIT. With job
control in effect, TTIN, TTOU, and
TSTP are also ignored.
```
-s

```
Reads commands from the
standard input. If an argument is
given to bash , this flag takes
precedence (i.e., the argument
won't be treated as a script name
and standard input will be read).
```
-r Restricted shell. SeeChapter 10.

#### -

```
Signals the end of options and
disables further option processing.
Any options after this are treated as
```

**Option Meaning**

```
filenames and arguments. — is
synonymous with -.
```
-norc

```
Does not read the initialization file
~/.bashrc if the shell is interactive.
This is on by default if the shell is
invoked as sh.
```
-noprofile

```
Does not read the startup file /etc/
profile or any of the personal
initialization files.
```
-rcfile _file_

```
Executes commands read from file
instead of the initialization file
~/.bashrc , if the shell is interactive.
```
-version

```
Shows the version number of this
instance of bash when starting.
```
-quiet
Shows no information on shell
startup. This is the default.


```
Option Meaning
```
```
-login
Makes bash act as if invoked as a
login shell.
```
```
-nobraceexpansion
Does not perform curly brace
expansion.
```
```
-nolineediting
```
```
Does not use the GNU readline
library to read command lines if
interactive.
```
```
-posix
```
```
Changes the behavior of bash to
follow the POSIX guidelines more
closely where the default operation
of bash is different.
```
[1]At the time of writing, the old 1. _x_ versions of _bash_ are

still used. We strongly recommend that you upgrade to
the latest version. We have included a table of old options
(Table B-2) just in case you encounter an old version of
the shell.


# Prompt String

# Customizations

Table B-3 shows a summary of the prompt
customizations that are available. The customizations \[
and \] are not available in _bash_ versions prior to 1.14. \a,
\e, \H, \T, \@, \v, and \V are not available in versions
prior to 2.0. \A, \D, \j, \l, and \r are only available in later
versions of _bash_ 2.0 and in _bash_ 3.0.

**Table B-3. Prompt string customizations**

```
Command Meaning
```
```
\a The ASCII bell character (007)
```
#### \A

```
The current time in 24-hour HH:MM
format
```
```
\d The date in "Weekday Month Day" format
```

**Command Meaning**

#### \D

{ _format_ }

```
The format is passed to strftime(3) and the
result is inserted into the prompt string; an
empty format results in a locale-specific
time representation; the braces are required
```
\e The ASCII escape character (033)

\H The hostname

\h The hostname up to the first "."

\j

```
The number of jobs currently managed by
the shell
```
\l
The basename of the shell's terminal
device name

\n A carriage return and line feed


**Command Meaning**

\r A carriage return

\s The name of the shell

#### \T

```
The current time in 12-hour HH:MM:SS
format
```
\t The current time in HH:MM:SS format

#### \@

```
The current time in 12-hour a.m./p.m.
format
```
\u The username of the current user

\v The version of _bash_ (e.g., 2.00)

#### \V

```
The release of bash ; the version and
patchlevel (e.g., 3.00.0)
```

**Command Meaning**

\w The current working directory

#### \W

```
The basename of the current working
directory
```
#### \#

```
The command number of the current
command
```
#### \!

```
The history number of the current
command
```
#### \$

```
If the effective UID is 0, print a #,
otherwise print a $
```
\nnn Character code in octal

\\ Print a backslash


**Command Meaning**

#### \[

```
Begin a sequence of non-printing
characters, such as terminal control
sequences
```
\] End a sequence of non-printing characters


# Built-In Commands and

# Reserved Words

Table B-4shows a summary of all built-in commands and
reserved words. The letters in the **Type** column of the
table have the following meanings: R = reserved word,
blank = Builtin.

**Table B-4. Commands and reserved words**

```
Command Chapter Type Summary
```
#### ! 5 R

```
Logical NOT of a
command exit status.
```
#### : 7

```
Do nothing (just do
expansions of any
arguments).
```
#### . 4

```
Read file and execute its
contents in current shell.
```

**Command Chapter Type Summary**

alias 3

```
Set up shorthand for
command or command
line.
```
bg 8 Put job in background.

bind 2

```
Bind a key sequence to a
readline function or
macro.
```
break 5

```
Exit from surrounding
for , select , while , or
until loop.
```
builtin 5
Execute the specified
shell built-in.

case 5 R

```
Reserved word.
Multi-way conditional
construct.
```

**Command Chapter Type Summary**

cd 1
Change working
directory.

command 7

```
Run a command
bypassing shell function
lookup.
```
compgen D
Generate possible
completion matches.

complete D

```
Specify how completion
should be performed.
```
continue

```
Skip to next iteration of
for , select , while , or
until loop.
```
declare 6

```
Declare variables and
give them attributes.
```

**Command Chapter Type Summary**

dirs 6

```
Display the list of
currently remembered
directories.
```
disown 8
Remove a job from the
job table.

do 5 R

```
Part of a for , select ,
while , or until looping
construct.
```
done 5 R

```
Part of a for , select ,
while , or until looping
construct.
```
echo 4
Expand and print any
arguments.

elif 5 R Part of an **if** construct.


**Command Chapter Type Summary**

else 5 R Part of an **if** construct.

enable 7
Enable and disable
built-in shell commands.

esac 5 R Part of a **case** construct.

eval 7

```
Run the given arguments
through command-line
processing.
```
exec 9

```
Replace the shell with
the given program.
```
exit 5 Exit from the shell.

export 3

```
Create environment
variables.
```

**Command Chapter Type Summary**

fc 2
Fix command (edit
history file).

fg 8
Put background job in
foreground.

fi 5 R Part of an **if** construct.

for 5 R Looping construct.

function 4 R Define a function.

getopts 6

```
Process command-line
options.
```
hash 3

```
Full pathnames are
determined and
remembered.
```

**Command Chapter Type Summary**

help 1

```
Display helpful
information on built-in
commands.
```
history 1
Display command
history.

if 5 R Conditional construct.

in 5 R Part of a **case** construct.

jobs 1

```
List any background
jobs.
```
kill 8
Send a signal to a
process.

let 6
Arithmetic variable
assignment.


**Command Chapter Type Summary**

local 4 Create a local variable.

logout 1 Exits a login shell.

popd 4

```
Removes a directory
from the directory stack.
```
pushd 4
Adds a directory to the
directory stack.

pwd 1
Print the working
directory.

read 7
Read a line from
standard input.

readonly 6

```
Make variables read-only
(unassignable).
```

**Command Chapter Type Summary**

return 5

```
Return from the
surrounding function or
script.
```
select 5 R
Menu-generation
construct.

set 3 Set options.

shift 6
Shift command-line
arguments.

suspend
Suspend execution of a
shell.

test 5
Evaluates a conditional
expression.

then 5 R Part of an **if** construct.


**Command Chapter Type Summary**

time R

```
Run command pipeline
and print execution
times. The format of the
output can be controlled
with TIMEFORMAT.
```
times

```
Print the accumulated
user and system times for
processes run from the
shell.
```
trap 8
Set up a signal-catching
routine.

type 3
Identify the source of a
command.

typeset 6

```
Declare variables and
give them attributes.
Same as declare.
```

**Command Chapter Type Summary**

ulimit 10
Set/show process
resource limits.

umask 10
Set/show file permission
mask.

unalias 3 Remove alias definitions.

unset 3
Remove definitions of
variables or functions.

until 5 R Looping construct.

wait 8
Wait for background
job(s) to finish.

while 5 R Looping construct.


# Built-In Shell Variables

Table B-5shows a complete list of environment variables
available in _bash_ 3.0. The letters in the **Type** column of
the table have the following meanings: A = Array, L =
colon separated list, R = read-only, U = unsetting it
causes it to lose its special meaning.

Note that the variables beginning BASH_, beginning
COMP, DIRSTACK, FUNCNAME, GLOBIGNORE,
GROUPS, HISTIGNORE, HOSTNAME,
HISTTIMEFORMAT, LANG, LC_ALL,
LC_COLLATE, LC_MESSAGE, MACHTYPE,
PIPESTATUS, SHELLOPTS, and TIMEFORMAT are
not available in versions prior to 2.0. BASH_ENV
replaces ENV found in earlier versions.

**Table B-5. Environment variables**

```
Variable Chapter Type Description
```
#### * 4 R

```
The positional parameters
given to the current script
or function.
```

**Variable Chapter Type Description**

#### @ 4 R

```
The positional parameters
given to the current script
or function.
```
#### # 4 R

```
The number of arguments
given to the current script
or function.
```
#### - R

```
Options given to the shell
on invocation.
```
#### ? 5 R

```
Exit status of the previous
command.
```
#### R

```
Last argument to the
previous command.
```
#### $ 8 R

```
Process ID of the shell
process.
```

**Variable Chapter Type Description**

#### ! 8 R

```
Process ID of the last
background command.
```
#### 0 4 R

```
Name of the shell or shell
script.
```
#### BASH 3

```
The full pathname used to
invoke this instance of
bash.
```
#### BASH_ARGC 9 A

```
An array of values which
are the number of
parameters in each frame
of the current bash
execution call stack. The
number of parameters to
the current subroutine
(shell function or script
executed with. or source)
is at the top of the stack.
```

**Variable Chapter Type Description**

#### BASH_ARGV 9 A

```
All of the parameters in
the current bash execution
call stack. The final
parameter of the last
subroutine call is at the top
of the stack; the first
parameter of the initial call
is at the bottom.
```
#### BASH_COMMAND 9

```
The command currently
being executed or about to
be executed, unless the
shell is executing a
command as the result of a
trap, in which case it is the
command executing at the
time of the trap.
```
#### BASH_EXECUTION_STRING

```
The command argument to
the -c invocation option.
```

**Variable Chapter Type Description**

#### BASH_ENV 3

```
The name of a file to run
as the environment file
when the shell is invoked.
```
#### BASH_LINENO 9 A

```
An array whose members
are the line numbers in
source files corresponding
to each member of
@var{FUNCNAME}.
${BASH_LINENO[$i]} is
the line number in the
source file where
${FUNCNAME[$i + 1]}
was called. The
corresponding source file
name is
${BASH_SOURCE[$i +
1]}.
```
#### BASH_REMATCH AR

```
An array whose members
are assigned by the =~
binary operator to the [[
conditional command. The
element with index 0 is the
```

**Variable Chapter Type Description**

```
portion of the string
matching the entire regular
expression. The element
with index n is the portion
of the string matching the
n th parenthesized
subexpression.
```
#### BASH_SOURCE 9 A

```
An array containing the
source filenames
corresponding to the
elements in the
FUNCNAME array
variable.
```
#### BASH_SUBSHELL

```
Incremented by one each
time a subshell or subshell
environment is spawned.
The initial value is 0.
```
#### BASH_VERSION 3

```
The version number of this
instance of bash.
```

**Variable Chapter Type Description**

#### BASH_VERSINFO 3,6 AR

```
Version information for
this instance of bash. Each
element of the array holds
parts of the version
number.
```
#### CDPATH 3 L

```
A list of directories for the
cd command to search.
```
#### COMP_CWORD

```
An index into
${COMP_WORDS} of
the word containing the
current cursor position.
This variable is available
only in shell functions
invoked by the
programmable completion
facilities.
```
#### COMP_LINE

```
The current command line.
This variable is available
only in shell functions and
external commands
```

**Variable Chapter Type Description**

```
invoked by the
programmable completion
facilities.
```
#### COMP_POINT

```
The index of the current
cursor position relative to
the beginning of the
current command. If the
current cursor position is
at the end of the current
command, the value of this
variable is equal to
${#COMP_LINE}. This
variable is available only
in shell functions and
external commands
invoked by the
programmable completion
facilities.
```
#### COMP_WORDBREAKS U

```
The set of characters that
the Readline library treats
as word separators when
performing word
```

**Variable Chapter Type Description**

```
completion. If
COMP_WORDBREAKS
is unset, it loses its special
properties, even if it is
subsequently reset.
```
#### COMP_WORDS A

```
An array of the individual
words in the current
command line. This
variable is available only
in shell functions invoked
by the programmable
completion facilities.
```
#### COMPREPLY A

```
The possible completions
generated by a shell
function invoked by the
programmable completion
facility.
```
#### DIRSTACK 4,6 ARU

```
The current contents of the
directory stack.
```

**Variable Chapter Type Description**

#### EUID R

```
The effective user ID of
the current user.
```
#### FUNCNAME 9 ARU

```
An array containing the
names of all shell
functions currently in the
execution call stack. The
element with index 0 is the
name of any
currently-executing shell
function. The bottom-most
element is "main". This
variable exists only when a
shell function is executing.
```
#### FCEDIT 2

```
The default editor for the
fc command.
```
#### FIGNORE L

```
A list of names to ignore
when doing filename
completion.
```

**Variable Chapter Type Description**

#### GLOBIGNORE L

```
A list of patterns defining
filenames to ignore during
pathname expansion.
```
#### GROUPS AR

```
An array containing a list
of groups of which the
current user is a member.
```
#### IFS 7

```
The Internal Field
Separator: a list of
characters that act as word
separators. Normally set to
SPACE, TAB, and
NEWLINE.
```
#### HISTCMD 3 U

```
The history number of the
current command.
```
#### HISTCONTROL 3

```
A list of patterns,
separated by colons (:),
which can have the
following values.
```

**Variable Chapter Type Description**

```
ignorespace : lines
beginning with a space are
not entered into the history
list. ignoredups : lines
matching the last history
line are not entered.
erasedups : all previous
lines matching the current
line to are removed from
the history list before the
line is saved. ignoreboth :
enables both ignorespace
and ignoredups.
```
#### HISTFILE 2

```
The name of the command
history file.
```
#### HISTIGNORE 3

```
A list of patterns to decide
what should be retained in
the history list.
```
#### HISTSIZE 2

```
The number of lines kept
in the command history.
```

**Variable Chapter Type Description**

#### HISTFILESIZE 3

```
The maximum number of
lines kept in the history
file.
```
#### HISTTIMEFORMAT 3

```
If set and not null, its
value is used as a format
string for strftime (3) to
print the time stamp
associated with each
history entry displayed by
the history builtin. If this
variable is set, time stamps
are written to the history
file so they may be
preserved across shell
sessions.
```
#### HOME 3

```
The home (login)
directory.
```
#### HOSTFILE 3

```
The file to be used for
hostname completion.
```

**Variable Chapter Type Description**

#### HOSTNAME

```
The name of the current
host.
```
#### HOSTTYPE 3

```
The type of machine bash
is running on.
```
#### IGNOREEOF 3

```
The number of EOF
characters received before
exiting an interactive shell.
```
INPUTRC 2 The _readline_ startup file.

#### LANG

```
Used to determine the
locale category for any
category not specifically
selected with a variable
starting with LC_.
```
#### LC_ALL

```
Overrides the value of
LANG and any other LC_
```

**Variable Chapter Type Description**

```
variable specifying a
locale category.
```
#### LC_COLLATE

```
Determines the collation
order used when sorting
the results of pathname
expansion.
```
#### LC_CTYPE

```
Determines the
interpretation of characters
and the behavior of
character classes within
pathname expansion and
pattern matching.
```
#### LC_MESSAGES

```
This variable determines
the locale used to translate
double-quoted strings
preceded by a $.
```

**Variable Chapter Type Description**

#### LC_NUMERIC

```
Determines the locale
category used for number
formatting.
```
#### LINENO 9 U

```
The number of the line
that just ran in a script or
function.
```
#### MACHTYPE

```
A string describing the
system on which bash is
executing.
```
#### MAIL 3

```
The name of the file to
check for new mail.
```
#### MAILCHECK 3

```
How often (in seconds) to
check for new mail.
```
#### MAILPATH 3 L

```
A list of file names to
check for new mail, if
MAIL is not set.
```

**Variable Chapter Type Description**

#### OLDPWD 3

```
The previous working
directory.
```
#### OPTARG 6

```
The value of the last
option argument processed
by getopts.
```
#### OPTERR 6

```
If set to 1, display error
messages from getopts.
```
#### OPTIND 6

```
The number of the first
argument after options.
```
#### OSTYPE

```
The operating system on
which bash is executing.
```
#### PATH 3 L

```
The search path for
commands.
```

**Variable Chapter Type Description**

#### PIPESTATUS 6 A

```
An array variable
containing a list of exit
status values from the
processes in the most
recently executed
foreground pipeline.
```
#### POSIXLY_CORRECT

```
If in the environment when
bash starts, the shell enters
posix mode before reading
the startup files, as if the
—posix invocation option
had been supplied. If it is
set while the shell is
running, bash enables
posix mode , as if the
command set -o posix had
been executed.
```
#### PROMPT_COMMAND

```
The value is executed as a
command before the
primary prompt is issued.
```

**Variable Chapter Type Description**

#### PS1 3

```
The primary command
prompt string.
```
#### PS2 3

```
The prompt string for line
continuations.
```
#### PS3 5

```
The prompt string for the
select command.
```
#### PS4 9

```
The prompt string for the
xtrace option.
```
#### PPID 8 R

```
The process ID of the
parent process.
```
#### PWD 3

```
The current working
directory.
```

**Variable Chapter Type Description**

#### RANDOM 9 U

```
A random number
between 0 and 32767
(215-1).
```
#### REPLY 5, 7

```
The user's response to the
select command; result of
the read command if no
variable names are given.
```
#### SECONDS 3 U

```
The number of seconds
since the shell was
invoked.
```
#### SHELL 3

```
The full pathname of the
shell.
```
#### SHELLOPTS LR

```
A list of enabled shell
options.
```

**Variable Chapter Type Description**

#### SHLVL

```
Incremented by one each
time an instance of bash is
invoked.
```
#### TIMEFORMAT

```
Specifies the format for
the output from using the
time reserved word on a
command pipeline.
```
#### TMOUT 10

```
If set to a positive integer,
the number of seconds
after which the shell
automatically terminates if
no input is received.
```
#### UID R

```
The user ID of the current
user.
```
auto_resume
Controls how job control
works.


**Variable Chapter Type Description**

histchars

```
Specifies what to use as
the history control
characters. Normally set to
the string `!^#'.
```

# Test Operators

Table B-6lists the operators that are used with **test** and
the **[** ... **]** and **[[** ... **]]** constructs. They can be logically
combined with **-a** ("and") and **-o** ("or") and grouped with
escaped parenthesis ( **\( ... \)** ). The string comparisons **<**
and **>** and the **[[** ... **]]** construct are not available in versions
of _bash_ prior to 2.0.

**Table B-6. Test operators**

```
Operator True if...
```
```
-a file file exists
```
```
-b file file exists and is a block device file
```
```
-c file file exists and is a character device file
```
```
-d file file exists and is a directory
```
```
-e file file exists; same as - a
```

**Operator True if...**

**-f** _file file_ exists and is a regular file

**-g** _file file_ exists and has its setgid bit set

**-G** _file_

```
file exists and is owned by the effective
group ID
```
**-h** _file file_ exists and is a symbolic link

**-k** _file file_ exists and has its sticky bit set

**-L** _file file_ exists and is a symbolic link

**-n** _string string_ is non-null

**-N** _file file_ was modified since it was last read


**Operator True if...**

**-O** _file
file_ exists and is owned by the effective
user ID

**-p** _file
file_ exists and is a pipe or named pipe
(FIFO file)

**-r** _file file_ exists and is readable

**-s** _file file_ exists and is not empty

**-S** _file file_ exists and is a socket

**-t** _N_ File descriptor _N_ points to a terminal

**-u** _file file_ exists and has its setuid bit set

**-w** _file file_ exists and is writeable


**Operator True if...**

**-x** _file
file_ exists and is executable, or _file_ is a
directory that can be searched

**-z** _string string_ has a length of zero

_fileA_ - **nt**
_fileB
fileA_ modification time is newer than _fileB_

_fileA_ - **ot**
_fileB
fileA_ modification time is older than _fileB_

_fileA_ - **ef**
_fileB
fileA_ and _fileB_ point to the same file

_stringA_ =
_stringB_

```
stringA equals stringB (POSIX version)
```
_stringA_ ==
_stringB
stringA_ equals _stringB_


**Operator True if...**

_stringA_ !=
_stringB
stringA_ does not match _stringB_

_stringA_ =~
_regexp_

```
stringA matches the extended regular
expression regexp[3]
```
_stringA_ <
_stringB_

```
stringA sorts before stringB
lexicographically
```
_stringA_ >
_stringB_

```
stringA sorts after stringB
lexicographically
```
_exprA_ - **eq**
_exprB_

```
Arithmetic expressions exprA and exprB
are equal
```
_exprA_ - **ne**
_exprB_

```
Arithmetic expressions exprA and exprB
are not equal
```
_exprA_ - **lt**
_exprB_

```
exprA is less than exprB
```

**Operator True if...**

_exprA_ - **gt**
_exprB
exprA_ is greater than _exprB_

_exprA_ - **le**
_exprB
exprA_ is less than or equal to _exprB_

_exprA_ - **ge**
_exprB_

```
exprA is greater than or equal to exprB
```
_exprA_ - **a**
_exprB
exprA_ is true and _exprB_ is true

_exprA_ - **o**
_exprB
exprA_ is true or _exprB_ is true

[3]Only available in _bash_ version 3.0 and later. May

only be used inside **[[** ... **]]**.


# set Options

Table B-7lists the options that can be turned on with the
**set -** _arg_ command. All are initially off except where
noted. **Full Name** s, where listed, are arguments to **set** that
can be used with **set -o**. The **Full Name** s **braceexpand** ,
**histexpand** , **history** , **keyword** , and **onecmd** are not
available in versions of _bash_ prior to 2.0. Also, in those
versions, hashing is switched with - **d**.

**Table B-7. Options to set**

```
Option Full name Meaning
```
```
-a allexport
Export all subsequently defined
or modified variables.
```
```
-B braceexpand
The shell performs brace
expansion. This is on by default.
```
```
-b notify
```
```
Report the status of terminating
background jobs immediately.
```

**Option Full name Meaning**

-C noclobber
Don't allow redirection to
overwrite existing files.

-E errtrace

```
Any trap on ERR is inherited by
shell functions, command
substitutions, and commands
executed in a subshell
environment.
```
-e errexit

```
Exit the shell when a simple
command exits with non-zero
status. A simple command is a
command not part of a while ,
until , or if ; or part of a && or ||
list; or a command whose return
value is inverted by!.
```
```
emacs
Use emacs -style command-line
editing.
```
-f noglob Disable pathname expansion.


**Option Full name Meaning**

-H histexpand

```
Enable! style history
substitution. On by default in an
interactive shell.
```
```
history
Enable command history. On by
default in interactive shells.
```
-h hashall
Disable the hashing of
commands.

```
ignoreeof
```
```
Disallow CTRL-D to exit the
shell.
```
-k keyword
Place keyword arguments in the
environment for a command.

-m monitor
Enable job control (on by
default in interactive shells).


**Option Full name Meaning**

-n noexec

```
Read commands and check
syntax but do not execute them.
Ignored for interactive shells.
```
-P physical

```
Do not follow symbolic links on
commands that change the
current directory. Use the
physical directory.
```
-p privileged Script is running in _suid_ mode.

```
pipefail
```
```
The return value of a pipeline is
the value of the last (rightmost)
command to exit with a
non-zero status, or zero if all
commands in the pipeline exit
successfully. This option is
disabled by default.
```
```
posix
```
```
Change the default behavior to
that of POSIX 1003.2 where it
differs from the standard.
```

**Option Full name Meaning**

-T functrace

```
Any trap on DEBUG is
inherited by shell functions,
command substitutions, and
commands executed in a
subshell environment.
```
-t onecmd
Exit after reading and executing
one command.

-u nounset

```
Treat undefined variables as
errors, not as null.
```
-v verbose
Print shell input lines before
running them.

```
vi
Use vi -style command-line
editing.
```
-x xtrace

```
Print commands (after
expansions) before running
them.
```

**Option Full name Meaning**

#### -

```
Signals the end of options. All
remaining arguments are
assigned to the positional
parameters. -x and -v are turned
off. If there are no remaining
arguments to set , the positional
arguments remain unchanged.
```
#### —

```
With no arguments following,
unset the positional parameters.
Otherwise, the positional
parameters are set to the
following arguments (even if
they begin with - ).
```

# shopt Options

The _shopt_ options are set with **shopt -s** _arg_ and unset
with **shopt -u** _arg_. SeeTable B-8for options to _shopt_.
Versions of _bash_ prior to 2.0 had environment variables
to perform some of these settings. Setting them equated to
**shopt -s**.

The variables (and corresponding shopt options) were:
**allow_null_glob_expansion** ( **nullglob** ), **cdable_vars**
( **cdable_vars** ), **command_oriented_history** ( **cmdhist** ),
**glob_dot_filenames** ( **dotglob** ), **no_exit_on_failed_exec**
( **execfail** ). These variables no longer exist.

The options **extdebug** , **failglob** , **force_fignore** , and
**gnu_errfmt** are not available in versions of _bash_ prior to
3.0.

**Table B-8. Options to shopt**

```
Option Meaning if set
```
```
cdable_vars
```
```
An argument to cd that is
not a directory is assumed
to be the name of a
```

**Option Meaning if set**

```
variable whose value is
the directory to change to.
```
cdspell

```
Minor errors in the
spelling of a directory
supplied to the cd
command will be
corrected if there is a
suitable match. This
correction includes
missing letters, incorrect
letters, and letter
transposition. It works for
interactive shells only.
```
checkhash

```
Commands found in the
hash table are checked for
existence before being
executed and
non-existence forces a
PATH search.
```

**Option Meaning if set**

checkwinsize

```
Checks the window size
after each command and,
if it has changed, updates
the variables LINES and
COLUMNS accordingly.
```
cmdhist

```
Attempt to save all lines
of a multiline command in
a single history entry.
```
dotglob

```
Filenames beginning with
a. are included in
pathname expansion.
```
execfail

```
A non-interactive shell
will not exit if it cannot
execute the argument to
an exec. Interactive shells
do not exit if exec fails.
```
expand_aliases Aliases are expanded.


**Option Meaning if set**

extdebug

```
Behavior intended for use
by debuggers is enabled.
This includes: the -F
option of declare displays
the source filename and
line number
corresponding to each
function name supplied as
an argument; if the
command run by the
DEBUG trap returns a
non-zero value, the next
command is skipped and
not executed; and if the
command run by the
DEBUG trap returns a
value of 2, and the shell is
executing in a subroutine,
a call to return is
simulated.
```
extglob

```
Extended pattern
matching features are
enabled.
```

**Option Meaning if set**

failglob

```
Patterns which fail to
match filenames during
pathname expansion
result in an expansion
error.
```
force_fignore

```
The suffixes specified by
the FIGNORE shell
variable cause words to be
ignored when performing
word completion even if
the ignored words are the
only possible
completions.
```
gnu_errfmt

```
Shell error messages are
written in the standard
GNU error message
format.
```
histappend

```
The history list is
appended to the file
named by the value of the
```

**Option Meaning if set**

```
variable HISTFILE when
the shell exits, rather than
overwriting the file.
```
histreedit

```
If readline is being used,
the opportunity is given
for re-editing a failed
history substitution.
```
histverify

```
If readline is being used,
the results of history
substitution are not
immediately passed to the
shell parser. Instead, the
resulting line is loaded
into the readline editing
buffer, allowing further
modification.
```
hostcomplete

```
If readline is being used,
an attempt will be made
to perform hostname
completion when a word
```

**Option Meaning if set**

```
beginning with @ is being
completed.
```
huponexit

```
bash will send SIGHUP
to all jobs when an
interactive login shell
exits.
```
interactive_comments

```
Allows a word beginning
with # and all subsequent
characters on the line to
be ignored in an
interactive shell.
```
lithist

```
If the cmdhist option is
enabled, multiline
commands are saved to
the history with
embedded newlines rather
than using semicolon
separators where possible.
```

**Option Meaning if set**

login_shell

```
If bash is started as a
login shell. This is a
read-only value.
```
mailwarn

```
If the file being checked
for mail has been
accessed since the last
time it was checked, the
message "The mail in
mailfile has been read" is
displayed.
```
no_empty_cmd_completion

```
If readline is being used,
no attempt will be made
to search the PATH for
possible completions
when completion is
attempted on an empty
line.
```
nocaseglob

```
bash matches filenames in
a case-insensitive fashion
```

**Option Meaning if set**

```
when performing
pathname expansion.
```
nullglob

```
Allows patterns which
match no files to expand
to null strings rather than
to themselves.
```
progcomp

```
Programmable
completion facilities are
enabled. Default is on.
```
promptvars

```
Prompt strings undergo
variable and parameter
expansion after being
expanded.
```
restricted_shell

```
Set if the shell is started in
restricted mode. The
value cannot be changed.
```

**Option Meaning if set**

shift_verbose

```
The shift built-in prints an
error if it has shifted past
the last positional
parameter.
```
sourcepath

```
The source built-in uses
the value of PATH to
find the directory
containing the file
supplied as an argument.
```
xpg_echo

```
echo expands
backslash-escape
sequences by default.
```

# I/O Redirection

Table B-9shows a complete list of I/O redirectors. (This
table is also included earlier asTable 7-1.) Note that there
are two formats for specifying standard output and error
redirection: **&>file** and **>&file**. The second of these, and
the one used throughout this book, is the preferred way.

**Table B-9. I/O redirectors**

```
Redirector Function
```
```
cmd1 |
cmd2
```
```
Pipe; take standard output of cmd1 as
standard input to cmd2
```
```
> file Direct standard output to file
```
```
< file Take standard input from file
```
```
>> file
```
```
Direct standard output to file ; append to
file if it already exists
```

**Redirector Function**

**>|** _file_
Force standard output to _file_ even if
**noclobber** is set

_n_ >| _file_
Force output to _file_ from file descriptor _n_
even if **noclobber** set

**<>** _file_
Use _file_ as both standard input and
standard output

_n_ <> _file_
Use _file_ as both input and output for file
descriptor _n_

**<<** _label_ Here-document

_n_ **>** _file_ Direct file descriptor _n_ to _file_

_n_ **<** _file_ Take file descriptor _n_ from _file_


**Redirector Function**

**>>** _file_
Direct file descriptor _n_ to _file_ ; append to
_file_ if it already exists

_n_ >&
Duplicate standard output to file descriptor
_n_

_n_ <&
Duplicate standard input from file
descriptor _n_

_n_ >& _m_
File descriptor _n_ is made to be a copy of
the output file descriptor

_n_ <& _m_
File descriptor _n_ is made to be a copy of
the input file descriptor

**&>** _file_
Directs standard output and standard error
to _file_

<&- Close the standard input


**Redirector Function**

>&- Close the standard output

_n_ >&- Close the output from file descriptor _n_

_n_ <&- Close the input from file descriptor _n_

n>&word

```
If n is not specified, the standard output
(file descriptor 1) is used; if the digits in
word do not specify a file descriptor open
for output, a redirection error occurs; as a
special case, if n is omitted, and word does
not expand to one or more digits, the
standard output and standard error are
redirected as described previously
```
n<&word

```
If word expands to one or more digits, the
file descriptor denoted by n is made to be
a copy of that file descriptor; if the digits
in word do not specify a file descriptor
open for input, a redirection error occurs;
if word evaluates to -, file descriptor n is
```

**Redirector Function**

```
closed; if n is not specified, the standard
input (file descriptor 0) is used
```
n>&digit-

```
Moves the file descriptor digit to file
descriptor n , or the standard output (file
descriptor 1) if n is not specified
```
n<&digit-

```
Moves the file descriptor digit to file
descriptor n , or the standard input (file
descriptor 0) if n is not specified; digit is
closed after being duplicated to n
```

# emacs Mode

# Commands

Table B-10shows a complete list of _emacs_ editing mode
commands.

**Table B-10. emacs mode commands**

```
Command Meaning
```
```
CTRL-A Move to beginning of line
```
```
CTRL-B Move backward one character
```
```
CTRL-D Delete one character forward
```
```
CTRL-E Move to end of line
```
```
CTRL-F Move forward one character
```

**Command Meaning**

#### CTRL-G

```
Abort the current editing command and
ring the terminal bell
```
CTRL-J Same as RETURN

CTRL-K Delete (kill) forward to end of line

CTRL-L Clear screen and redisplay the line

CTRL-M Same as RETURN

CTRL-N Next line in command history

#### CTRL-O

```
Same as RETURN, then display next
line in history file
```
CTRL-P Previous line in command history


**Command Meaning**

CTRL-R Search backward

CTRL-S Search forward

CTRL-T Transpose two characters

#### CTRL-U

```
Kill backward from point to the
beginning of line
```
CTRL-V Make the next character typed verbatim

#### CTRL-V

#### TAB

```
Insert a TAB
```
#### CTRL-W

```
Kill the word behind the cursor, using
whitespace as the boundary
```
#### CTRL-X /

```
List the possible filename completions
of the current word
```

**Command Meaning**

#### CTRL-X ~

```
List the possible username completions
of the current word
```
#### CTRL-X $

```
List the possible shell variable
completions of the current word
```
#### CTRL-X @

```
List the possible hostname completions
of the current word
```
#### CTRL-X!

```
List the possible command name
completions of the current word
```
#### CTRL-X (

```
Begin saving characters into the current
keyboard macro
```
#### CTRL-X )

```
Stop saving characters into the current
keyboard macro
```
CTRL-X e
Re-execute the last keyboard macro
defined


**Command Meaning**

#### CTRL-X

#### CTRL-R

```
Read in the contents of the readline
initialization file
```
#### CTRL-X

#### CTRL-V

```
Display version information on this
instance of bash
```
CTRL-Y Retrieve (yank) last item killed

DEL Delete one character backward

CTRL-[ Same as ESC (most keyboards)

ESC-B Move one word backward

#### ESC-C

```
Change word after point to all capital
letters
```
ESC-D Delete one word forward


**Command Meaning**

ESC-F Move one word forward

#### ESC-L

```
Change word after point to all
lowercase letters
```
ESC-N Non-incremental forward search

ESC-P Non-incremental reverse search

ESC-R Undo all the changes made to this line

ESC-T Transpose two words

#### ESC-U

```
Change word after point to all
uppercase letters
```
#### ESC-CTRL-E

```
Perform shell alias, history, and word
expansion on the line
```

**Command Meaning**

ESC-CTRL-H Delete one word backward

#### ESC-CTRL-Y

```
Insert the first argument to the previous
command (usually the second word) at
point
```
ESC-DEL Delete one word backward

ESC-^ Perform history expansion on the line

ESC-< Move to first line of history file

ESC-> Move to last line of history file

#### ESC-.

```
Insert last word in previous command
line after point
```
ESC-_ Same as above


**Command Meaning**

#### TAB

```
Attempt filename completion on
current word
```
#### ESC-?

```
List the possible completions of the text
before point
```
#### ESC-/

```
Attempt filename completion on
current word
```
#### ESC-~

```
Attempt username completion on
current word
```
#### ESC-$

```
Attempt variable completion on current
word
```
#### ESC-@

```
Attempt hostname completion on
current word
```
#### ESC-!

```
Attempt command name completion on
current word
```

**Command Meaning**

#### ESC-TAB

```
Attempt completion from text in the
command history
```
#### ESC-~

```
Attempt tilde expansion on the current
word
```
#### ESC-\

```
Delete all the spaces and TABs around
point
```
#### ESC-*

```
Insert all of the completions that would
be generated by ESC-= before point
```
#### ESC-=

```
List the possible completions before
point
```
#### ESC-{

```
Attempt filename completion and
return the list to the shell enclosed
within braces
```

# vi Control Mode

# Commands

Table B-11shows a complete list of all _vi_ control mode
commands.

**Table B-11. vi control mode commands**

```
Command Meaning
```
```
h Move left one character
```
```
l Move right one character
```
```
w Move right one word
```
```
b Move left one word
```
```
W Move to beginning of next non-blank word
```

**Command Meaning**

#### B

```
Move to beginning of preceding non-blank
word
```
**e** Move to end of current word

**E** Move to end of current non-blank word

**0** Move to beginning of line

. Repeat the last **a** insertion.

**^** Move to first non-blank character in line

**$** Move to end of line

**i** Insert text before current character

**a** Insert text after current character


**Command Meaning**

**I** Insert text at beginning of line

**A** Insert text at end of line

**R** Overwrite existing text

dh Delete one character backward

dl Delete one character forward

db Delete one word backward

dw Delete one word forward

dB Delete one non-blank word backward

dW Delete one non-blank word forward


**Command Meaning**

d$ Delete to end of line

d0 Delete to beginning of line

D Equivalent to **d$** (delete to end of line)

dd Equivalent to **0d$** (delete entire line)

#### C

```
Equivalent to c$ (delete to end of line,
enter input mode)
```
cc
Equivalent to **0c$** (delete entire line, enter
input mode)

x
Equivalent to **dl** (delete character
forwards)

#### X

```
Equivalent to dh (delete character
backwards)
```

**Command Meaning**

k or - Move backward one line

j or + Move forward one line

G Move to line given by repeat count

**/** _string_ Search forward for _string_

**?** _string_ Search backward for _string_

n Repeat search forward

N Repeat search backward

**f** _x_ Move right to next occurrence of _x_

**F** _x_ Move left to previous occurrence of _x_


**Command Meaning**

**t** _x_
Move right to next occurrence of _x_ , then
back one space

**T** _x_
Move left to previous occurrence of _x_ , then
forward one space

; Redo last character finding command

#### ,

```
Redo last character finding command in
opposite direction
```
\ Do filename completion

#### *

```
Do wildcard expansion (onto command
line)
```
\= Do wildcard expansion (as printed list)


**Command Meaning**

#### ~

```
Invert (twiddle) case of current
character(s)
```
#### \_

```
Append last word of previous command,
enter input mode
```
#### CTRL-L

```
Start a new line and redraw the current line
on it
```
#### #

```
Prepend # (comment character) to the line
and send it to history
```

# Appendix C. Loadable

# Built-Ins

_bash_ 2.0 introduced a new feature that increased the
flexibility of the shell: dynamically loadable built-ins. On
systems that support dynamic loading, you can write your
own built-ins in C, compile them into shared objects, and
load them at any time from within the shell with the
**enable** built-in (seeChapter 7 for details on all of the
**enable** options).

This appendix will discuss briefly how to go about
writing a built-in and loading it in _bash_. The discussion
assumes that you have experience with writing,
compiling, and linking C programs.

The _bash_ archive contains a number of pre-written
built-ins in the directory _examples/loadables/_. You can
build them by uncommenting the lines in the file _Makefile_
that are relevant to your system, and typing **make**. We'll
take one of these built-ins, **tty** , and use it as a "case
study" for built-ins in general.

**tty** will mimic the standard UNIX command _tty_. It will
print the name of the terminal that is connected to
standard input. The built-in will, like the command,
return true if the device is a TTY and false if it isn't. In
addition, it will take an option, **-s** , which specifies that it


should work silently, i.e., print nothing and just return a
result.

The C code for a built-in can be divided into three distinct
sections: the code that implements the functionality of the
built-in, a help text message definition, and a structure
describing the built-in so that _bash_ can access it.

The description structure is quite straightforward and
takes the form:

```
struct builtin structname = {
" builtin_name ",
function_name ,
BUILTIN_ENABLED,
help_array ,
" usage ",
0
};
```
_builtin_name_ is the name of the built-in as it appears in
_bash_. The next field, _function-name_ , is the name of the C
function that implements the built-in. We'll look at this in
a moment. BUILTIN_ENABLED is the initial state of the
built-in, whether it is enabled or not. This field should
always be set to BUILTIN_ENABLED. _help_array_ is an
array of strings which are printed when **help** is used on
the built-in. _usage_ is the shorter form of help; the
command and its options. The last field in the structure
should be set to 0.


In our example we'll call the built-in **tty** , the C function
**tty_builtin** , and the help array **tty_doc**. The usage string
will be **tty [-s]**. The resulting structure looks like this:

```
struct builtin tty_struct = {
"tty",
tty_builtin,
BUILTIN_ENABLED,
tty_doc,
"tty [-s]",
0
};
```
The next section is the code that does the work. It looks
like this:

```
tty_builtin (list)
WORD_LIST *list;
{
int opt, sflag;
char *t;
```
```
reset_internal_getopt ( );
sflag = 0;
while ((opt = internal_getopt (list, "s")) != -1)
{
switch (opt)
{
case 's':
sflag = 1;
break;
default:
builtin_usage ( );
return (EX_USAGE);
}
}
list = loptend;
```

```
t = ttyname (0);
if (sflag == 0)
puts (t? t : "not a tty");
return (t? EXECUTION_SUCCESS : EXECUTION_FAILURE);
}
```
Built-in functions are always given a pointer to a list of
type WORD_LIST. If the built-in doesn't actually take
any options, you must call _no_options(list)_ and check its
return value before any further processing. If the return
value is non-zero, your function should immediately
return with the value EX_USAGE.

You must always use _internal_getopt_ rather than the
standard C library _getopt_ to process the built-in options.
Also, you must reset the option processing first by calling
_reset_internal_getopt_.

Option processing is performed in the standard way,
except if the options are incorrect, in which case you
should return EX_USAGE. Any arguments left after
option processing are pointed to by _loptend_. Once the
function is finished, it should return the value
EXECUTION_SUCCESS or EXECUTION_FAILURE.

In the case of our **tty** built-in, we then just call the
standard C library routine _ttyname_ , and if the **-s** option
wasn't given, print out the name of the tty (or "not a tty" if
the device wasn't). The function then returns success or
failure, depending upon the result from the call to
_ttyname_.


The last major section is the help definition. This is
simply an array of strings, the last element of the array
being NULL. Each string is printed to standard output
when **help** is run on the built-in. You should, therefore,
keep the strings to 76 characters or less (an 80-character
standard display minus a 4-character margin). In the case
of **tty** , our help text looks like this:

```
char *tty_doc[] = {
"tty writes the name of the terminal that is opened for standard",
"input to standard output. If the `-s' option is supplied, nothing",
"is written; the exit status determines whether or not the standard",
"input is connected to a tty.",
(char *)NULL
};
```
The last things to add to our code are the necessary C
header files. These are _stdio.h_ and the _bash_ header files
_config.h_ , _builtins.h_ , _shell.h_ , and _bashgetopt.h_.

Here is the C program in its entirety:

```
#include "config.h"
#include <stdio.h>
#include "builtins.h"
#include "shell.h"
#include "bashgetopt.h"
```
```
extern char *ttyname ( );
```
```
tty_builtin (list)
WORD_LIST *list;
{
int opt, sflag;
```

```
char *t;
```
```
reset_internal_getopt ( );
sflag = 0;
while ((opt = internal_getopt (list, "s")) != -1)
{
switch (opt)
{
case 's':
sflag = 1;
break;
default:
builtin_usage ( );
return (EX_USAGE);
}
}
list = loptend;
```
t = ttyname (0);
if (sflag == 0)
puts (t? t : "not a tty");
return (t? EXECUTION_SUCCESS : EXECUTION_FAILURE);
}

char *tty_doc[] = {
"tty writes the name of the terminal that is opened for standard",
"input to standard output. If the `-s' option is supplied, nothing",
"is written; the exit status determines whether or not the standard",
"input is connected to a tty.",
(char *)NULL
};

struct builtin tty_struct = {
"tty",
tty_builtin,
BUILTIN_ENABLED,
tty_doc,


```
"tty [-s]",
0
};
```
We now need to compile and link this as a dynamic
shared object. Unfortunately, different systems have
different ways to specify how to compile dynamic shared
objects.Table C-1 lists some common systems and the
commands needed to compile and link _tty.c_. Replace
_archive_ with the path of the top level of the _bash_ archive.

**Table C-1. Shared object compilation**

```
System Commands
```
```
SunOS 4
cc -pic -I archive -I archive /builtins
-I archive /lib -c tty.c
```
```
ld -assert pure-text -o tty tty.o
```
```
SunOS 5
cc -K pic -I archive -I archive /builtins
-I archive /lib -c tty.c
```
```
cc -dy -z text -G -i -h tty -o tty tty.o
```

**System Commands**

#### SVR4,

SVR4.2, Irix

```
cc -K PIC -I archive -I archive /builtins
-I archive /lib -c tty.c
```
```
ld -dy -z text -G -h tty -o tty tty.o
```
#### AIX

```
cc -K -I archive -I archive /builtins
-I archive /lib -c tty.c
```
```
ld -bdynamic -bnoentry -bexpall -G -o
tty tty.o
```
Linux

```
cc -fPIC -I archive -I archive /builtins
-I archive /lib -c tty.c
```
```
ld -shared -o tty tty.o
```
NetBSD,
FreeBSD

```
cc -fpic -I archive -I archive /builtins
-I archive /lib -c tty.c
```

```
System Commands
```
```
ld -x -Bshareable -o tty tty.o
```
After you have compiled and linked the program, you
should have a shared object called **tty**. To load this into
_bash_ , just type **enable -f** _path/_ **tty tty** , where _path_ is the
full pathname of the shared object. You can remove a
loaded built-in at any time with the **-d** option, e.g., **enable
-d tty**.

You can put as many built-ins as you like into one shared
object; all you need are the three main sections that we
saw above for each built-in in the same C file. It is best,
however, to keep the number of built-ins per shared
object small. You will also probably find it best to keep
similar built-ins, or built-ins that work together (e.g.,
**pushd** , **popd** , **dirs** ), in the same shared object.

_bash_ loads a shared object as a whole, so if you ask it to
load one built-in from a shared object that has twenty
built-ins, it will load all 20 (but only one will be enabled).
For this reason, keep the number of built-ins small to save
loading memory with unnecessary things, and group
similar built-ins so that if the user enables one of them, all
of them will be loaded and ready in memory for enabling.


# Appendix D. Programmable

# Completion

Programmable completion is a feature that was
introduced in _bash_ 2.0.[1]It extends the built-in textual
completion that is discussed inChapter 2 by providing
hooks into the completion mechanism. This means that it
is possible to write virtually any form of completion
desired. For instance, if you were typing the _man_
command, wouldn't it be nice to be able to hit TAB and
have the manual sections listed for you. Programmable
completion allows you to do this and much more.

This Appendix will only look at the basics of
programmable completion. While completion is a feature
you are very likely to use in everyday shell operation, you
are unlikely to need to delve into the inner depths and
actually write your own completion code. Fortunately the
feature has been around for some time and there are
already several libraries of completion commands
developed by other people. We'll just outline the basic
commands and procedures needed to use the completion
mechanism should you ever need to work on it yourself.

In order to be able to do textual completion in a particular
way you first have to tell the shell how to do it when you
press the TAB key. This is done via the **complete**
command.


The main argument of **complete** is a name that can be the
name of a command or anything else that you want
textual completion to work with. As an example we will
look at the _gunzip_ command that allows compressed
archives of various types to be uncompressed. Normally,
if you were to type:[2]

```
$ gunzip [TAB][TAB]
```
you would get a list of filenames from which to complete.
This list will include all kinds of things that are unsuitable
for the gunzip command. What we really would like is
the subset of those files that are suitable for the command
to work on. We can set this up by using **complete** :[3]

```
complete -A file -X '!*.@(Z|gz|tgz)' gunzip
```
Here we are telling completion mechanism that when the
_gunzip_ command is typed in we want it to do something
special. The - **A** flag is an _action_ and takes a variety of
arguments. In this case we provide **file** as the argument,
which asks the mechanism to provide a list of files as
possible completions. The next step is to cut this down by
selecting only the files that we know will work with
_gunzip_. We've done this with the - **X** option, which takes
as its argument a filter pattern. When applied to the
completion list the filter removes anything matching the
pattern, i.e., the result is everything that _doesn't_ match the
pattern. _gunzip_ can uncompress a number of file types
including those with the extensions. _Z_ ,. _gz_ , and. _tgz_. We
want to match all filenames with extensions that have one


of these three patterns. We then have to negate this with a
_!_ (remember, the filter removes the patterns that match).

We can actually try this out first and see what
completions would be returned without having to install
the completion with **complete**. We can do this via the
**compgen** command:

```
compgen -A file -X '!*.@(Z|gz|tgz)'
```
This produces a list of completion strings (assuming you
have some files in the current directory with these
extensions). **compgen** is useful for trying out filters to see
what completion strings are produced. It is also needed
when more complex completion is required. We'll see an
example of this later in the Appendix.

Once we install the **complete** command above, either by
sourcing a script with it in or executing it on the
command line, we can use the augmented completion
mechanism with the _gunzip_ command:

```
$ gunzip [TAB][TAB]
archive.tgz archive1.tgz file.Z
$ gunzip
```
You can probably see that there are other things we could
do. What about providing a list of possible arguments for
specific options to a command? For instance, the **kill**
command can takes a process ID but can optionally take a
signal name preceded by a dash (-) or a signal name
following the option **-n**. We should be able to complete


with PIDs but, if there is a dash or a _-n_ , with signal
names.

This is slightly more complex than the one-line example
above. Here we will need some code to distinguish what
has already been typed in. We'll also need to get the PIDs
and the signal names. We'll put the code in a function and
call the function via the completion mechanism. Here's
the code to call our function, which we'll name __kill_ :

```
complete -F _kill kill
```
The - **F** option to **complete** tells it to call the function
named __kill_ when it is performing textual completion for
the **kill** command. The next step is to code the function:

```
_kill( )
{
local cur
local sign
COMPREPLY=( )
cur=${COMP_WORDS[COMP_CWORD]}
if (($COMP_CWORD == 2)) && [[ ${COMP_WORDS[1]} == -n ]]; then
# return list of available signals
_signals
elif (($COMP_CWORD == 1 )) && [[ "$cur" == -* ]]; then
# return list of available signals
sign="-"
_signals
else
# return list of available PIDs
COMPREPLY=( $( compgen -W '$( command ps axo pid | sed 1d )' $cur ) )
fi
}
```

The code is fairly standard apart from the use of some
special environment variables and a call to a function
called __signals_ , which we'll come to shortly.

The variable COMPREPLY is used to hold the result that
is returned back to the completion mechanism. It is an
array that holds a set of completion strings. Initially this
is set to an empty array.

The local variable _cur_ is a convenience variable to make
the code more readable because the value is used in
several places. Its value is derived from an element in the
array COMP_WORDS. This array holds the individual
words on the current command line. COMP_CWORD is
an index into the array; it gives the word containing the
current cursor position. The value of _cur_ is the word
currently containing the cursor.

The first _if_ statement tests for the condition where the **kill**
command is followed by the - **n** option. If the first word
was - _n_ and we are on the second word, then we need to
provide a list of signal names for the completion
mechanism.

The second _if_ statement is similar, except this time we are
looking to complete on the current word, which starts
with a dash and is followed by anything else. The body of
this _if_ again calls __signals_ but this time it sets the sign
variable to a dash. The reason for this will become
obvious when we look at the __signals_ function.


The remaining part in the _else_ block returns a list of
process IDs. This uses the **compgen** command to help in
creating the array of completion strings. First it runs the
_ps_ command to obtain a list of PIDs and then pipes the
result through _sed_ to remove the first line (which is the
heading "PID").[4]This is then given as an argument to
the - **W** option of **compgen** , which takes a word list.
**compgen** then returns all completion strings that match
the value of the variable _cur_ and the resulting array is
assigned to COMPREPLY.

**compgen** is important here because we can't just return
the complete list of PIDs provided by _ps_. The user may
have already typed part of a PID and then attempted
completion. As the partial PID will be in the variable _cur_ ,
**compgen** restricts the results to those that match or
partially match that value. For example if _cur_ had the
value 5 then **compgen** would return only values
beginning with a "5", such as 5, 59 or 562.

The last piece of the puzzle is the __signals_ function:

```
_signals( )
{
local i
COMPREPLY=( $( compgen -A signal SIG${cur#-} ))
for (( i=0; i < ${#COMPREPLY[@]}; i++ )); do
COMPREPLY[i]=$sign${COMPREPLY[i]#SIG}
done
}
```
While we can get a list of signal names by using
**complete -A signal** , the names are unfortunately not in a


form that is very usable and so we can't use this to
directly generate the array of names. The names
generated begin with the letters "SIG" while the names
needed by the **kill** command don't. The __signal_ function
should assign to COMPREPLY an array of signal names,
optionally preceded by a dash.

First we generate the list of signal names with **compgen**.
Each name starts with the letters "SIG". In order to get
**complete** to provide the correct subset if the user has
begun to type a name, we add "SIG" to the beginning of
the value in _cur_. We also take the opportunity to remove
any preceding dash that the value has so it will match.

We then loop on the array removing the letters "SIG" and
adding a dash if needed (the value of the variable _sign_ ) to
each entry.

Both **complete** and **compgen** have many other options
and actions; far more than we can cover in a few simple
exercises. If you are interested in taking programmable
completion further, we recommend looking in the _bash_
manual and downloading some of the many examples that
are available on the Internet or in the _bash_ archive under
_bash-3.0\examples\complete_.

As you can see, textual completion can get quite involved
and creating the necessary code can be time-consuming.
Fortunately there are already completion libraries
available for _bash_. One of these is the bash Completion


Project, which can be found at [http://freshmeat.net/](http://freshmeat.net/)
projects/bashcompletion/.

[1]Technically it was added in _bash_ Version 2.04.

[2]For the rest of this Appendix we will denote typing a

TAB character as [TAB].

[3]In order for@(...) to work you will need extended

pattern matching switched on ( **shopt -s extglob** ).

[4]On AIX and Solaris you will have to use the command

**ps -efo pid**.


# Colophon

Our look is the result of reader comments, our own
experimentation, and feedback from distribution
channels. Distinctive covers complement our distinctive
approach to technical topics, breathing personality and
life into potentially dry subjects.

The animal on the cover of Learning the bash Shell, Third
Edition, is a silver bass, one of the 400-500 species of sea
bass. The silver bass, also known as the white perch, is
found in freshwater bays and river mouths along the
Atlantic coast of North America from Nova Scotia to
South Carolina, and is most abundant in the Chesapeake
region. Silver bass live in large schools and feed on small
fishes and crustaceans. Although many bass never stray
far from one place their whole lives, silver bass swim
upstream to spawn, often becoming landlocked in the
process. Like most bass, the s\ ilver bass is attracted to
bright, shiny objects, and can be drawn quite close to
swimmers and divers in this way.

Colleen Gorman was the production editor and
copyeditor for Learning the bash Shell, Third Edition.
MaryAnne Weeks Mayo, Lydia Onofrei, and Emily Quill
provided quality control. Peter Ryan provided production
assistance. Angela Howard wrote the index.


Edie Freedman designed the cover of this book. The
cover image is a 19th-century engraving from the Dover
Pictorial Archive. Karen Montgomery produced the cover
layout with Adobe InDesign CS using Adobe's ITC
Garamond font.

David Futato designed the interior layout. This book was
converted by Judy Hoer to FrameMaker 5.5.6 with a
format conversion tool created by Erik Ray, Jason
McIntosh, Neil Walls, and Mike Sierra that uses Perl and
XML technologies. The text font is Linotype Birka; the
heading font is Adobe Myriad Condensed; and the code
font is LucasFont's TheSans Mono Condensed. The
illustrations that appear in the book were created by Chris
Reilley and updated for the third edition by Robert
Romano, Jessamyn Read, and Lesley Borash using
Macromedia FreeHand MX and Adobe Photoshop CS.
This colophon was written by Clairemarie Fisher
O'Leary.

The online edition of this book was created by the Digital
Books production group (John Chodacki, Ken Douglass,
and Ellie Cutler) using a set of Frame-to-XML
conversion and cleanup tools written and maintained by
Erik Ray, Benn Salter, John Chodacki, Ellie Cutler, and
Jeff Liggett.


