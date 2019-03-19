# Chapter 3. Customizing

# Your Environment

An _environment_ is a collection of concepts that express
the things a computer system or other set of tools does in
terms designed to be understandable and coherent, and a
look and feel that is comfortable. For example, your desk
at work is an environment. Concepts involved in desk
work usually include memos, phone calls, letters, forms,
etc. The tools on or in your desk that you use to deal with
these things include paper, staples, envelopes, pens, a
telephone, a calculator, etc. Every one of these has a set
of characteristics that express how you use it; such
characteristics range from location on your desk or in a
drawer (for simple tools) to more sophisticated things like
which numbers the memory buttons on your phone are set
to. Taken together, these characteristics make up your
desk's look and feel.

You customize the look and feel of your desk
environment by putting pens where you can most easily
reach them, programming your phone buttons, etc. In
general, the more customization you have done, the more
tailored to your personal needs—and therefore the more
productive—your environment is.

Similarly, UNIX shells present you with such concepts as
files, directories, and standard input and output, while


UNIX itself gives you tools to work with these, such as
file manipulation commands, text editors, and print
queues. Your UNIX environment's look and feel is
determined by your keyboard and display, of course, but
also by how you set up your directories, where you put
each kind of file, and what names you give to files,
directories, and commands. There are also more
sophisticated ways of customizing your shell
environment.

This chapter will look at the four most important features
that _bash_ provides for customizing your environment.

**_Special files_**

```
The files .bash_profile , .bash_logout , and .bashrc
that are read by bash when you log in and out or start
a new shell.
```
**_Aliases_**

```
Synonyms for commands or command strings that
you can define for convenience.
```
**_Options_**

```
Controls for various aspects of your environment that
you can turn on and off.
```
**_Variables_**

```
Changeable values that are referred to by a name.
The shell and other programs can modify their
```

```
behavior according to the values stored in the
variables.
```
Although these features are not the only ones available,
they form the basis for doing more advanced
customization. They are also the features that are
common to the various shells available on UNIX. Later
chapters will cover more advanced shell features, such as
the ability to program the shell.

# The .bash_profile,

# .bash_logout, and

# .bashrc Files

Three files in your home directory have a special
meaning to _bash_ , providing a way for you to set up your
account environment automatically when you log in and
when you invoke another _bash_ shell, and allowing you to
perform commands when you log out. These files may
already exist in your home directory, depending on how
your system administrator has set up your account. If they
don't exist, your account is using only the default system
file _/etc/profile_. You can easily create your own _bash_ files
using your favorite text editor. If you are unfamiliar with
text editors available under UNIX, we suggest that you
familiarize yourself with one of the better-known ones
such as _vi_ or _emacs_ before proceeding further with the
techniques described in this chapter.


The most important _bash_ file, _.bash_profile_ , is read and
the commands in it executed by _bash_ every time you log
in to the system. If you examine your _.bash_profile_ you
will probably see lines similar to:

```
PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/bin
SHELL=/bin/bash
MANPATH=/usr/man:/usr/X11/man
EDITOR=/usr/bin/vi
```
```
PS1='\h:\w\$ '
PS2='> '
export EDITOR
```
These lines define the basic environment for your login
account. For the moment, it is probably best to leave
these lines alone until you understand what they do.
When editing your _.bash_profile_ , just add your new lines
after the existing ones.

Note that whatever you add to your _.bash_profile_ won't
take effect until the file is re-read by logging out and then
logging in again. Alternatively, you can also use the
_source_ command.[1]For example:

```
source .bash_profile
```
**source** executes the commands in the specified file, in
this case _.bash_profile_ , including any commands that you
have added.

_bash_ allows two synonyms for _.bash_profile_ : _.bash_login_ ,
derived from the C shell's file named _.login_ , and _.profile_ ,
derived from the Bourne shell and Korn shell files named


_.profile_. Only one of these three is read when you log in.
If _.bash_profile_ doesn't exist in your home directory, then
_bash_ will look for _.bash_login_. If that doesn't exist it will
look for _.profile_.

One advantage of _bash_ 's ability to look for either
synonym is that you can retain your _.profile_ if you have
been using the Bourne shell. If you need to add
_bash_ -specific commands, you can put them in
_.bash_profile_ followed by the command **source .profile**.
When you log in, all the _bash_ -specific commands will be
executed, and _bash_ will source _.profile_ , executing the
remaining commands. If you decide to switch to using the
Bourne shell you don't have to modify your existing files.
A similar approach was intended for _.bash_login_ and the
C shell _.login_ , but due to differences in the basic syntax
of the shells, this is not a good idea.

_.bash_profile_ is read and executed only by the login shell.
If you start up a new shell (a _subshell_ ) by typing **bash** on
the command line, it will attempt to read commands from
the file _.bashrc_. This scheme allows you the flexibility to
separate startup commands needed at login time from
those you might need when you run a subshell. If you
need to have the same commands run regardless of
whether it is a login shell or a subshell, you can just use
the _source_ command from within _.bash_profile_ to execute
_.bashrc_. If _.bashrc_ doesn't exist then no commands are
executed when you start up a subshell.


The file _.bash_logout_ is read and executed every time a
login shell exits. It is provided to round out the
capabilities for customizing your environment. If you
wanted to execute some commands that remove
temporary files from your account or record how much
time you have spent logged in to the system then you
would place the commands in _.bash_logout_. This file
doesn't have to exist in your account—if it isn't there
when you log out, then no extra commands are executed.

[1]You can also use the synonymous command dot (.).


# Aliases

If you have used UNIX for any length of time you will
have noticed that there are many commands available and
that some of them have cryptic names. Sometimes the
commands you use the most have a string of options and
arguments that need to be specified. Wouldn't it be nice if
there was a feature that let you rename the commands or
allowed you to type in something simple instead of half a
dozen options? Fortunately, _bash_ provides such a feature:
the alias.[2]

Aliases can be defined on the command line, in your
_.bash_profile_ , or in your _.bashrc_ , using this form:

```
alias name=command
```
This syntax specifies that _name_ is an alias for _command_.
Whenever you type **name** as a command, _bash_ will
substitute _command_ in its place when it executes the line.
Notice that there are no spaces on either side of the equal
sign ( **=** ); this is the required syntax.

There are a few basic ways to use an alias. The first, and
simplest, is as a more mnemonic name for an existing
command. Many commonly used UNIX commands have
names that are poor mnemonics and are therefore
excellent candidates for aliasing, the classic example
being:


```
alias search=grep
```
_grep_ , the UNIX file-searching utility, was named as an
acronym for something like "Generalized Regular
Expression Parser."[3] This acronym may mean
something to a computer scientist, but not to the office
administrator who has to find **Fred** in a list of phone
numbers. If you have to find **Fred** and you have the word
_search_ defined as an alias for _grep_ , you can type:

```
$ search Fred phonelist
```
Some people who aren't particularly good typists like to
use aliases for typographical errors they make often. For
example:

```
alias emcas=emacs
alias mali=mail
alias gerp=grep
```
This can be handy, but we feel you're probably better off
suffering with the error message and getting the correct
spelling under your fingers. Another common way to use
an alias is as a shorthand for a longer command string.
For example, you may have a directory to which you
need to go often. It's buried deep in your directory
hierarchy, so you want to set up an alias that will allow
you to **cd** there without typing (or even remembering) the
entire pathname:

```
alias cdvoy='cd sipp/demo/animation/voyager'
```

Notice the quotes around the full **cd** command; these are
necessary if the string being aliased consists of more than
one word.[4]

As another example, a useful option to the _ls_ command is
**-F** : it puts a slash ( **/** ) after directory files and an asterisk
( ***** ) after executable files. Since typing a dash followed by
a capital letter is inconvenient, many people define an
alias like this:

```
alias lf='ls -F'
```
A few things about aliases are important to remember.
First, _bash_ makes a textual substitution of the alias for
that which it is aliasing; it may help to imagine _bash_
passing your command through a text editor or word
processor and issuing a "change" or "substitute"
command before interpreting and executing it. Any
special characters (such as wildcards like ***** and**?** ) that
result when the alias is expanded are interpreted properly
by the shell.[5]For example, to make it easier to print all
of the files in your directory, you could define the alias:

```
alias printall='pr * | lpr'
```
Second, keep in mind that aliases are recursive, which
means that it is possible to alias an alias. A legitimate
objection to the previous example is that the alias, while
mnemonic, is too long and doesn't save enough typing. If
we want to keep this alias but add a shorter abbreviation,
we could define:

```
alias pa=printall
```

With recursive aliasing available it would seem possible
to create an infinite loop:

```
alias ls='ls -l'
```
_bash_ ensures that this loop cannot happen, because only
the first word of the replacement text is checked for
further aliasing; if that word is identical to the alias being
expanded, it is not expanded a second time. The above
command will work as expected (typing **ls** produces a
long list with permissions, sizes, owners, etc.), while in
more meaningless situations such as:

```
alias listfile=ls
alias ls=listfile
```
the alias _listfile_ is ignored.

Aliases can be used only for the beginning of a command
string—albeit with certain exceptions. In the **cd** example
above, you might want to define an alias for the directory
name alone, not for the entire command. But if you
define:

```
alias anim=sipp/demo/animation/voyager
```
and then type **cd anim** , _bash_ will probably print a
message like **anim: No such file or directory**.

An obscure feature of _bash_ 's alias facility—one not
present in the analogous C shell feature—provides a way
around this problem. If the value of an alias (the right side
of the equal sign) ends in a blank, then _bash_ tries to do
alias substitution on the next word on the command line.


To make the value of an alias end in a blank, you need to
surround it with quotes.

Here is how you would use this capability to allow aliases
for directory names, at least for use with the **cd**
command. Just define:

```
alias cd='cd '
```
This causes _bash_ to search for an alias for the directory
name argument to **cd** , which in the previous example
would enable it to expand the alias _anim_ correctly.

Another way to define a directory variable for use with
the **cd** command is to use the environment variable
**cdable_vars** , discussed later in this chapter.

Finally, there are a few useful adjuncts to the basic **alias**
command. If you type **alias** _name_ without an equal sign
( **=** ) and value, the shell will print the alias's value or **alias**
_name_ **not found** if it is undefined. If you type **alias**
without any arguments, you get a list of all the aliases you
have defined. The command **unalias** _name_ removes any
alias definition for its argument.

Aliases are very handy for creating a comfortable
environment, but they have essentially been superseded
by shell scripts and functions, which we will look at in
the next chapter. These give you everything aliases do
plus much more, so if you become proficient at them, you
may find that you don't need aliases anymore. However,
aliases are ideal for novices who find UNIX to be a rather


forbidding place, full of terseness and devoid of good
mnemonics. Chapter 4 shows the order of precedence
when, for example, an alias and a function have the same
name.

[2]C shell users should note that the _bash_ alias feature

does not support arguments in alias expansions, as C shell
aliases do. This functionality is provided by _functions_ ,
which we'll look at inChapter 4.

[3] Another theory has it that _grep_ stands for the

command "g/re/p", in the old _ed_ text editor, which does
essentially the same thing as _grep_.

[4]This contrasts with C shell aliases, in which the quotes

aren't required.

[5]An important corollary: wildcards and other special

characters cannot be used in the names of aliases, i.e., on
the left side of the equal sign.


# Options

While aliases let you create convenient names for
commands, they don't really let you change the shell's
behavior. _Options_ are one way of doing this. A shell
option is a setting that is either "on" or "off." While
several options relate to arcane shell features that are of
interest only to programmers, those that we will cover
here are of interest to all users.

The basic commands that relate to options are **set -o**
_optionname_ and **set +** o _optionname_. You can change
more than one option with the one **set** command by
preceding each _optionname_ with a **-o** or **+o**. The use of
plus ( **+** ) and minus ( **-** ) signs is counterintuitive: the **-** turns
the named option on, while the **+** turns it off. The reason
for this incongruity is that the dash ( **-** ) is the conventional
UNIX way of specifying options to a command, while the
use of **+** is an afterthought.

Most options also have one-letter abbreviations that can
be used in lieu of the **set -o** command; for example, **set -o
noglob** can be abbreviated **set -f**. These abbreviations are
carryovers from the Bourne shell. Like several other
"extra" _bash_ features, they exist to ensure upward
compatibility; otherwise, their use is not encouraged.

Table 3-1lists the options that are useful to general UNIX
users. All of them are off by default except as noted.


**Table 3-1. Basic shell options**

```
Option Description
```
```
emacs Enters emacs editing mode (on by default)
```
```
ignoreeof
```
```
Doesn't allow use of a single CTRL-D to
log off; use the exit command to log off
immediately (this has the same effect as
setting the shell variable IGNOREEOF=10)
```
```
noclobber
Doesn't allow output redirection ( > ) to
overwrite an existing file
```
```
noglob
```
```
Doesn't expand filename wildcards like *
and? (wildcard expansion is sometimes
called globbing )
```
```
nounset
```
```
Indicates an error when trying to use a
variable that is undefined
```
```
vi Enters vi editing mode
```

There are several other options (21 in all;Appendix B
lists them). To check the status of an option, just type **set
-o**. _bash_ will print a list of all options along with their
settings.

## shopt

_bash_ 2.0 introduced a new built-in for configuring shell
behaviour, **shopt**. This built-in is meant as a replacement
for option configuration originally done through
environment variables and the **set** command.[6]

The **shopt -o** functionality is a duplication of parts of the
**set** command and is provided for completeness on the
part of **shopt** , while retaining backward compatibility by
its continued inclusion in **set**.

The format for this command is **shopt** _options
option-names_. Table 3-2lists **shopt** 's options.

**Table 3-2. Options to shopt**

```
Option Meaning
```
```
-p
Displays a list of the settable options and their
current values
```

```
Option Meaning
```
```
-s Sets each option name
```
```
-u Unsets each option name
```
```
-q
```
```
Suppresses normal output; the return status
indicates if a variable is set or unset
```
```
-o
```
```
Allows the values of the option names to be
those defined for the -o option of the set
command
```
The default action is to unset (turn off) the named
options. If no options and arguments are given, or the **-p**
option is used, **shopt** displays a list of the settable options
and the values that they currently have. If **-s** or **-u** is also
given, the list is confined to only those options that are set
or unset, respectively.

A list of the most useful option names is given inTable
3-3. A complete list is given inAppendix B.

**Table 3-3. shopt option names**


**Option Meaning**

cdable_vars

```
If set, an argument to the cd built-in
command that is not a directory is
assumed to be the name of a variable
whose value is the directory to change to.
```
checkhash

```
If set, bash checks that a command found
in the hash table exists before trying to
execute it. If a hashed command no longer
exists, a normal path search is performed.
```
cmdhist

```
If set, bash attempts to save all lines of a
multiple-line command in the same
history entry.
```
dotglob

```
If set, bash includes filenames beginning
with a. (dot) in the results of pathname
expansion.
```
execfail

```
If set, a non-interactive shell will not exit
if it cannot execute the file specified as an
argument to the exec command. An
interactive shell does not exit if exec fails.
```

```
Option Meaning
```
```
histappend
```
```
If set, the history list is appended to the
file named by the value of the HISTFILE
variable when the shell exits, rather than
overwriting the file.
```
```
lithist
```
```
If set, and the cmdhist option is enabled,
multiline commands are saved to the
history with embedded newlines, rather
than using semicolon separators where
possible.
```
```
mailwarn
```
```
If set, and a file that bash is checking for
mail has been accessed since the last time
it was checked, the message "The mail in
mailfile has been read" is displayed.
```
We'll look at the use of the various options later in this
chapter.

[6]Appendix Bprovides a complete list of **shopt** shell

options and the corresponding environment variables in
earlier versions of the shell.


# Shell Variables

There are several characteristics of your environment that
you may want to customize but that cannot be expressed
as an on/off choice. Characteristics of this type are
specified in shell variables. Shell variables can specify
everything from your prompt string to how often the shell
checks for new mail.

Like an alias, a shell variable is a name that has a value
associated with it. _bash_ keeps track of several built-in
shell variables; shell programmers can add their own. By
convention, built-in variables should have names in all
capital letters. _bash_ does, however, have two
exceptions.[7] The syntax for defining variables is
somewhat similar to the syntax for aliases:

```
varname = value
```
There must be no space on either side of the equal sign,
and if the value is more than one word, it must be
surrounded by quotes. To use the value of a variable in a
command, precede its name by a dollar sign ($).

You can delete a variable with the command **unset**
_varname_. Normally this isn't useful, since all variables
that don't exist are assumed to be null, i.e., equal to the
empty string "". But if you use the **set** option **nounset** ,
which causes the shell to indicate an error when it


encounters an undefined variable, then you may be
interested in **unset**.

The easiest way to check a variable's value is to use the
**echo** built-in command. All **echo** does is print its
arguments, but not until the shell has evaluated them.
This includes—among other things that will be discussed
later—taking the values of variables and expanding
filename wildcards. So, if the variable **wonderland** has
the value **alice** , typing:

```
$ echo "$wonderland"
```
will cause the shell to simply print **alice**. If the variable is
undefined, the shell will print a blank line. A more
verbose way to do this is:

```
$ echo "The value of \$
varname
is \"$
varname
\"."
```
The first dollar sign and the inner double quotes are
backslash-escaped (i.e., preceded with **\** so the shell
doesn't try to interpret them—see Chapter 1) so they
appear literally in the output, which for the above
example would be:

```
The value of $wonderland is "alice".
```

## Variables and Quoting

Notice that we used double quotes around variables (and
strings containing them) in these **echo** examples. In
Chapter 1, we said that some special characters inside
double quotes are still interpreted, while none are
interpreted inside single quotes.

A special character that "survives" double quotes is the
dollar sign—meaning that variables are evaluated. It's
possible to do without the double quotes in some cases;
for example, we could have written the above **echo**
command this way:

```
$ echo The value of \$
varname
is \"$
varname \".
```
But double quotes are more generally correct. Here's why.
Suppose we did this:

```
$ fred='Four spaces between these words.'
```
Then if we entered the command **echo $fred** , the result
would be:

```
Four spaces between these words.
```
What happened to the extra spaces? Without the double
quotes, the shell splits the string into words after
substituting the variable's value, as it normally does when


it processes command lines. The double quotes
circumvent this part of the process (by making the shell
think that the whole quoted string is a single word).

Therefore the command **echo** " **$fred** " prints this:

```
Four spaces between these words.
```
The distinction between single and double quotes
becomes particularly important when we start dealing
with variables that contain user or file input later on.

Double quotes also allow other special characters to
work, as we'll see inChapter 4,Chapter 6, andChapter 7.
But for now, we'll revise the "When in doubt, use single
quotes" rule inChapter 1by adding, "...unless a string
contains a variable, in which case you should use double
quotes."


## Built-In Variables

As with options, some built-in shell variables are
meaningful to general UNIX users, while others are
arcana for hackers. We'll look at the more generally
useful ones here, and we'll save some of the more obscure
ones for later chapters. Again, Appendix Bcontains a
complete list.

### Editing mode variables

Several shell variables relate to the command-line editing
modes that we saw in the previous chapter. These are
listed inTable 3-4.

**Table 3-4. Editing mode variables**

```
Variable Meaning
```
#### HISTCMD

```
The history number of the
current command.
```

**Variable Meaning**

#### HISTCONTROL

```
A list of patterns, separated by
colons (:), which can have the
following values. ignorespace :
lines beginning with a space are
not entered into the history list.
ignoredups : lines matching the
last history line are not entered.
erasedups : all previous lines
matching the current line are
removed from the history list
before the line is saved.
ignoreboth : enables both
ignorespace and ignoredups. [8]
```
#### HISTIGNORE

```
A list of patterns, separated by
colons (:), used to decide which
command lines to save in the
history list. Patterns are
considered to start at the
beginning of the command line
and must fully specify the line,
i.e., no wildcard ( * ) is implicitly
appended. The patterns are
checked against the line after
HISTCONTROL is applied.
```

**Variable Meaning**

```
An ampersand ( & ) matches the
previous line. An explicit &
may be generated by escaping it
with a backslash.[9]
```
#### HISTFILE

```
Name of history file in which
the command history is saved.
The default is ~/.bash_history.
```
#### HISTFILESIZE

```
The maximum number of lines
to store in the history file. The
default is 500. When this
variable is assigned a value, the
history file is truncated, if
necessary, to the given number
of lines.
```
#### HISTSIZE

```
The maximum number of
commands to remember in the
command history. The default is
500.
```

**Variable Meaning**

#### HISTTIMEFORMAT

```
If it is set and not null, its value
is used as a format string for
strftime(3) to print the time
stamp associated with each
history entry displayed by the
history command. Time stamps
are written to the history file so
they may be preserved across
shell sessions.[10]
```
#### FCEDIT

```
Pathname of the editor to use
with the fc command.
```
[8] _history_control_ is synonymous with _HISTCONTROL_

in versions of _bash_ prior to 2.0. Versions prior to 1.14
only define **history_control**. _ignoreboth_ is not available
in _bash_ versions prior to 1.14. **HISTCONTROL** is a
colon-separated list, and _erasedups_ has been added in
bash 3.0 and later.

[9]This variable is not available in versions of _bash_

prior to 2.0.


```
Variable Meaning
```
```
[10]This variable is not available in versions of bash
prior to 3.0.
```
In the previous chapter, we saw how _bash_ numbers
commands. To find out the current command number in
an interactive shell, you can use the **HISTCMD**. Note
that if you unset **HISTCMD** , it will lose its special
meaning, even if you subsequently set it again.

We also saw in the last chapter how _bash_ keeps the
history list in memory and saves it to a file when you exit
a shell session. The variables **HISTFILESIZE** and
**HISTSIZE** allow you to set the maximum number of
lines that the shell saves in the history file, and the
maximum number of lines to "remember" in the history
list, i.e., the lines that it displays with the **history**
command.

Suppose you wanted to maintain a small history file in
your home directory. By setting **HISTFILESIZE** to 100,
you immediately cause the history file to allow a
maximum of 100 lines. If it is already larger than the size
you specify, it will be truncated.

**HISTSIZE** works in the same way, but only on the
history that the current shell has in memory. When you


exit an interactive shell, **HISTSIZE** will be the maximum
number of lines saved in your history file. If you have
already set **HISTFILESIZE** to be less than **HISTSIZE** ,
the saved list will be truncated.

You can also cut down on the size of your history file and
history list by use of the **HISTCONTROL** variable. This
is a colon-separated list of values. If it includes
**ignorespace** , any commands that you type that start with
a space won't appear in the history. Even more useful is
the **ignoredups** option. This discards consecutive entries
from the history list that are duplicated. Suppose you
want to monitor the size of a file with _ls_ as it is being
created. Normally, every time you type _ls_ it will appear in
your history. By setting **HISTCONTROL** to
**ignoredups** , only the first _ls_ will appear in the history.

The variable **HISTIGNORE** allows you to specify a list
of patterns which the command line is checked against. If
the command line matches one of the patterns, it is not
entered into the history list. You can also request that it
ignore duplicates by using the pattern **&**.

For example, suppose you didn't want any command
starting with _l_ , nor any duplicates, to appear in the
history. Setting **HISTIGNORE** to **l*:&** will do just that.
Just as with other pattern matching we have seen, the
wildcard after the _l_ will match any command line starting
with that letter.


Another useful variable is **HISTTIMEFORMAT** , which
prepends a time stamp to each history entry showing
when the command was executed. If it is unset or the
value is null then no time stamp is written. If a format is
given then time stamps are inserted using the specified
format as part of the history and are shown with the
**history** command.

The time stamp formats are shown inTable 3-5. Some of
the results will be displayed using the particular format
for the underlying locale, e.g., weekday names will be
translated into the language being used on the system.

**Table 3-5. Time stamp formats**

```
Format Replaced by
```
```
%a The locale's abbreviated weekday name
```
```
%A The locale's full weekday name
```
```
%b The locale's abbreviated month name
```
```
%B The locale's full month name
```

**Format Replaced by**

%c
The locale's appropriate date and time
representation

#### %C

```
The century number (the year divided by 100
and truncated to an integer) as a decimal
number [00-99]
```
%d
The day of the month as a decimal number
[01-31]

#### %D

```
The date in American format; the same value
as %m/%d/%y.
```
%e
The day of the month as a decimal number
[1-31]; a single digit is preceded by a space

%h The same as %b

#### %H

```
The hour (24-hour clock) as a decimal
number [00-23]
```

**Format Replaced by**

#### %I

```
The hour (12-hour clock) as a decimal
number [01-12]
```
%j
The day of the year as a decimal number
[001-366]

%m The month as a decimal number [01-12]

%M The minute as a decimal number [00-59]

%n A newline character

%p The locale's equivalent of either a.m. or p.m

%r

```
The time in a.m. and p.m. notation; in the
POSIX locale this is equivalent to
%I:%M:%S %p
```
%R The time in 24-hour notation (%H:%M)


**Format Replaced by**

%S The second as a decimal number [00-61]

%t A tab character

%T The time (%H:%M:%S)

%u
The weekday as a decimal number [1-7], with
1 representing Monday

#### %U

```
The week number of the year (Sunday as the
first day of the week) as a decimal number
[00-53]
```
#### %V

```
The week number of the year (Monday as the
first day of the week) as a decimal number
[01-53]; if the week containing 1 January has
four or more days in the new year, then it is
considered week 1—otherwise, it is the last
week of the previous year, and the next week
is week 1
```

**Format Replaced by**

%w
The weekday as a decimal number [0-6], with
0 representing Sunday

#### %W

```
The week number of the year (Monday as the
first day of the week) as a decimal number
[00-53]; all days in a new year preceding the
first Monday are considered to be in week 0
```
%x The locale's appropriate date representation

%X The locale's appropriate time representation

%y
The year without century as a decimal number
[00-99]

%Y The year with century as a decimal number

#### %Z

```
The timezone name or abbreviation, or by
nothing if no timezone information exists
```

```
Format Replaced by
```
#### %% %

If you wanted to have the date and time with each history
entry, you could put:

```
HISTTIMEFORMAT="%y/%m/%d %T "
```
then the output of the **history** command would look
something like:

```
...
78 04/11/26 17:14:05 HISTTIMEFORMAT="%y/%m/%d %T "
79 04/11/26 17:14:08 ls -l
80 04/11/26 17:14:09 history
```
If the history has never had a date format set before then
all of the entries prior to setting the variable will get the
time stamp of the time the variable was set. If you set
**HISTTIMEFORMAT** to null and then set it to a format,
the previous time stamps are retained and displayed in the
new format.


### Mail variables

Since the _mail_ program is not running all the time, there
is no way for it to inform you when you get new mail;
therefore the shell does this instead.[11]The shell can't
actually check for incoming mail, but it can look at your
mail file periodically and determine whether the file has
been modified since the last check. The variables listed in
Table 3-6let you control how this works.

**Table 3-6. Mail variables**

```
Variable Meaning
```
#### MAIL

```
Name of file to check for incoming
mail
```
#### MAILCHECK

```
How often, in seconds, to check for
new mail (default 60 seconds)
```
#### MAILPATH

```
List of filenames, separated by colons
(:), to check for incoming mail
```

Under the simplest scenario, you use the standard UNIX
mail program, and your mail file is _/usr/mail/yourname_ or
something similar. In this case, you would just set the
variable **MAIL** to this filename if you want your mail
checked:

```
MAIL=/usr/mail/yourname
```
If your system administrator hasn't already done it for
you, put a line like this in your _.bash_profile_.

However, some people use nonstandard mailers that use
multiple mail files; **MAILPATH** was designed to
accommodate this. _bash_ will use the value of **MAIL** as
the name of the file to check, unless **MAILPATH** is set;
in which case, the shell will check each file in the
**MAILPATH** list for new mail. You can use this
mechanism to have the shell print a different message for
each mail file: for each mail filename in **MAILPATH** ,
append a question mark followed by the message you
want printed.

For example, let's say you have a mail system that
automatically sorts your mail into files according to the
username of the sender. You have mail files called _/usr/
mail/you/martin_ , _/usr/mail/you/geoffm_ , _/usr/mail/you/
paulr_ , etc. You define your **MAILPATH** as follows:

```
MAILPATH=/usr/mail/you/martin:/usr/mail/you/geoffm:\
/usr/mail/you/paulr
```

If you get mail from Martin Lee, the file _/usr/mail/you/
martin_ will change. _bash_ will notice the change within
one minute and print the message:

```
You have new mail in /usr/mail/you/martin
```
If you are in the middle of running a command, the shell
will wait until the command finishes (or is suspended) to
print the message. To customize this further, you could
define **MAILPATH** to be:

```
MAILPATH="\
/usr/mail/you/martin?You have mail from Martin.:\
/usr/mail/you/geoffm?Mail from Geoff has arrived.:\
/usr/mail/you/paulr?There is new mail from Paul."
```
The backslashes at the end of each line allow you to
continue your command on the next line. But be careful:
you can't indent subsequent lines. Now, if you get mail
from Martin, the shell will print:

```
You have mail from Martin.
```
You can also use the variable **$_** in the message to print
the name of the current mail file. For example:

```
MAILPATH='/usr/mail/you?You have some new mail in $_'
```
When new mail arrives, this will print the line:

```
You have some new mail in /usr/mail/you
```
The ability to receive notification of mail can be switched
on and off by using the **mailwarn** option to the **shopt**
command.


### Prompting variables

If you have seen enough experienced UNIX users at
work, you may already have realized that the shell's
prompt is not engraved in stone. Many of these users
have all kinds of things encoded in their prompts. It is
possible to put useful information into the prompt,
including the date and the current directory. We'll give
you some of the information you need to modify your
own here; the rest will come in the next chapter.

Actually , _bash_ uses four prompt strings. They are stored
in the variables **PS1** , **PS2** , **PS3** , and **PS4**. The first of
these is called the primary prompt string; it is your usual
shell prompt, and its default value is " **\s-\v\$** ".[12]Many
people like to set their primary prompt string to
something containing their login name. Here is one way
to do this:

```
PS1="\u--> "
```
The \u tells _bash_ to insert the name of the current user
into the prompt string. If your user name is **alice** , your
prompt string will be " **alice—>** ". If you are a C shell user
and, like many such people, are used to having a history
number in your prompt string, _bash_ can do this similarly
to the C shell: if the sequence **\!** is used in the prompt
string, it will substitute the history number. Thus, if you
define your prompt string to be:


```
PS1="\u \!--> "
```
then your prompts will be like **alice 1—>** , **alice 2—>** ,
and so on.

But perhaps the most useful way to set up your prompt
string is so that it always contains your current directory.
This way, you needn't type **pwd** to remember where you
are. Here's how:

```
PS1="\w--> "
```
Table 3-7 lists the prompt customizations that are
available.[13]

**Table 3-7. Prompt string customizations**

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
patchlevel (e.g., 2.00.0)
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


```
Command Meaning
```
#### \[

```
Begin a sequence of non-printing
characters, such as terminal control
sequences
```
```
\] End a sequence of non-printing characters
```
**PS2** is called the secondary prompt string; its default
value is **>**. It is used when you type an incomplete line
and hit RETURN, as an indication that you must finish
your command. For example, assume that you start a
quoted string but don't close the quote. Then if you hit
RETURN, the shell will print **>** and wait for you to finish
the string:

```
$ echo "This is a long line,
# PS1 for the command
> which is terminated down here"
# PS2 for the continuation
$
# PS1 for the next command
```
**PS3** and **PS4** relate to shell programming and debugging.
They will be explained inChapter 5, andChapter 9.


### Command search path

Another important variable is **PATH** , which helps the
shell find the commands you enter.

As you probably know, every command you use is
actually a file that contains code for your machine to
run.[14] These files are called executable files or just
executables for short. They are stored in various
directories. Some directories, like _/bin_ or _/usr/bin_ , are
standard on all UNIX systems; some depend on the
particular version of UNIX you are using; some are
unique to your machine; if you are a programmer, some
may even be your own. In any case, there is no reason
why you should have to know where a command's
executable file is in order to run it.

That is where **PATH** comes in. Its value is a list of
directories that the shell searches every time you enter a
command;[15] the directory names are separated by
colons (:), just like the files in **MAILPATH**.

For example, if you type **echo $PATH** , you will see
something like this:

```
/bin:/usr/bin:/usr/local/bin:/usr/X386/bin
```
Why should you care about your path? There are two
main reasons. First, once you have read the later chapters
of this book and you try writing your own shell programs,


you will want to test them and eventually set aside a
directory for them. Second, your system may be set up so
that certain restricted commands' executable files are kept
in directories that are not listed in **PATH**. For example,
there may be a directory _/usr/games_ in which there are
executables that are verboten during regular working
hours.

Therefore you may want to add directories to your
**PATH**. Let's say you have created a _bin_ directory under
your login directory, which is _/home/you_ , for your own
shell scripts and programs. To add this directory to your
**PATH** so that it is there every time you log in, put this
line in your _.bash_profile_ :

```
PATH=$PATH":/home/you/bin"
```
This line sets **PATH** to whatever it was before, followed
immediately by a colon and _/home/you/bin_.

This is the safe way of doing it. When you enter a
command, the shell searches directories in the order they
appear in **PATH** until it finds an executable file.
Therefore, if you have a shell script or program whose
name is the same as an existing command, the shell will
use the existing command—unless you type in the
command's full pathname to make it clear. For example,
if you have created your own version of the _more_
command in the above directory and your **PATH** is set up
as in the last example, you will need to type **/home/you/
bin/more** (or just **~/bin/more** ) to get your version.


The more reckless way of resetting your path is to put
your own directory before the other directories:

```
PATH="/home/you/bin:"$PATH
```
This is unsafe because you are trusting that your own
version of the _more_ command works properly. But it is
also risky for a more important reason: system security. If
your **PATH** is set up in this way, you leave open a "hole"
that is well known to computer crackers and mischief
makers: they can install "Trojan horses" and do other
things to steal files or do damage. (SeeChapter 10 for
more details.) Therefore, unless you have complete
control of (and confidence in) everyone who uses your
system, use the first of the two methods of adding your
own command directory.

If you need to know which directory a command comes
from, you need not look at directories in your **PATH**
until you find it. The shell built-in command **type** prints
the full pathname of the command you give it as
argument, or just the command's name and its type if it's a
built-in command itself (like **cd** ), an alias, or a function
(as we'll see inChapter 4).


### Command hashing

You may be thinking that having to go and find a
command in a large list of possible places would take a
long time, and you'd be right. To speed things up, _bash_
uses what is known as a _hash_ table.

Every time the shell goes and finds a command in the
search path, it enters it in the hash table. If you then use
the command again, _bash_ first checks the hash table to
see if the command is listed. If it is, it uses the path given
in the table and executes the command; otherwise, it just
has to go and look for the command in the search path.

You can see what is currently in the hash table with the
command **hash** :

```
$ hash
hits command
2 /bin/cat
1 /usr/bin/stat
2 /usr/bin/less
1 /usr/bin/man
2 /usr/bin/apropos
2 /bin/more
1 /bin/ln
3 /bin/ls
1 /bin/ps
2 /bin/vi
```

This not only shows the hashed commands, but how
many times they have been executed (the _hits_ ) during the
current login session.

Supplying a command name to **hash** forces the shell to
look up the command in the search path and enter it in the
hash table. You can also make _bash_ "forget" what is in
the hash table by using **hash -r** to remove everything in
the table or **hash -d** _name_ to remove the specified
name.[16] Another option, **-p** , allows you to enter a
command into the hash table, even if the command
doesn't exist.[17]

Command hashing can be turned on and off with the
**hashall** option to **set**. In general use, there shouldn't be
any need to turn it off.

Don't be too concerned about the details of hashing. The
command hashing and lookup is all done by _bash_ without
you knowing it's taking place.


### Directory search path and

### variables

**CDPATH** is a variable whose value, like that of **PATH** ,
is a list of directories separated by colons. Its purpose is
to augment the functionality of the **cd** built-in command.

By default, **CDPATH** isn't set (meaning that it is null),
and when you type **cd** _dirname_ , the shell will look in the
current directory for a subdirectory that is called
_dirname_ .[18]If you set **CDPATH** , you give the shell a list
of places to look for _dirname_ ; the list may or may not
include the current directory.

Here is an example. Consider the alias for the long **cd**
command from earlier in this chapter:

```
alias cdvoy='cd sipp/demo/animation/voyager'
```
Now suppose there were a few directories under this
directory to which you need to go often; they are called
_src_ , _bin_ , and _doc_. You define your **CDPATH** like this:

```
CDPATH=:~/sipp/demo/animation/voyager
```
In other words, you define your **CDPATH** to be the
empty string (meaning the current directory) followed by
_~/sipp/demo/animation/voyager_.


With this setup, if you type **cd doc** , then the shell will
look in the current directory for a (sub)directory called
_doc_. Assuming that it doesn't find one, it looks in the
directory _~/sipp/demo/animation/voyager_. The shell finds
the _doc_ directory there, so you go _directly_ there.

If you often find yourself going to a specific group of
directories as you work on a particular project, you can
use **CDPATH** to get there quickly. Note that this feature
will only be useful if you update it whenever your work
habits change.

_bash_ provides another shorthand mechanism for referring
to directories; if you set the shell option **cdable_vars**
using **shopt** ,[19] any argument supplied to the **cd**
command that is not a directory is assumed to be a
variable.

We might define the variable **anim** to be _~/sipp/demo/
animation/voyager_. If we set **cdable_vars** and then type:

```
cd anim
```
the current directory will become _~/sipp/demo/animation/
voyager_.


### Miscellaneous variables

We have covered the shell variables that are important
from the standpoint of customization. There are also
several that serve as status indicators and for various
other miscellaneous purposes. Their meanings are
relatively straightforward; the more basic ones are
summarized inTable 3-8.

**Table 3-8. Status variables**

```
Variable Meaning
```
#### HOME

```
Name of your home (login)
directory
```
#### SECONDS

```
Number of seconds since the shell
was invoked
```
#### BASH

```
Pathname of this instance of the
shell you are running
```

```
Variable Meaning
```
#### BASH_VERSION

```
The version number of the shell
you are running
```
#### BASH_VERSINFO

```
An array of version information
for the shell you are running
```
```
PWD Current directory
```
#### OLDPWD

```
Previous directory before the last
cd command
```
The shell sets the values of these variables, except
**HOME** (which is set by the login process: _login_ , _rshd_ ,
etc.). The first five are set at login time, the last two
whenever you change directories. Although you can also
set their values, just like any other variables, it is difficult
to imagine any situation where you would want to. In the
case of **SECONDS** , if you set it to a new value it will
start counting from the value you give it, but if you unset
**SECONDS** it will lose its special meaning, even if you
subsequently set it again.


[7] Versions prior to 2.0 have many more lowercase

built-in variables. Most of these are now obsolete, the
functionality having been moved to the **shopt** command.

[11]BSD UNIX users should note that the _biff_ command

on those systems does a better job of informing you about
new mail; while _bash_ only prints "you have new mail"
messages right before it prints command prompts, _biff_ can
do so at any time.

[12]In versions of _bash_ prior to 2.0, the default is " **bash\$**

".

[13]\a, \e, \H, \T, \@, \v, and \V are not available in

versions prior to 2.0. \D was introduced in _bash_ 2.05b.

[14]Unless it's a built-in command (one of those shown in

**boldface** , like **cd** and **echo** ), in which case the code is
simply part of the executable file for the entire shell.

[15]Unless the command name contains a slash (/), in

which case the search does not take place.

[16]The - **d** option is not available in versions of _bash_

prior to 2.05b.

[17]The **-p** option is not available in versions of _bash_

prior to 2.0.

[18]This search is disabled when _dirname_ starts with a

slash. It is also disabled when _dirname_ starts with **./** or **../**.


[19]In versions of _bash_ prior to 2.0, **cdable_vars** is a

shell variable that you can set and unset.


# Customization and

# Subprocesses

Some of the variables discussed above are used by
commands you may run—as opposed to the shell
itself—so that they can determine certain aspects of your
environment. The majority, however, are not even known
outside the shell.

This dichotomy begs an important question: which shell
"things" are known outside the shell, and which are only
internal? This question is at the heart of many
misunderstandings about the shell and shell
programming. Before we answer, we'll ask it again in a
more precise way: which shell "things" are known to
subprocesses? Remember that whenever you enter a
command, you are telling the shell to run that command
in a subprocess; furthermore, some complex programs
may start their own subprocesses.

Now for the answer, which (like many UNIX concepts) is
unfortunately not as simple as you might like. A few
things are known to subprocesses, but the reverse is not
true: subprocesses can never make these things known to
the processes that created them.

Which things are known depends on whether the
subprocess in question is a _bash_ program (seeChapter 4)


or an interactive shell. If the subprocess is a _bash_
program, then it's possible to propagate nearly every type
of thing we've seen in this chapter—options and
variables—plus a few we'll see later.

## Environment Variables

By default, only one kind of thing is known to all kinds of
subprocesses: a special class of shell variables called
_environment variables_. Some of the built-in variables we
have seen are actually environment variables: **HOME** ,
**MAIL** , **PATH** , and **PWD**.

It should be clear why these and other variables need to
be known by subprocesses. For example, text editors like
_vi_ and _emacs_ need to know what kind of terminal you are
using; the environment variable **TERM** is their way of
determining this. As another example, most UNIX mail
programs allow you to edit a message with your favorite
text editor. How does _mail_ know which editor to use? The
value of **EDITOR** (or sometimes **VISUAL** ).

Any variable can become an environment variable. First
it must be defined as usual; then it must be _exported_ with
the command:[20]

```
export varnames
```

( _varnames_ can be a list of variable names separated by
blanks). You can combine variable assignment and the
export into one statement:

```
export wonderland=alice
```
It is also possible to define variables to be in the
environment of a particular subprocess (command) only,
by preceding the command with the variable assignment,
like this:

```
varname = value command
```
You can put as many assignments before the command as
you want.[21]For example, assume that you're using the
_emacs_ editor. You are having problems getting it to work
with your terminal, so you're experimenting with different
values of **TERM**. You can do this most easily by entering
commands that look like:

```
TERM= trythisone
emacs
filename
```
_emacs_ will have _trythisone_ defined as its value of **TERM** ,
yet the environment variable in your shell will keep
whatever value (if any) it had before. This syntax is
surprisingly useful, but not very widely used; we won't
see it much throughout the remainder of this book.

Nevertheless, environment variables are important. Most
_.bash_profile_ files include definitions of environment


variables; the sample built-in _.bash_profile_ earlier in this
chapter contained six such definitions:

```
PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/bin
SHELL=/bin/bash
MANPATH=/usr/man:/usr/X11/man
EDITOR=/usr/bin/vi
PS1='\h:\w\$ '
PS2='> '
export EDITOR
```
You can find out which variables are environment
variables and what their values are by typing **export**
without arguments or by using the **-p** option to the
command.

Some environment variable names have been used by so
many applications that they have become standard across
many shell environments. These variables are not built
into _bash_ , although some shells, such as the Korn shell,
have them as built-ins.Table 3-9lists the ones you are
most likely to come across.

**Table 3-9. Standard variables**

```
Variable Meaning
```
#### COLUMNS

```
The number of columns your display
has[22]
```

```
Variable Meaning
```
```
EDITOR Pathname of your text editor
```
```
LINES The number of lines your display has
```
```
SHELL Pathname of the shell you are running
```
```
TERM The type of terminal that you are using
```
```
[22]Note that bash will set COLUMNS and LINES
during certain situations, such as when the window the
shell is in changes in size.
```
You may well find that some of these already exist in
your own environment, most likely set from the system
_/etc/profile_ file (seeChapter 10). You can define them
yourself in your _.bash_profile_ and **export** them, as we did
earlier.


### Terminal types

The variable **TERM** is vitally important for any program
that uses your entire screen or window, like a text editor.
Such programs include all screen editors (such as _vi_ and
_emacs_ ), _more_ , and countless third-party applications.

Because users are spending more and more time within
programs, and less and less using the shell itself, it is
extremely important that your **TERM** is set correctly. It's
really your system administrator's job to help you do this
(or to do it for you), but in case you need to do it yourself,
here are a few guidelines.

The value of **TERM** must be a short character string with
lowercase letters that appears as a filename in the
_terminfo_ database.[23] This database is a two-tiered
directory of files under the root directory _/usr/lib/
terminfo_. This directory contains subdirectories with
single-character names; these in turn contain files of
terminal information for all terminals whose names begin
with that character. Each file describes how to tell the
terminal in question to do certain common things like
position the cursor on the screen, go into reverse video,
scroll, insert text, and so on. The descriptions are in
binary form (i.e., not readable by humans).

Names of terminal description files are the same as that of
the terminal being described; sometimes an abbreviation
is used. For example, the DEC VT100 has a description
in the file _/usr/lib/terminfo/v/vt100_. An _xterm_ terminal


window under the X Window System has a description in
_/usr/lib/terminfo/x/xterm_.

Sometimes your UNIX software will set up **TERM**
incorrectly; this usually happens for X terminals and
PC-based UNIX systems. Therefore, you should check
the value of **TERM** by typing **echo $TERM** before
going any further. If you find that your UNIX system isn't
setting the right value for you (especially likely if your
terminal is of a different make from that of your
computer), you need to find the appropriate value of
**TERM** yourself.

The best way to find the **TERM** value—if you can't find
a local guru to do it for you—is to guess the _terminfo_
name and search for a file of that name under _/usr/lib/
terminfo_ by using _ls_. For example, if your terminal is a
Hewlett-Packard 70092, you could try:

```
$ cd /usr/lib/terminfo
$ ls 7/7*
```
If you are successful, you will see something like this:

```
70092 70092A 70092a
```
In this case, the three names are likely to be synonyms for
(links to) the same terminal description, so you could use
any one as a value of **TERM**. In other words, you could
put _any_ of these three lines in your _.bash_profile_ :


```
TERM=70092
TERM=70092A
TERM=70092a
```
If you aren't successful, _ls_ will print an error message, and
you will have to make another guess and try again. If you
find that _terminfo_ contains nothing that resembles your
terminal, all is not lost. Consult your terminal's manual to
see if the terminal can emulate a more popular model;
nowadays the odds for this are excellent.

Conversely, _terminfo_ may have several entries that relate
to your terminal, for submodels, special modes, etc. If
you have a choice of which entry to use as your value of
**TERM** , we suggest you test each one out with your text
editor or any other screen-oriented programs you use and
see which one works best.

The process is much simpler if you are using a
windowing system, in which your "terminals" are logical
portions of the screen rather than physical devices. In this
case, operating system-dependent software was written to
control your terminal window(s), so the odds are very
good that if it knows how to handle window resizing and
complex cursor motion, then it is capable of dealing with
simple things like **TERM**. The X Window System, for
example, automatically sets **xterm** as its value for **TERM**
in an _xterm_ terminal window.


### Other common variables

Some programs, such as _mail_ , need to know what type of
editor you would like to use. In most cases they will
default to a common editor like _ed_ unless you set the
**EDITOR** variable to the path of your favorite editor and
export it in your _.bash_profile_.

Some programs run shells as subprocesses within
themselves (e.g., many mail programs and the _emacs_
editor's shell mode); by convention they use the **SHELL**
variable to determine which shell to use. **SHELL** is
usually set by the process that invokes the login shell;
usually _login_ or something like _rshd_ if you are logged in
remotely. _bash_ sets it only if it hasn't already been set.

You may have noticed that the value of **SHELL** looks the
same as **BASH**. These two variables serve slightly
different purposes. **BASH** is set to the pathname of the
current shell, whether it is an interactive shell or not.
**SHELL** , on the other hand, is set to the name of your
login shell, which may be a completely different shell.

**COLUMNS** and **LINES** are used by screen-oriented
editors like _vi_. In most cases a default is used if they are
undefined, but if you are having display problems with
screen-oriented applications then you should check these
variables to see if they are correct.


## The Environment File

Although environment variables will always be known to
subprocesses, the shell must be explicitly told which
other variables, options, aliases, and so on, are to be
communicated to subprocesses. The way to do this is to
put all such definitions into the _environment file_. _bash_ 's
default environment file is the _.bashrc_ file that we
touched on briefly at the beginning of this chapter.

Remember, if you take your definitions out of
_.bash_profile_ and put them in _.bashrc_ , you will have to
have the line **source .bashrc** at the end of your
_.bash_profile_ so that the definitions become available to
the login shell.

The idea of the environment file comes from the C shell's
_.cshrc_ file. This is reflected in the choice of the name
_.bashrc_. The _rc_ suffix for initialization files is practically
universal throughout the UNIX world.[24]

As a general rule, you should put as few definitions as
possible in _.bash_profile_ and as many as possible in your
environment file. Because definitions add to rather than
take away from an environment, there is little chance that
they will cause something in a subprocess not to work
properly. (An exception might be name clashes if you go
overboard with aliases.)


The only things that really need to be in _.bash_profile_ are
environment variables and their exports and commands
that aren't definitions but actually run or produce output
when you log in. Option and alias definitions should go
into the environment file. In fact, there are many _bash_
users who have tiny _.bash_profile_ files, e.g.:

```
stty stop ^S intr ^C erase ^?
date
source .bashrc
```
Although this is a small _.bash_profile_ , this user's
environment file could be huge.

[20]Unless automatic exporting has been turned on by **set**

**-a** or **set -o allexport** , in which case all variables that are
assigned to will be exported.

[21]There is an obscure option, **set -k** , that lets you put

this type of environment variable definition _anywhere_ on
the command line, not just at the beginning.

[23]Note that most modern UNIX systems now use a

database rather than a flat file for the terminal
descriptions.

[24] According to the folklore, it stands for "run

commands" and has its origins in old DEC operating
systems.


# Customization Hints

You should feel free to try any of the techniques
presented in this chapter. The best strategy is to test
something out by typing it into the shell during your login
session; then if you decide you want to make it a
permanent part of your environment, add it to your
_.bash_profile_.

A nice, painless way to add to your _.bash_profile_ without
going into a text editor makes use of the **echo** command
and one of _bash_ 's editing modes. If you type a
customization command in and later decide to add it to
your _.bash_profile_ , you can recall it via CTRL-P or
CTRL-R (in emacs-mode) or **j** , **-** , or**?** (vi-mode). Let's
say the line is:

```
PS1="\u \!--> "
```
After you recall it, edit the line so that it is preceded by an
**echo** command, surrounded by _single_ quotes, and
followed by an I/O redirector that (as you will see in
Chapter 7) appends the output to _~/.bash_profile_ :

```
$ echo 'PS1="\u \!--> " ' >> ~/.bash_profile
```
Remember that the single quotes are important because
they prevent the shell from trying to interpret things like
dollar signs, double quotes, and exclamation points. Also


make sure that you use a double right-caret ( **>>** ). A single
one will overwrite the file rather than appending to it.


# Chapter 4. Basic Shell

# Programming

If you have become familiar with the customization
techniques we presented in the previous chapter, you
have probably run into various modifications to your
environment that you want to make but can't—yet. Shell
programming makes these possible.

_bash_ has some of the most advanced programming
capabilities of any command interpreter of its type.
Although its syntax is nowhere near as elegant or
consistent as that of most conventional programming
languages, its power and flexibility are comparable. In
fact, _bash_ can be used as a complete environment for
writing software prototypes.

Some aspects of _bash_ programming are really extensions
of the customization techniques we have already seen,
while others resemble traditional programming language
features. We have structured this chapter so that if you
aren't a programmer, you can read this chapter and do
quite a bit more than you could with the information in
the previous chapter. Experience with a conventional
programming language like Pascal or C is helpful (though
not strictly necessary) for subsequent chapters.
Throughout the rest of the book, we will encounter


occasional programming problems, called _tasks_ , whose
solutions make use of the concepts we cover.

# Shell Scripts and

# Functions

A _script_ (a file that contains shell commands) is a shell
program. Your _.bash_profile_ and environment files,
discussed in the previous chapter, are shell scripts.

You can create a script using the text editor of your
choice. Once you have created one, there are two ways to
run it. One, which we have already covered, is to type
**source** _scriptname_. This causes the commands in the
script to be read and run as if you typed them in.

The second way to run a script is simply to type its name
and hit RETURN, just as if you were invoking a built-in
command. This, of course, is the more convenient way.
This method makes the script look just like any other
UNIX command, and in fact several "regular" commands
are implemented as shell scripts (i.e., not as programs
originally written in C or some other language), including
_spell_ , _man_ on some systems, and various commands for
system administrators. The resulting lack of distinction
between "user command files" and "built-in commands"
is one factor in UNIX's extensibility and, hence, its
favored status among programmers.


You can run a script by typing its name only if the
directory where the script is located is in your command
search path, or. (the current directory) is part of your
command search path, i.e., the script's directory path (as
discussed inChapter 3). If these aren't in your path, you
must type _./scriptname_ , which is really the same thing as
typing the script's absolute pathname (seeChapter 1).

Before you can invoke the shell script by name, you must
also give it "execute" permission. If you are familiar with
the UNIX filesystem, you know that files have three types
of permissions (read, write, and execute) and that those
permissions apply to three categories of user (the file's
owner, a _group_ of users, and everyone else). Normally,
when you create a file with a text editor, the file is set up
with read and write permission for you and read-only
permission for everyone else.

Therefore you must give your script execute permission
explicitly, by using the _chmod_ command. The simplest
way to do this is to type:

```
$ chmod +x
scriptname
```
Your text editor will preserve this permission if you make
subsequent changes to your script. If you don't add
execute permission to the script and you try to invoke it,
the shell will print the message:

```
scriptname : Permission denied
```

But there is a more important difference between the two
ways of running shell scripts. While using **source** causes
the commands in the script to be run as if they were part
of your login session, the "just the name" method causes
the shell to do a series of things. First, it runs another
copy of the shell as a subprocess; this is called a _subshell_.
The subshell then takes commands from the script, runs
them, and terminates, handing control back to the parent
shell.

Figure 4-1shows how the shell executes scripts. Assume
you have a simple shell script called _alice_ that contains
the commands _hatter_ and _gryphon_. In .a, typing **source
alice** causes the two commands to run in the same shell,
just as if you had typed them in by hand. .b shows what
happens when you type just **alice** : the commands run in
the subshell while the parent shell waits for the subshell
to finish.

You may find it interesting to compare this with the
situation in .c, which shows what happens when you type
**alice &**. As you will recall fromChapter 1, the **&** makes
the command run in the _background_ , which is really just
another term for "subprocess." It turns out that the only
significant difference between .c and .b is that you have
control of your terminal or workstation while the
command runs—you need not wait until it finishes before
you can enter further commands.


```
Figure 4-1. Ways to run a shell script
```
There are many ramifications to using subshells. An
important one is that the _export_ ed environment variables
that we saw in the last chapter (e.g., **TERM** , **EDITOR** ,
**PWD** ) are known in subshells, whereas other shell
variables (such as any that you define in your
_.bash_profile_ without an _export_ statement) are not.

Other issues involving subshells are too complex to go
into now; seeChapter 7andChapter 8for more details
about subshell I/O and process characteristics,


respectively. For now, just bear in mind that a script
normally runs in a subshell.

## Functions

_bash_ 's _function_ feature is an expanded version of a similar
facility in the System V Bourne shell and a few other
shells. A function is sort of a script-within-a-script; you
use it to define some shell code by name and store it in
the shell's memory, to be invoked and run later.

Functions improve the shell's programmability
significantly, for two main reasons. First, when you
invoke a function, it is already in the shell's memory;
therefore a function runs faster. Modern computers have
plenty of memory, so there is no need to worry about the
amount of space a typical function takes up. For this
reason, most people define as many commonly used
functions as possible rather than keep lots of scripts
around.

The other advantage of functions is that they are ideal for
organizing long shell scripts into modular "chunks" of
code that are easier to develop and maintain. If you aren't
a programmer, ask one what life would be like without
functions ( also called _procedures_ or _subroutines_ in other
languages) and you'll probably get an earful.

To define a function, you can use either one of two forms:


```
function functname
{
shell commands
}
```
or:

```
functname
( )
{
shell commands
}
```
There is no functional difference between the two. We
will use both forms in this book. You can also delete a
function definition with the command **unset -f** _functname_.

When you define a function, you tell the shell to store its
name and definition (i.e., the shell commands it contains)
in memory. If you want to run the function later, just type
in its name followed by any arguments, as if it were a
shell script.

You can find out what functions are defined in your login
session by typing **declare -f**. The shell will print not just
the names but the definitions of all functions, in
alphabetical order by function name. Since this may
result in long output, you might want to pipe the output
through _more_ or redirect it to a file for examination with a
text editor. If you just want to see the names of the
functions, you can use **declare -F** .[1] We will look at
**declare** in more detail inChapter 6.


Apart from the advantages, there are two important
differences between functions and scripts. First, functions
do not run in separate processes, as scripts do when you
invoke them by name; the "semantics" of running a
function are more like those of your _.bash_profile_ when
you log in or any script when invoked with the **source**
command. Second, if a function has the same name as a
script or executable program, the function takes
precedence.

This is a good time to show the order of precedence for
the various sources of commands when you type a
command to the shell:

1. Aliases
2. Keywords such as **function** and several others,
    like **if** and **for** , which we will see inChapter 5
3. Functions
4. Built-ins like **cd** and **type**
5. Scripts and executable programs, for which the
    shell searches in the directories listed in the
    **PATH** environment variable

Thus, an alias takes precedence over a function or a script
with the same name. You can, however, change the order
of precedence by using the built-ins **command** , **builtin** ,
and **enable**. This allows you to define functions, aliases,
and script files with the same names, and select which


one you want to execute. We'll examine this process in
more detail in the section on command-line processing in
Chapter 7.

If you need to know the exact source of a command, there
are options to the **type** built-in command that we saw in
Chapter 3. **type** by itself will print how _bash_ would
interpret the command, based on the search locations
listed above. If you supply more than one argument to
**type** , it will print the information for each command in
turn. If you had a shell script, a function, and an alias all
called _dodo_ , **type** would tell you that _dodo_ , as an alias,
would be used if you typed _dodo_.

**type** has several options that allow you to find specific
details of a command. If you want to find out all of the
definitions for _dodo_ you can use **type -a**. This will
produce output similar to the following:

```
$ type -all dodo
dodo is aliased to `echo "Everybody has won, and all must have prizes"'
dodo is a function
dodo ( )
{
echo "Everybody has won, and all must have prizes"
}
dodo is ./dodo
```
It is also possible to restrict the search to commands that
are executable files or shell scripts by using the **-p** option.
If the command as typed to _bash_ executes a file or shell
script, the path name of the file is returned; otherwise,
nothing is printed.


The - **P** option forces type to look for executable files or
shell scripts even if the result of - **t** would not return _file_.

A further option, **-f** , suppresses shell function lookup, i.e.,
only keywords, files and aliases will be returned.[2]

The default output from **type** is verbose; it will give you
the full definition for an alias or function. By using the **-t**
option, you can restrict this to a single word descriptor:
_alias_ , _keyword_ , _function_ , _builtin_ , or _file_. For example:

```
$ type -t bash
file
$ type -t if
keyword
```
The **-t** option can also be used with all other options.

We will refer mainly to scripts throughout the remainder
of this book, but unless we note otherwise, you should
assume that whatever we say applies equally to functions.

[1]The **-F** option is not available in versions of _bash_ prior

to 2.0.

[2]The options - **f** and - **P** are not available in versions of

_bash_ prior to 2.05b.


# Shell Variables

_bash_ derives much of its programming functionality from
shell variables. We've already seen the basics of
variables. To recap briefly: they are named places to store
data, usually in the form of character strings, and their
values can be obtained by preceding their names with
dollar signs ($). Certain variables, called _environment
variables_ , are conventionally named in all capital letters,
and their values are made known (with the **export**
statement) to subprocesses.

If you are a programmer, you already know that just
about every major programming language uses variables
in some way; in fact, an important way of characterizing
differences between languages is comparing their
facilities for variables.

The chief difference between _bash_ 's variable schema and
those of conventional languages is that _bash_ 's places
heavy emphasis on character strings. (Thus it has more in
common with a special-purpose language like SNOBOL
than a general-purpose one like Pascal.) This is also true
of the Bourne shell and the C shell, but _bash_ goes beyond
them by having additional mechanisms for handling
integers explicitly.


## Positional Parameters

As we have already seen, you can define values for
variables with statements of the form **varname=value** ,
e.g.:

```
$ hatter=mad
$ echo "$hatter"
mad
```
The shell predefines some environment variables when
you log in. There are other built-in variables that are vital
to shell programming. We will look at a few of them now
and save the others for later.

The most important special, built-in variables are called
_positional parameters_. These hold the command-line
arguments to scripts when they are invoked. Positional
parameters have the names **1** , **2** , **3** , etc., meaning that their
values are denoted by **$1** , **$2** , **$3** , etc. There is also a
positional parameter **0** , whose value is the name of the
script (i.e., the command typed in to invoke it).

Two special variables contain all of the positional
parameters (except positional parameter **0** ): ***** and **@**. The
difference between them is subtle but important, and it's
apparent only when they are within double quotes.

" **$*** " is a single string that consists of all of the positional
parameters, separated by the first character in the value of
the environment variable **IFS** (internal field separator),


which is a space, TAB, and NEWLINE by default. On the
other hand, " **$@** " is equal to " **$1** " " **$2** "**.** .. " **$** _N_ ", where _N_
is the number of positional parameters. That is, it's equal
to _N_ separate double-quoted strings, which are separated
by spaces. If there are no positional parameters, " **$@** "
expands to nothing. We'll explore the ramifications of this
difference in a little while.

The variable **#** holds the number of positional parameters
(as a character string). All of these variables are
"read-only," meaning that you can't assign new values to
them within scripts.

For example, assume that you have the following simple
shell script:

```
echo "alice: $@"
echo "$0: $1 $2 $3 $4"
echo "$# arguments"
```
Assume further that the script is called _alice_. Then if you
type **alice in wonderland** , you will see the following
output:

```
alice: in wonderland
alice: in wonderland
2 arguments
```
In this case, **$3** and **$4** are unset, which means that the
shell will substitute the empty (or null) string for them.[3]


### Positional parameters in

### functions

Shell functions use positional parameters and special
variables like ***** and **#** in exactly the same way as shell
scripts do. If you wanted to define _alice_ as a function, you
could put the following in your _.bash_profile_ or
environment file:

```
function alice
{
echo "alice: $*"
echo "$0: $1 $2 $3 $4"
echo "$# arguments"
}
```
You will get the same result if you type **alice in
wonderland**.

Typically, several shell functions are defined within a
single shell script. Therefore each function will need to
handle its own arguments, which in turn means that each
function needs to keep track of positional parameters
separately. Sure enough, each function has its own copies
of these variables (even though functions don't run in
their own subshells, as scripts do); we say that such
variables are _local_ to the function.

However, other variables defined within functions are not
local (they are _global_ ), meaning that their values are
known throughout the entire shell script. For example,


assume that you have a shell script called _ascript_ that
contains this:

```
function afunc
{
echo in function: $0 $1 $2
var1="in function"
echo var1: $var1
}
```
```
var1="outside function"
echo var1: $var1
echo $0: $1 $2
afunc funcarg1 funcarg2
echo var1: $var1
echo $0: $1 $2
```
If you invoke this script by typing **ascript arg1 arg2** , you
will see this output:

```
var1: outside function
ascript: arg1 arg2
in function: ascript funcarg1 funcarg2
var1: in function
var1: in function
ascript: arg1 arg2
```
In other words, the function _afunc_ changes the value of
the variable **var1** from "outside function" to "in
function," and that change is known outside the function,
while **$1** and **$2** have different values in the function and
the main script. Notice that **$0** _doesn't_ change because the
function executes in the environment of the shell script
and **$0** takes the name of the script.Figure 4-2shows the
scope of each variable graphically.


**Figure 4-2. Functions have their own
positional parameters**


## Local Variables in Functions

A **local** statement inside a function definition makes the
variables involved all become _local_ to that function. The
ability to define variables that are local to "subprogram"
units (procedures, functions, subroutines, etc.) is
necessary for writing large programs, because it helps
keep subprograms independent of the main program and
of each other.

Here is the function from our last example with the
variable _var1_ made local:

```
function afunc
{
local var1
echo in function: $0 $1 $2
```
```
var1="in function"
echo var1: $var1
}
```
Now the result of running **ascript arg1 arg2** is:

```
var1: outside function
ascript: arg1 arg2
in function: ascript funcarg1 funcarg2
var1: in function
var1: outside function
ascript: arg1 arg2
```

Figure 4-3shows the scope of each variable in our new
script. Note that **afunc** now has its own, local copy of
_var1_ , although the original _var1_ would still be used by
any other functions that **ascript** invokes.

```
Figure 4-3. Functions can have local
variables
```

## Quoting with $@ and $*

Now that we have this background, let's take a closer look
at " **$@** " and " **$*** ". These variables are two of the shell's
greatest idiosyncracies, so we'll discuss some of the most
common sources of confusion.

- Why are the elements of " **$*** " separated by the
    first character of **IFS** instead of just spaces? To
    give you output flexibility. As a simple example,
    let's say you want to print a list of positional
    parameters separated by commas. This script
    would do it:

```
IFS=,
echo "$*"
```
- Changing **IFS** in a script is risky, but it's
    probably OK as long as nothing else in the script
    depends on it. If this script were called _arglist_ ,
    then the command **arglist alice dormouse hatter**
    would produce the output
    **alice,dormouse,hatter**. Chapter 5 and Chapter
    10 contain other examples of changing **IFS**.
- Why does " **$@** " act like N separate
    double-quoted strings? To allow you to use them
    again as separate values. For example, say you


```
want to call a function within your script with the
same list of positional parameters, like this:
```
```
function countargs
{
echo "$# args."
}
```
- Assume your script is called with the same
    arguments as _arglist_ above. Then if it contains
    the command **countargs** " **$*** ", the function will
    print **1 args**. But if the command is **countargs**
    " **$@** ", the function will print **3 args**.


## More on Variable Syntax

Before we show the many things you can do with shell
variables, we have to point out a simplification we have
been making: the syntax of _$varname_ for taking the value
of a variable is actually the simple form of the more
general syntax, _${varname}_.

Why two syntaxes? For one thing, the more general
syntax is necessary if your code refers to more than nine
positional parameters: you must use **${10}** for the tenth
instead of **$10**. Aside from that, consider the following
case where you would like to place an underscore after
your user ID:

```
echo $UID_
```
The shell will try to use **UID_** as the name of the
variable. Unless, by chance, **$UID_** already exists, this
won't print anything (the value being _null_ or the empty
string, ""). To obtain the desired result, you need to
enclose the shell variable in curly brackets:

```
echo ${UID}_
```
It is safe to omit the curly brackets ( **{}** ) if the variable
name is followed by a character that isn't a letter, digit, or
underscore.


[3]Unless the option **nounset** is turned on, in which case

the shell will return an error message.


# String Operators

The curly-bracket syntax allows for the shell's _string
operators_. String operators allow you to manipulate
values of variables in various useful ways without having
to write full-blown programs or resort to external UNIX
utilities. You can do a lot with string-handling operators
even if you haven't yet mastered the programming
features we'll see in later chapters.

In particular, string operators let you do the following:

- Ensure that variables exist (i.e., are defined and
    have non-null values)
- Set default values for variables
- Catch errors that result from variables not being
    set
- Remove portions of variables' values that match
    patterns

## Syntax of String Operators

The basic idea behind the syntax of string operators is
that special characters that denote operations are inserted
between the variable's name and the right curly bracket.


Any argument that the operator may need is inserted to
the operator's right.

The first group of string-handling operators tests for the
existence of variables and allows substitutions of default
values under certain conditions. These are listed inTable
4-1.[4]

**Table 4-1. Substitution operators**

```
Operator Substitution
```
```
${ varname :- word }
```
```
If varname exists and isn't null,
return its value; otherwise return
word.
```
```
Purpose : Returning a default
value if the variable is
undefined.
```
```
Example : ${count:-0}
evaluates to 0 if count is
undefined.
```
```
${ varname := word }
```
```
If varname exists and isn't null,
return its value; otherwise set it
to word and then return its
```

**Operator Substitution**

```
value. Positional and special
parameters cannot be assigned
this way.
```
```
Purpose : Setting a variable to a
default value if it is undefined.
```
```
Example : ${count:=0} sets
count to 0 if it is undefined.
```
**${** _varname_ **:?**
_message_ **}**

```
If varname exists and isn't null,
return its value; otherwise print
varname : followed by message ,
and abort the current command
or script (non-interactive shells
only). Omitting message
produces the default message
parameter null or not set.
```
```
Purpose : Catching errors that
result from variables being
undefined.
```
```
Example :
{count:? " undefined! " } prints
```

**Operator Substitution**

```
"count: undefined!" and exits if
count is undefined.
```
**${** _varname_ :+ _word_ **}**

```
If varname exists and isn't null,
return word ; otherwise return
null.
```
```
Purpose : Testing for the
existence of a variable.
```
```
Example : ${count:+1} returns
1 (which could mean "true") if
count is defined.
```
#### ${

_varname_ : _offset_ : _length_
**}**

```
Performs substring expansion.[5]
It returns the substring of
$ varname starting at offset and
up to length characters. The first
character in $ varname is
position 0. If length is omitted,
the substring starts at offset and
continues to the end of
$ varname. If offset is less than 0
then the position is taken from
```

```
Operator Substitution
```
```
the end of $ varname. If
varname is @, the length is the
number of positional parameters
starting at parameter offset.
```
```
Purpose : Returning parts of a
string (substrings or slices ).
```
```
Example : If count is set to
frogfootman , ${count:4} returns
footman. ${count:4:4} returns
foot.
```
```
[5]The substring expansion operator is not available in
versions of bash prior to 2.0.
```
The first of these operators is ideal for setting defaults for
command-line arguments in case the user omits them.
We'll use this technique in our first programming task.

```
Task 4-1
```

```
You have a large album collection, and you want to
write some software to keep track of it. Assume that you
have a file of data on how many albums you have by
each artist. Lines in the file look like this:
```
```
5 Depeche Mode
2 Split Enz
3 Simple Minds
1 Vivaldi, Antonio
```
```
Write a program that prints the N highest lines, i.e., the
N artists by whom you have the most albums. The
default for N should be 10. The program should take one
argument for the name of the input file and an optional
second argument for how many lines to print.
```
By far the best approach to this type of script is to use
built-in UNIX utilities, combining them with I/O
redirectors and pipes. This is the classic "building-block"
philosophy of UNIX that is another reason for its great
popularity with programmers. The building-block
technique lets us write a first version of the script that is
only one line long:

```
sort -nr $1 | head -${2:-10}
```
Here is how this works: the _sort_ program sorts the data in
the file whose name is given as the first argument ( **$1** ).
The **-n** option tells _sort_ to interpret the first word on each
line as a number (instead of as a character string); the **-r**


tells it to reverse the comparisons, so as to sort in
descending order.

The output of _sort_ is piped into the _head_ utility, which,
when given the argument **-** _N_ , prints the first _N_ lines of its
input on the standard output. The expression **-${2:-10}**
evaluates to a dash ( **-** ) followed by the second argument if
it is given, or to -10 if it's not; notice that the variable in
this expression is **2** , which is the second positional
parameter.

Assume the script we want to write is called _highest_.
Then if the user types **highest myfile** , the line that
actually runs is:

```
sort -nr myfile | head -10
```
Or if the user types **highest myfile 22** , the line that runs
is:

```
sort -nr myfile | head -22
```
Make sure you understand how the **:-** string operator
provides a default value.

This is a perfectly good, runnable script—but it has a few
problems. First, its one line is a bit cryptic. While this
isn't much of a problem for such a tiny script, it's not wise
to write long, elaborate scripts in this manner. A few
minor changes will make the code more readable.

First, we can add comments to the code; anything
between # and the end of a line is a comment. At a


minimum, the script should start with a few comment
lines that indicate what the script does and what
arguments it accepts. Second, we can improve the
variable names by assigning the values of the positional
parameters to regular variables with mnemonic names.
Finally, we can add blank lines to space things out; blank
lines, like comments, are ignored. Here is a more readable
version:

```
#
# highest filename [howmany]
#
# Print howmany highest-numbered lines in file filename.
# The input file is assumed to have lines that start with
# numbers. Default for howmany is 10.
#
```
```
filename=$1
howmany=${2:-10}
```
```
sort -nr $filename | head -$howmany
```
The square brackets around **howmany** in the comments
adhere to the convention in UNIX documentation that
square brackets denote optional arguments.

The changes we just made improve the code's readability
but not how it runs. What if the user were to invoke the
script without any arguments? Remember that positional
parameters default to null if they aren't defined. If there
are no arguments, then **$1** and **$2** are both null. The
variable **howmany** ( **$2** ) is set up to default to 10, but
there is no default for **filename** ( **$1** ). The result would be
that this command runs:


```
sort -nr | head -10
```
As it happens, if _sort_ is called without a filename
argument, it expects input to come from standard input,
e.g., a pipe (|) or a user's terminal. Since it doesn't have
the pipe, it will expect the terminal. This means that the
script will appear to hang! Although you could always hit
CTRL-D or CTRL-C to get out of the script, a naive user
might not know this.

Therefore we need to make sure that the user supplies at
least one argument. There are a few ways of doing this;
one of them involves another string operator. We'll
replace the line:

```
filename=$1
```
with:

```
filename=${1:?"filename missing."}
```
This will cause two things to happen if a user invokes the
script without any arguments: first the shell will print the
somewhat unfortunate message:

```
highest: 1: filename missing.
```
to the standard error output. Second, the script will exit
without running the remaining code. With a somewhat
"kludgy" modification, we can get a slightly better error
message.

Consider this code:


```
filename=$1
filename=${filename:?"missing."}
```
This results in the message:

```
highest: filename: missing.
```
(Make sure you understand why.) Of course, there are
ways of printing whatever message is desired; we'll find
out how inChapter 5.

Before we move on, we'll look more closely at the three
remaining operators inTable 4-1 and see how we can
incorporate them into our task solution. The **:=** operator
does roughly the same thing as **:-** , except that it has the
"side effect" of setting the value of the variable to the
given word if the variable doesn't exist.

Therefore we would like to use **:=** in our script in place of
**:-** , but we can't; we'd be trying to set the value of a
positional parameter, which is not allowed. But if we
replaced:

```
howmany=${2:-10}
```
with just:

```
howmany=$2
```
and moved the substitution down to the actual command
line (as we did at the start), then we could use the **:=**
operator:

```
sort -nr $filename | head -${howmany:=10}
```

The operator **:+** substitutes a value if the given variable
exists and isn't null. Here is how we can use it in our
example: let's say we want to give the user the option of
adding a header line to the script's output. If she types the
option **-h** , then the output will be preceded by the line:

```
ALBUMS ARTIST
```
Assume further that this option ends up in the variable
**header** , i.e., **$header** is **-h** if the option is set or null if
not. (Later we will see how to do this without disturbing
the other positional parameters.)

The following expression yields null if the variable
**header** is null, or **ALBUMSARTIST\n** if it is non-null:

```
${header:+"ALBUMSARTIST\n"}
```
This means that we can put the line:

```
echo -e -n ${header:+"ALBUMSARTIST\n"}
```
right before the command line that does the actual work.
The **-n** option to **echo** causes it _not_ to print a LINEFEED
after printing its arguments. Therefore this **echo** statement
will print nothing—not even a blank line—if **header** is
null; otherwise it will print the header line and a
LINEFEED (\n). The **-e** option makes **echo** interpret the
\n as a LINEFEED rather than literally.

The final operator, substring expansion, returns sections
of a string. We can use it to "pick out" parts of a string
that are of interest. Assume that our script is able to


assign lines of the sorted list, one at a time, to the variable
_album_line_. If we want to print out just the album name
and ignore the number of albums, we can use substring
expansion:

```
echo ${album_line:8}
```
This prints everything from character position 8, which is
the start of each album name, onwards.

If we just want to print the numbers and not the album
names, we can do so by supplying the length of the
substring:

```
echo ${album_line:0:7}
```
Although this example may seem rather useless, it should
give you a feel for how to use substrings. When
combined with some of the programming features
discussed later in the book, substrings can be extremely
useful.


## Patterns and Pattern

## Matching

We'll continue refining our solution to Task 4-1 later in
this chapter. The next type of string operator is used to
match portions of a variable's string value against
_patterns_. Patterns, as we saw inChapter 1, are strings that
can contain wildcard characters ( ***** ,**?** , and **[]** for character
sets and ranges).

Table 4-2lists _bash_ 's pattern-matching operators.

**Table 4-2. Pattern-matching operators**

```
Operator Meaning
```
```
$ { variable
#pattern }
```
```
If the pattern matches the
beginning of the variable's value,
delete the shortest part that
matches and return the rest.
```
```
$ { variable
##pattern }
```
```
If the pattern matches the
beginning of the variable's value,
```

**Operator Meaning**

```
delete the longest part that matches
and return the rest.
```
**$** { _variable
%pattern_ }

```
If the pattern matches the end of
the variable's value, delete the
shortest part that matches and
return the rest.
```
**$** { _variable
%%pattern_ }

```
If the pattern matches the end of
the variable's value, delete the
longest part that matches and
return the rest.
```
**$** { _variable/
pattern/
string_ } **$** { _variable//
pattern/ string_ }

```
The longest match to pattern in
variable is replaced by string. In
the first form, only the first match
is replaced. In the second form, all
matches are replaced. If the pattern
begins with a # , it must match at
the start of the variable. If it begins
with a % , it must match with the
end of the variable. If string is
null, the matches are deleted. If
```

```
Operator Meaning
```
```
variable is @ or * , the operation is
applied to each positional
parameter in turn and the
expansion is the resultant list.[6]
```
```
[6]The pattern-matching and replacement operator is
not available in versions of bash prior to 2.0.
```
These can be hard to remember; here's a handy mnemonic
device: **#** matches the front because number signs _precede_
numbers; **%** matches the rear because percent signs
_follow_ numbers.

The classic use for pattern-matching operators is in
stripping off components of pathnames, such as directory
prefixes and filename suffixes. With that in mind, here is
an example that shows how all of the operators work.
Assume that the variable **path** has the value _/home/cam/
book/long.file.name_ ; then:

```
Expression Result
${path##/*/} long.file.name
${path#/*/} cam/book/long.file.name
$path /home/cam/book/long.file.name
${path%.*} /home/cam/book/long.file
${path%%.*} /home/cam/book/long
```

The two patterns used here are **/*/** , which matches
anything between two slashes, and. ***** , which matches a
dot followed by anything.

The longest and shortest pattern-matching operators
produce the same output unless they are used with the *****
wildcard operator. As an example, if **filename** had the
value **alicece** , then both **${filename%ce}** and
**${filename%%ce}** would produce the result **alice**. This
is because **ce** is an _exact_ match; for a match to occur, the
string _ce_ must appear on the end **$filename**. Both the
short and long matches will then match the last grouping
of _ce_ and delete it. If, however, we had used the *****
wildcard, then **${filename%ce*}** would produce **alice**
because it matches the shortest occurrence of _ce_ followed
by anything else. **${filename%%ce*}** would return **ali**
because it matches the longest occurrence of _ce_ followed
by anything else; in this case the first and second _ce_.

The next task will incorporate one of these
pattern-matching operators.

```
Task 4-2
```
```
You are writing a graphics file conversion utility for use
in creating a web page. You want to be able to take a
PCX file and convert it to a JPEG file for use on the
web page.[7]
```

Graphics file conversion utilities are quite common
because of the plethora of different graphics formats and
file types. They allow you to specify an input file, usually
from a range of different formats, and convert it to an
output file of a different format. In this case, we want to
take a PCX file, which can't be displayed with a web
browser, and convert it to a JPEG which can be displayed
by nearly all browsers. Part of this process is taking the
filename of the PCX file, which ends in _.pcx_ , and
changing it to one ending in _.jpg_ for the output file. In
essence, you want to take the original filename and strip
off the _.pcx_ , then append _.jpg_. A single shell statement
will do this:

```
outfile=${filename%.pcx}.jpg
```
The shell takes the filename and looks for **.pcx** on the end
of the string. If it is found, **.pcx** is stripped off and the rest
of the string is returned. For example, if **filename** had the
value **alice.pcx** , the expression **${filename%.pcx}** would
return **alice**. The **.jpg** is appended to form the desired
**alice.jpg** , which is then stored in the variable **outfile**.

If **filename** had an inappropriate value (without the **.pcx** )
such as **alice.xpm** , the above expression would evaluate
to **alice.xpm.jpg** : since there was no match, nothing is
deleted from the value of **filename** , and **.jpg** is appended
anyway. Note, however, that if **filename** contained more
than one dot (e.g., if it were **alice.1.pcx—** the expression
would still produce the desired value **alice.1.jpg** ).

The next task uses the longest pattern-matching operator.


```
Task 4-3
```
```
You are implementing a filter that prepares a text file
for printer output. You want to put the file's
name—without any directory prefix—on the "banner"
page. Assume that, in your script, you have the
pathname of the file to be printed stored in the variable
pathname.
```
Clearly, the objective is to remove the directory prefix
from the pathname. The following line will do it:

```
bannername=${pathname##*/}
```
This solution is similar to the first line in the examples
shown before. If **pathname** were just a filename, the
pattern ***/** (anything followed by a slash) would not match
and the value of the expression would be **pathname**
untouched. If **pathname** were something like _book/
wonderland_ , the prefix _book/_ would match the pattern and
be deleted, leaving just **wonderland** as the expression's
value. The same thing would happen if **pathname** were
something like _/home/cam/ book/wonderland_ : since the
**##** deletes the longest match, it deletes the entire _/home/
cam/book/_.

If we used **#*/** instead of **##*/** , the expression would have
the incorrect value _home/cam/book/wonderland_ , because


the shortest instance of "anything followed by a slash" at
the beginning of the string is just a slash ( **/** ).

The construct **${** _variable_ **##*/}** is actually equivalent to
the UNIX utility _basename_. _basename_ takes a pathname
as argument and returns the filename only; it is meant to
be used with the shell's command substitution mechanism
(see the following explanation). _basename_ is less efficient
than **${** _variable_ **##*/}** because it runs in its own separate
process rather than within the shell. Another utility,
_dirname_ , does essentially the opposite of _basename_ : it
returns the directory prefix only. It is equivalent to the
_bash_ expression **${** _variable_ **%/*}** and is less efficient for
the same reason.

The last operator in the table matches patterns and
performs substitutions. Task 4-4 is a simple task where it
comes in useful.

```
Task 4-4
```
```
The directories in PATH can be hard to distinguish
when printed out as one line with colon delimiters. You
want a simple way to display them, one to a line.
```
As directory names are separated by colons, the easiest
way would be to replace each colon with a LINEFEED:


```
$ echo -e ${PATH//:/'\n'}
/home/cam/bin
/usr/local/bin
/bin
/usr/bin
/usr/X11R6/bin
```
Each occurrence of the colon is replaced by \n. As we
saw earlier, the **-e** option allows **echo** to interpret \n as a
LINEFEED. In this case we used the second of the two
substitution forms. If we'd used the first form, only the
first colon would have been replaced with a \n.


## Length Operator

There is one remaining operator on variables. It is **${#**
_varname_ **}** , which returns the length of the value of the
variable as a character string. (InChapter 6, we will see
how to treat this and similar values as actual numbers so
they can be used in arithmetic expressions.) For example,
if **filename** has the value **alice.c** , then **${#filename}**
would have the value **7**.


## Extended Pattern Matching

Bash provides a further set of pattern matching operators
if the **shopt** option **extglob** is switched on. Each operator
takes one or more patterns, normally strings, separated by
the vertical bar ( **|** ). The extended pattern matching
operators are given inTable 4-3.[8]

**Table 4-3. Pattern-matching operators**

```
Operator Meaning
```
```
*( patternlist )
Matches zero or more occurrences of
the given patterns.
```
```
+( patternlist )
```
```
Matches one or more occurrences of the
given patterns.
```
```
?( patternlist )
Matches zero or one occurrences of the
given patterns.
```

```
Operator Meaning
```
```
@( patternlist )
Matches exactly one of the given
patterns.
```
```
!( patternlist )
Matches anything except one of the
given patterns.
```
Some examples of these include:

- ***(alice|hatter|hare)** would match zero or more
    occurrences of **alice** , **hatter** , and **hare**. So it
    would match the null string, **alice** , **alicehatter** ,
    etc.
- **+(alice|hatter|hare)** would do the same except
    not match the null string.
- **?(alice|hatter|hare)** would only match the null
    string, **alice** , **hatter** , or **hare**.
- **@(alice|hatter|hare)** would only match **alice** ,
    **hatter** , or **hare**.
- **!(alice|hatter|hare)** matches everything except
    **alice** , **hatter** , and **hare**.


The values provided can contain shell wildcards too. So,
for example, **+([0-9])** matches a number of one or more
digits. The patterns can also be nested, so you could
remove all files except those beginning with **vt** followed
by a number by doing **rm !(vt+([0-9]))**.

[4]The colon (:) in all but the last of these operators is

actually optional. If the colon is omitted, then change
"exists and isn't null" to "exists" in each definition, i.e.,
the operator tests for existence only.

[7]PCX is a popular graphics file format under Microsoft

Windows. JPEG (Joint Photographic Expert Group) is a
common graphics format on the Internet and is used to a
great extent on web pages.

[8]Be aware that these are not available in early releases

of _bash_ 2.0.


# Command Substitution

From the discussion so far, we've seen two ways of
getting values into variables: by assignment statements
and by the user supplying them as command-line
arguments (positional parameters). There is another way:
_command substitution_ , which allows you to use the
standard output of a command as if it were the value of a
variable. You will soon see how powerful this feature is.

The syntax of command substitution is:[9]

```
$( UNIX command )
```
The command inside the parentheses is run, and anything
the command writes to standard output is returned as the
value of the expression. These constructs can be nested,
i.e., the UNIX command can contain command
substitutions.

Here are some simple examples:

- The value of **$(pwd** ) is the current directory
    (same as the environment variable **$PWD** ).
- The value of **$(ls $HOME)** is the names of all
    files in your home directory.
- The value of **$(ls $(pwd))** is the names of all files
    in the current directory.


- The value of **$(< alice)** is the contents of the file
    _alice_ with any trailing newlines removed.[10]
- To find out detailed information about a
    command if you don't know where its file
    resides, type **ls -l $(type -path -all**
    command-name). The **-all** option forces **type** to
    do a pathname look-up and **-path** causes it to
    ignore keywords, built-ins, etc.
- If you want to edit (with vi) every chapter of your
    book on bash that has the phrase "command
    substitution," assuming that your chapter files all
    begin with ch, you could type:

```
vi $(grep -l 'command substitution' ch*)
```
- The **-l** option to _grep_ prints only the names of
    files that contain matches.

Command substitution, like variable and tilde expansion,
is done within double quotes. Therefore, our rule in
Chapter 1 andChapter 3 about using single quotes for
strings unless they contain variables will now be
extended: "When in doubt, use single quotes, unless the
string contains variables or command substitutions, in
which case use double quotes."

Command substitution helps us with the solution to the
next programming task, which relates to the album
database in Task 4-1.


```
Task 4-5
```
```
The file used in Task 4-1 is actually a report derived
from a bigger table of data about albums. This table
consists of several columns, or fields , to which a user
refers by names like "artist," "title," "year," etc. The
columns are separated by vertical bars ( | , the same as the
UNIX pipe character). To deal with individual columns
in the table, field names need to be converted to field
numbers.
```
```
Suppose there is a shell function called getfield that
takes the field name as argument and writes the
corresponding field (or column) number on the standard
output. Use this routine to help extract a column from
the data table.
```
The _cut_ utility is a natural for this task. _cut_ is a data filter:
it extracts columns from tabular data. If you supply the
numbers of columns you want to extract from the input,
_cut_ will print only those columns on the standard output.
Columns can be character positions or—relevant in this
example—fields that are separated by TAB characters or
other delimiters.[11]Assume that the data table in our task
is a file called _albums_ and that it looks like this:

```
Depeche Mode|Speak and Spell|Mute Records|1981
Depeche Mode|Some Great Reward|Mute Records|1984
Depeche Mode|101|Mute Records|1989
Depeche Mode|Violator|Mute Records|1990
```

```
Depeche Mode|Songs of Faith and Devotion|Mute Records|1993
...
```
Here is how we would use _cut_ to extract the fourth (year)
column:

```
cut -f4 -d\| albums
```
The **-d** argument is used to specify the character used as
field delimiter (TAB is the default). The vertical bar must
be backslash-escaped so that the shell doesn't try to
interpret it as a pipe.

From this line of code and the _getfield_ routine, we can
easily derive the solution to the task. Assume that the first
argument to _getfield_ is the name of the field the user
wants to extract. Then the solution is:

```
fieldname=$1
cut -f$(getfield $fieldname) -d\| albums
```
If we called this script with the argument **year** , the output
would be:

```
1981
1984
1989
1990
1993
...
```
Task 4-6 shows another small task that makes use of _cut_.


```
Task 4-6
```
```
Send a mail message to everyone who is currently
logged in.
```
The command _who_ tells you who is logged in (as well as
which terminal they're on and when they logged in). Its
output looks like this:

```
root tty1 Oct 13 12:05
michael tty5 Oct 13 12:58
cam tty23 Oct 13 11:51
kilrath tty25 Oct 13 11:58
```
The fields are separated by spaces, not TABs. Since we
need the first field, we can get away with using a space as
the field separator in the _cut_ command. (Otherwise we'd
have to use the option to _cut_ that uses character columns
instead of fields.) To provide a space character as an
argument on a command line, you can surround it by
quotes:

```
$ who | cut -d' ' -f1
```
With the above _who_ output, this command's output would
look like this:

```
root
michael
cam
kilrath
```
This leads directly to a solution to the task. Just type:


```
$ mail $(who | cut -d' ' -f1)
```
The command **mail root michael cam kilrath** will run
and then you can type your message.

Task 4-7 is another task that shows how useful command
pipelines can be in command substitution.

```
Task 4-7
```
```
The ls command gives you pattern-matching capability
with wildcards, but it doesn't allow you to select files by
modification date. Devise a mechanism that lets you do
this.
```
Here is a function that allows you to list all files that were
last modified on the date you give as argument. Once
again, we choose a function for speed reasons. No pun is
intended by the function's name:

```
function lsd
{
date=$1
ls -l | grep -i "^.\{42\}$date" | cut -c55-
}
```
This function depends on the column layout of the **ls -l**
command. In particular, it depends on dates starting in
column 42 and filenames starting in column 55. If this
isn't the case in your version of UNIX, you will need to
adjust the column numbers.[12]


We use the _grep_ search utility to match the date given as
argument (in the form _Mon DD_ , e.g., **Jan 15** or **Oct 6** , the
latter having two spaces) to the output of **ls -l**. This gives
us a long listing of only those files whose dates match the
argument. The **-i** option to _grep_ allows you to use all
lowercase letters in the month name, while the rather
fancy argument means, "Match any line that contains 41
characters followed by the function argument." For
example, typing **lsd** `jan 15' causes _grep_ to search for
lines that match any 41 characters followed by **jan 15** (or
**Jan 15** ).[13]

The output of _grep_ is piped through our ubiquitous friend
_cut_ to retrieve the filenames only. The argument to _cut_
tells it to extract characters in column 55 through the end
of the line.

With command substitution, you can use this function
with _any_ command that accepts filename arguments. For
example, if you want to print all files in your current
directory that were last modified today, and today is
January 15th, you could type:

```
$ lp $(lsd 'jan 15')
```
The output of _lsd_ is on multiple lines (one for each
filename), but LINEFEEDs are legal field separators for
the _lp_ command, because the environment variable **IFS**
(see earlier in this chapter) contains LINEFEED by
default.


[9] Bourne and C shell users should note that the

command substitution syntax of those shells, _`UNIX
command_ **`** (with backward quotes, or grave accents), is
also supported by _bash_ for backward compatibility
reasons. However, it is harder to read and less conducive
to nesting.

[10]Not available in versions of _bash_ prior to 2.02.

[11]Some older BSD-derived systems don't have _cut_ , but

you can use _awk_ instead. Whenever you see a command
of the form: cut -f _N_ -d _C filename_ , use this

instead:awk -F _C_ '{print $ _N_ }' _filename_.

[12]For example, **ls -l** on SunOS 4.1.x has dates starting

in column 33 and filenames starting in column 46.

[13]Some older BSD-derived versions of UNIX (without

System V extensions) do not support the **\{** _N_ **\}** option.
For this example, use 42 periods in a row instead of
**.\{42\}**.


# Advanced Examples:

# pushd and popd

We will conclude this chapter with a couple of functions
that are already built into _bash_ but are useful in
demonstrating some of the concepts we have covered in
this chapter.[14]

```
Task 4-8
```
```
The functions pushd and popd implement a stack of
directories that enable you to move to another directory
temporarily and have the shell remember where you
were. Implement them as shell functions.
```
We will start by implementing a significant subset of
their capabilities and finish the implementation in
Chapter 6.

Think of a stack as a spring-loaded dish receptacle in a
cafeteria. When you place dishes on the receptacle, the
spring compresses so that the top stays at roughly the
same level. The dish most recently placed on the stack is
the first to be taken when someone wants food; thus, the
stack is known as a "last-in, first-out" or _LIFO_ structure.
Putting something onto a stack is known in computer


science parlance as _pushing_ , and taking something off the
top is called _popping_.

A stack is very handy for remembering directories, as we
will see; it can "hold your place" up to an arbitrary
number of times. The **cd -** form of the **cd** command does
this, but only to one level. For example: if you are in
_firstdir_ and then you change to _seconddir_ , you can type
**cd -** to go back. But if you start out in _firstdir_ , then
change to _seconddir_ , and then go to _thirddir_ , you can use
**cd -** only to go back to _seconddir_. If you type **cd -** again,
you will be back in _thirddir_ , because it is the previous
directory.[15]

If you want the "nested" remember-and-change
functionality that will take you back to _firstdir_ , you need
a stack of directories along with the _pushd_ and _popd_
commands. Here is how these work:

- The first time **pushd** dir is called, **pushd** pushes
    the current directory onto the stack, then **cd** s to
    dir and pushes it onto the stack.
- Subsequent calls to **pushd** dir **cd** to dir and push
    dir only onto the stack.
- **popd** removes the top directory off the stack,
    revealing a new top. Then it **cd** s to the new top
    directory.


For example, consider the series of events inTable 4-4.
Assume that you have just logged in, and that you are in
your home directory ( _/home/you_ ).

**Table 4-4. pushd/popd example**

```
Command Stack contents
Result
directory
```
```
pushd
lizard
```
```
/home/you/lizard /home/
you
```
```
/home/you/
lizard
```
```
pushd /etc
```
```
/etc /home/you/lizard
/home/you
/etc
```
```
popd
/home/you/lizard /home/
you
```
```
/home/you/
lizard
```
```
popd /home/you /home/you
```
```
popd <empty> (error)
```
We will implement a stack as an environment variable
containing a list of directories separated by spaces.[16]


Your directory stack should be initialized to the null
string when you log in. To do this, put this in your
_.bash_profile_ :

```
DIR_STACK=""
export DIR_STACK
```
Do _not_ put this in your environment file if you have one.
The **export** statement guarantees that DIR_STACK is
known to all subprocesses; you want to initialize it only
once. If you put this code in an environment file, it will
get reinitialized in every subshell, which you probably
don't want.

Next, we need to implement _pushd_ and _popd_ as functions.
Here are our initial versions:

```
pushd ( )
{
dirname=$1
DIR_STACK="$dirname ${DIR_STACK:-$PWD' '}"
cd ${dirname:?"missing directory name."}
echo "$DIR_STACK"
}
```
```
popd ( )
{
DIR_STACK=${DIR_STACK#* }
cd ${DIR_STACK%% *}
echo "$PWD"
}
```
Notice that there isn't much code! Let's go through the
two functions and see how they work, starting with


_pushd_. The first line merely saves the first argument in
the variable **dirname** for readability reasons.

The second line of the function pushes the new directory
onto the stack. The expression **${DIR_STACK:-** $PWD`
' **}** evaluates to **$DIR_STACK** if it is non-null or **$PWD** ''
(the current directory and a space) if it is null. The
expression within double quotes, then, consists of the
argument given, followed by a single space, followed by
DIR_STACK or the current directory and a space. The
trailing space on the current directory is required for
pattern matching in the _popd_ function; each directory in
the stack is considered to be of the form " _dirname_ ".

The double quotes in the assignment ensure that all of this
is packaged into a single string for assignment back to
DIR_STACK. Thus, this line of code handles the special
initial case (when the stack is empty) as well as the more
usual case (when it's not empty).

The third line's main purpose is to change to the new
directory. We use the **:?** operator to handle the error when
the argument is missing: if the argument is given, then the
expression **${dirname:?** " **missing directory name** ." **}**
evaluates to **$dirname** , but if it is not given, the shell will
print the message **pushd: dirname: missing directory
name** and exit from the function.

The last line merely prints the contents of the stack, with
the implication that the leftmost directory is both the
current directory and at the top of the stack. (This is why


we chose spaces to separate directories, rather than the
more customary colons as in PATH and MAILPATH.)

The _popd_ function makes yet another use of the shell's
pattern-matching operators. Its first line uses the **#**
operator, which tries to delete the shortest match of the
pattern " ***** " (anything followed by a space) from the
value of DIR_STACK. The result is that the top directory
and the space following it are deleted from the stack. This
is why we need the space on the end of the first directory
pushed onto the stack.

The second line of _popd_ uses the pattern-matching
operator **%%** to delete the _longest_ match to the pattern
" ***** " (a space followed by anything) from DIR_STACK.
This extracts the top directory as an argument to **cd** , but it
doesn't affect the value of DIR_STACK because there is
no assignment. The final line just prints a confirmation
message.

This code is deficient in four ways. First, it has no
provision for errors. For example:

- What if the user tries to push a directory that
    doesn't exist or is invalid?
- What if the user tries popd and the stack is
    empty?

Test your understanding of the code by figuring out how
it would respond to these error conditions. The second
problem is that if you use _pushd_ in a shell script, it will


exit everything if no argument is given; **${** _varname_ **:?**
_message_ **}** always exits from non-interactive shells. It
won't, however, exit an interactive shell from which the
function is called. The third deficiency is that it
implements only some of the functionality of _bash_ 's
_pushd_ and _popd_ commands—albeit the most useful parts.
In the next chapter, we will see how to overcome all of
these deficiencies.

The fourth problem with the code is that it will not work
if, for some reason, a directory name contains a space.
The code will treat the space as a separator character.
We'll accept this deficiency for now, but you might like
to think about how to overcome it in the next few
chapters.

[14]Your copy of _bash_ may not have _pushd_ and _popd_ ,

since it can be configured without these built-ins.

[15]Think of **cd -** as a synonym for **cd $OLDPWD** ; see

the previous chapter.

[16] _bash_ also maintains a directory stack for the **pushd**

and **popd** built-ins, accessible through the environment
variable **DIRSTACK**. Unlike our version, however, it is
implemented as an _array_ (see Chapter 6for details on
arrays).


# Chapter 5. Flow Control

If you are a programmer, you may have read the last
chapter—with its claim at the outset that _bash_ has an
advanced set of programming capabilities—and
wondered where many of the features from conventional
languages were. Perhaps the most glaringly obvious
"hole" in our coverage thus far concerns _flow control_
constructs like **if** , **for** , **while** , and so on.

Flow control gives a programmer the power to specify
that only certain portions of a program run, or that certain
portions run repeatedly, according to conditions such as
the values of variables, whether or not commands execute
properly, and others. We call this the ability to control the
flow of a program's execution.

Almost every shell script or function that's been shown
thus far has had no flow control—they have just been lists
of commands to be run! Yet _bash_ , like the C and Bourne
shells, has all of the flow control abilities you would
expect and more; we will examine them in this chapter.
We'll use them to enhance the solutions to some of the
programming tasks we saw in the last chapter and to
solve tasks that we will introduce here.

Although we have attempted to explain flow control so
that nonprogrammers can understand it, we also
sympathize with programmers who dread having to slog


through yet another _tabula rasa_ explanation. For this
reason, some of our discussions relate _bash_ 's flow-control
mechanisms to those that programmers should know
already. Therefore you will be in a better position to
understand this chapter if you already have a basic
knowledge of flow control concepts.

_bash_ supports the following flow control constructs:

**_if/else_**

```
Execute a list of statements if a certain condition is/is
not true
```
**_for_**

```
Execute a list of statements a fixed number of times
```
**_while_**

```
Execute a list of statements repeatedly while a
certain condition holds true
```
**_until_**

```
Execute a list of statements repeatedly until a certain
condition holds true
```
**_case_**

```
Execute one of several lists of statements depending
on the value of a variable
```

In addition, _bash_ provides a new type of flow-control
construct:

**_select_**

```
Allow the user to select one of a list of possibilities
from a menu
```
We will now cover each of these in detail.

# if/else

The simplest type of flow control construct is the
_conditional_ , embodied in _bash_ 's **if** statement. You use a
conditional when you want to choose whether or not to do
something, or to choose among a small number of things
to do, according to the truth or falsehood of _conditions_.
Conditions test values of shell variables, characteristics of
files, whether or not commands run successfully, and
other factors. The shell has a large set of built-in tests that
are relevant to the task of shell programming.

The **if** construct has the following syntax:

```
if condition
then
statements
[elif condition
then statements ...]
[else
statements ]
fi
```

The simplest form (without the **elif** and **else** parts, or
_clauses_ ) executes the _statements_ only if the _condition_ is
true. If you add an **else** clause, you get the ability to
execute one set of statements if a condition is true or
another set of statements if the condition is false. You can
use as many **elif** (a contraction of "else if") clauses as you
wish; they introduce more conditions, and thus more
choices for which set of statements to execute. If you use
one or more **elif** s, you can think of the **else** clause as the
"if all _else_ fails" part.

## Exit Status

Perhaps the only aspect of this syntax that differs from
that of conventional languages like C and Pascal is that
the "condition" is really a list of statements rather than the
more usual Boolean (true or false) expression. How is the
truth or falsehood of the condition determined? It has to
do with a general UNIX concept that we haven't covered
yet: the _exit status_ of commands.

Every UNIX command, whether it comes from source
code in C, some other language, or a shell script/function,
returns an integer code to its calling process—the shell in
this case—when it finishes. This is called the exit status.
0 is _usually_ the OK exit status, while anything else (1 to
255) _usually_ denotes an error.[1]

**if** checks the exit status of the _last_ statement in the list
following the **if** keyword. The list is usually just a single


statement. If the status is 0, the condition evaluates to
true; if it is anything else, the condition is considered
false. The same is true for each condition attached to an
**elif** statement (if any).

This enables us to write code of the form:

```
if command ran successfully
then
normal processing
else
error processing
fi
```
More specifically, we can now improve on the _pushd_
function that we saw in the last chapter:

```
pushd ( )
{
dirname=$1
DIR_STACK="$dirname ${DIR_STACK:-$PWD' '}"
cd ${dirname:?"missing directory name."}
echo $DIR_STACK
}
```
This function requires a valid directory as its argument.
Let's look at how it handles error conditions: if no
argument is given, the third line of code prints an error
message and exits. This is fine.

However, the function reacts deceptively when an
argument is given that isn't a valid directory. In case you
didn't figure it out when reading the last chapter, here is
what happens: the **cd** fails, leaving you in the same


directory you were in. This is also appropriate. But the
second line of code has pushed the bad directory onto the
stack anyway, and the last line prints a message that leads
you to believe that the push was successful. Even placing
the **cd** before the stack assignment won't help because it
doesn't exit the function if there is an error.

We need to prevent the bad directory from being pushed
and to print an error message. Here is how we can do this:

```
pushd ( )
{
dirname=$1
if cd ${dirname:?"missing directory name."} # if cd was successful
then
DIR_STACK="$dirname ${DIR_STACK:-$PWD' '}" # push the directory
echo $DIR_STACK
else
echo still in $PWD. # else do nothing
fi
}
```
The call to **cd** is now inside an **if** construct. If **cd** is
successful, it will return 0; the next two lines of code are
run, finishing the _pushd_ operation. But if the **cd** fails, it
returns with exit status 1, and _pushd_ will print a message
saying that you haven't gone anywhere.

Notice that in providing the check for a bad directory, we
have slightly altered the way _pushd_ functions. The stack
will now always start out with two copies of the first
directory pushed onto it. That is because **$PWD** is
expanded after the new directory has been changed to.
We'll fix this in the next section.


You can usually rely on built-in commands and standard
UNIX utilities to return appropriate exit statuses, but
what about your own shell scripts and functions? For
example, what if you wrote a **cd** function that overrides
the built-in command?

Let's say you have the following code in your
_.bash_profile_.

```
cd ( )
{
builtin cd "$@"
echo "$OLDPWD --> $PWD"
}
```
The function _cd_ simply changes directories and prints a
message saying where you were and where you are now.
Because functions have higher priority than most built-in
commands in the shell's order of command look-up, we
need to make sure that the built-in **cd** is called, otherwise
the shell will enter an endless loop of calling the function,
known as _infinite recursion_.

The **builtin** command allows us to do this. **builtin** tells
the shell to use the built-in command and ignore any
function of that name. Using **builtin** is easy; you just give
it the name of the built-in you want to execute and any
parameters you want to pass. If you pass in the name of
something which isn't a built-in command, **builtin** will
display an appropriate message. For example: **builtin:
alice: not a shell builtin**.


We want this function to return the same exit status that
the built-in **cd** returns. The problem is that the exit status
is reset by every command, so it "disappears" if you don't
save it immediately. In this function, the built-in **cd** 's exit
status disappears when the **echo** statement runs (and sets
its own exit status).

Therefore, we need to save the status that **cd** sets and use
it as the entire function's exit status. Two shell features
we haven't seen yet provide the way. First is the special
shell variable**?** , whose value ( **$?** ) is the exit status of the
last command that ran. For example:

```
cd baddir
echo $?
```
causes the shell to print **1** , while the following command
causes it to print **0** :

```
cd gooddir
echo $?
```
So, to save the exit status we need to assign the value of**?**
to a variable with the line **es=$?** right after the **cd** is done.


## Return

The second feature we need is the statement **return** _N_ ,
which causes the surrounding function to exit with exit
status _N_. _N_ is actually optional; it defaults to the exit
status of the last command. Functions that finish without
a **return** statement (i.e., every one we have seen so far)
return whatever the last statement returns. **return** can
only be used inside functions, and shell scripts that have
been executed with **source**. In contrast, the statement **exit**
_N_ exits the entire script, no matter how deeply you are
nested in functions.

Getting back to our example: if the call to the built-in **cd**
were last in our _cd_ function, it would behave properly.
Unfortunately, we really need the assignment statement
where it is. Therefore we need to save **cd** 's exit status and
return it as the function's exit status. Here is how to do it:

```
cd ( )
{
builtin cd "$@"
es=$?
echo "$OLDPWD --> $PWD"
return $es
}
```
The second line saves the exit status of **cd** in the variable
**es** ; the fourth returns it as the function's exit status. We'll
see a substantial **cd** "wrapper" inChapter 7.


Exit statuses aren't very useful for anything other than
their intended purpose. In particular, you may be tempted
to use them as "return values" of functions, as you would
with functions in C or Pascal. That won't work; you
should use variables or command substitution instead to
simulate this effect.


## Combinations of Exit

## Statuses

One of the more obscure parts of _bash_ syntax allows you
to combine exit statuses logically, so that you can test
more than one thing at a time.

The syntax _statement1_ **&&** _statement2_ means, "execute
_statement1_ , and if its exit status is 0, execute _statement2_ ."
The syntax _statement1_ **||** _statement2_ is the converse: it
means, "execute _statement1_ , and if its exit status is _not_ 0,
execute _statement2_ ." At first, these look like "if/then" and
"if not/then" constructs, respectively. But they are really
intended for use within conditions of **if** constructs—as C
programmers will readily understand.

It's much more useful to think of these constructs as "and"
and "or," respectively. Consider this:

```
if statement1 && statement2
then
...
fi
```
In this case, _statement1_ is executed. If it returns a 0 status,
then presumably it ran without error. Then _statement2_
runs. The **then** clause is executed if _statement2_ returns a
0 status. Conversely, if _statement1_ fails (returns a


non-zero exit status), then _statement2_ doesn't even run;
the last statement that actually ran was _statement1_ , which
failed—so the **then** clause doesn't run, either. Taken all
together, it's fair to conclude that the **then** clause runs if
_statement1_ and _statement2_ both succeeded.

Similarly, consider this:

```
if statement1 || statement2
then
...
fi
```
If _statement1_ succeeds, then _statement2_ does _not_ run.
This makes _statement1_ the last statement, which means
that the **then** clause runs. On the other hand, if _statement1_
fails, then _statement2_ runs, and whether the **then** clause
runs or not depends on the success of _statement2_. The
upshot is that the **then** clause runs if _statement1 or
statement2_ succeeds.

_bash_ also allows you to reverse the return status of a
statement with the use of**!** , the logical "not". Preceding a
statement with**!** will cause it to return 0 if it fails and 1 if
it succeeds. We'll see an example of this at the end of this
chapter.

As a simple example of testing exit statuses, assume that
we need to write a script that checks a file for the
presence of two words and just prints a message saying
whether _either_ word is in the file or not. We can use _grep_


for this: it returns exit status 0 if it found the given string
in its input, non-zero if not:

```
filename=$1
word1=$2
word2=$3
```
```
if grep $word1 $filename || grep $word2 $filename
then
echo "$word1 or $word2 is in $filename."
fi
```
The **then** clause of this code runs if either _grep_ statement
succeeds. Now assume that we want the script to say
whether the input file contains _both_ words. Here's how to
do it:

```
filename=$1
word1=$2
word2=$3
```
```
if grep $word1 $filename && grep $word2 $filename
then
echo "$word1 and $word2 are both in $filename."
fi
```
We'll see more examples of these logical operators later
in this chapter.


## Condition Tests

Exit statuses are the only things an **if** construct can test.
But that doesn't mean you can check only whether
commands ran properly. The shell provides two ways of
testing a variety of conditions. The first is with the [...]
construct, which is available in many different versions of
the Bourne shell.[2]The second is by using the newer
[[...]] construct.[3]The second version is identical to the
first except that word splitting and pathname expansion
are not performed on the words within the brackets. For
the examples in this chapter we will use the first form of
the construct.

You can use the construct to check many different
attributes of a file (whether it exists, what type of file it is,
what its permissions and ownership are, etc.), compare
two files to see which is newer, and do comparisons on
strings.

[ _condition_ ] is actually a statement just like any other,
except that the only thing it does is return an exit status
that tells whether _condition_ is true. (The spaces after the
opening bracket "[" and before the closing bracket "]" are
required.) Thus it fits within the **if** construct's syntax.


### String comparisons

The square brackets ([]) surround expressions that include
various types of _operators_. We will start with the string
comparison operators, listed in Table 5-1. (Notice that
there are no operators for "greater than or equal" or "less
than or equal" comparisons.) In the table, _str1_ and _str2_
refer to expressions with a string value.

**Table 5-1. String comparison operators**

```
Operator True if...
```
```
str1 = str2[4] str1 matches str2
```
```
str1 != str2 str1 does not match str2
```
```
str1 < str2 str1 is less than str2
```
```
str1 > str2 str1 is greater than str2
```
```
-n str1 str1 is not null (has length greater than 0)
```

```
Operator True if...
```
```
-z str1 str1 is null (has length 0)
```
```
[4]Note that there is only one equal sign ( = ). This is a
common source of error.
```
We can use one of these operators to improve our _popd_
function, which reacts badly if you try to pop and the
stack is empty. Recall that the code for _popd_ is:

```
popd ( )
{
DIR_STACK=${DIR_STACK#* }
cd ${DIR_STACK%% *}
echo "$PWD"
}
```
If the stack is empty, then **$DIR_STACK** is the null
string, as is the expression **${DIR_STACK%% }**. This
means that you will change to your home directory;
instead, we want _popd_ to print an error message and do
nothing.

To accomplish this, we need to test for an empty stack,
i.e., whether **$DIR_STACK** is null or not. Here is one
way to do it:


```
popd ( )
{
if [ -n "$DIR_STACK" ]; then
DIR_STACK=${DIR_STACK#* }
cd ${DIR_STACK%% *}
echo "$PWD"
else
echo "stack empty, still in $PWD."
fi
}
```
In the condition, we have placed the **$DIR_STACK** in
double quotes, so that when it is expanded it is treated as
a single word. If you don't do this, the shell will expand
**$DIR_STACK** to individual words and the test will
complain that it was given too many arguments.

There is another reason for placing **$DIR_STACK** in
double quotes, which will become important later on:
sometimes the variable being tested will expand to
nothing, and in this example the test will become **[ -n ]** ,
which returns **true**. Surrounding the variable in double
quotes ensures that even if it expands to nothing, there
will be an empty string as an argument (i.e., **[ -n "" ]** ).

Also notice that instead of putting **then** on a separate line,
we put it on the same line as the **if** after a semicolon,
which is the shell's standard statement separator
character.

We could have used operators other than **-n**. For example,
we could have used **-z** and switched the code in the **then**
and **else** clauses.


While we're cleaning up code we wrote in the last
chapter, let's fix up the error handling in the _highest_ script
(Task 5-1). The code for that script was:

```
filename=${1:?"filename missing."}
howmany=${2:-10}
sort -nr $filename | head -$howmany
```
Recall that if you omit the first argument (the filename),
the shell prints the message **highest: 1: filename
missing**. We can make this better by substituting a more
standard "usage" message. While we are at it, we can also
make the command more in line with conventional UNIX
commands by requiring a dash before the optional
argument.

```
if [ -z "$1" ]; then
echo 'usage: highest filename [-N]'
else
filename=$1
howmany=${2:--10}
sort -nr $filename | head $howmany
fi
```
Notice that we have moved the dash in front of
**$howmany** inside the parameter expansion **${2:—10}**.

It is considered better programming style to enclose all of
the code in the **if** - **then** - **else** , but such code can get
confusing if you are writing a long script in which you
need to check for errors and bail out at several points
along the way. Therefore, a more usual style for shell
programming follows.


```
if [ -z "$1" ]; then
echo 'usage: highest filename [-N]'
exit 1
fi
```
```
filename=$1
howmany=${2:--10}
sort -nr $filename | head $howmany
```
The **exit** statement informs any calling program whether
it ran successfully or not.

As an example of the **=** operator, we can add to the
graphics utility that we touched on in Task 4-2. Recall
that we were given a filename ending in _.pcx_ (the original
graphics file), and we needed to construct a filename that
was the same but ended in _.jpg_ (the output file). It would
be nice to be able to convert several other types of
formats to JPEG files so that we could use them on a web
page. Some common types we might want to convert
besides PCX include XPM (X PixMap), TGA (Targa),
TIFF (Tagged Image File Format), and GIF.

We won't attempt to perform the actual manipulations
needed to convert one graphics format to another
ourselves. Instead we'll use some tools that are freely
available on the Internet, graphics conversion utilities
from the NetPBM archive.[5]

Don't worry about the details of how these utilities work;
all we want to do is create a shell frontend that processes
the filenames and calls the correct conversion utilities. At
this point it is sufficient to know that each conversion


utility takes a filename as an argument and sends the
results of the conversion to standard output. To reduce the
number of conversion programs necessary to convert
between the 30 or so different graphics formats it
supports, NetPBM has its own set of internal formats.
These are called Portable Anymap files (also called
PNMs) with extensions _.ppm_ (Portable Pix Map) for color
images, _.pgm_ (Portable Gray Map) for grayscale images,
and _.pbm_ (Portable Bit Map) for black and white images.
Each graphics format has a utility to convert to and from
this "central" PNM format.

The frontend script we are developing should first choose
the correct conversion utility based on the filename
extension, and then convert the resulting PNM file into a
JPEG:

```
filename=$1
extension=${filename##*.}
pnmfile=${filename%.*}.pnm
outfile=${filename%.*}.jpg
if [ -z $filename ]; then
echo "procfile: No file specified"
exit 1
fi
if [ $extension = jpg ]; then
exit 0
elif [ $extension = tga ]; then
tgatoppm $filename > $pnmfile
elif [ $extension = xpm ]; then
xpmtoppm $filename > $pnmfile
elif [ $extension = pcx ]; then
pcxtoppm $filename > $pnmfile
elif [ $extension = tif ]; then
```

```
tifftopnm $filename > $pnmfile
elif [ $extension = gif ]; then
giftopnm $filename > $pnmfile
else
echo "procfile: $filename is an unknown graphics file."
exit 1
fi
pnmtojpeg $pnmfile > $outfile
rm $pnmfile
```
Recall from the previous chapter that the expression
**${filename%.*}** deletes the extension from **filename** ;
**${filename##*.}** deletes the basename and keeps the
extension.

Once the correct conversion is chosen, the script runs the
utility and writes the output to a temporary file. The
second to last line takes the temporary file and converts it
to a JPEG. The temporary file is then removed. Notice
that if the original file was a JPEG we just exit without
having to do any processing.

This script has a few problems. We'll look at improving it
later in this chapter.


### File attribute checking

The other kind of operator that can be used in conditional
expressions checks a file for certain properties. There are
24 such operators. We will cover those of most general
interest here; the rest refer to arcana like sticky bits,
sockets, and file descriptors, and thus are of interest only
to systems hackers. Refer toAppendix Bfor the complete
list.Table 5-2lists those that we will examine.

**Table 5-2. File attribute operators**

```
Operator True if...
```
```
-a file file exists
```
```
-d file file exists and is a directory
```
```
-e file file exists; same as - a
```
```
-f file
file exists and is a regular file (i.e., not a
directory or other special type of file)
```

**Operator True if...**

**-r** _file_ You have read permission on _file_

**-s** _file file_ exists and is not empty

**-w** _file_ You have write permission on _file_

**-x** _file_

```
You have execute permission on file , or
directory search permission if it is a
directory
```
**-N** _file file_ was modified since it was last read

**-O** _file_ You own _file_

**-G** _file
file_ 's group ID matches yours (or one of
yours, if you are in multiple groups)

_file1_ **-nt**
_file2
file1_ is newer than _file2[6]_


```
Operator True if...
```
```
file1 -ot
file2
file1 is older than file2
```
```
[6] Specifically, the -nt and -ot operators compare
modification times of two files.
```
Before we get to an example, you should know that
conditional expressions inside **[** and **]** can also be
combined using the logical operators **&&** and **||** , just as
we saw with plain shell commands, in the previous
section entitledSection 5.1.3_._ " For example:

```
if [ condition ] && [ condition ]; then
```
It's also possible to combine shell commands with
conditional expressions using logical operators, like this:

```
if command && [ condition ]; then
...
```
You can also negate the truth value of a conditional
expression by preceding it with an exclamation point (**!** ),
so that**!** _expr_ evaluates to true only if _expr_ is false.
Furthermore, you can make complex logical expressions
of conditional operators by grouping them with
parentheses (which must be "escaped" with backslashes


to prevent the shell from treating them specially), and by
using two logical operators we haven't seen yet: **-a**
(AND) and **-o** (OR).

The **-a** and **-o** operators are similar to the **&&** and **||**
operators used with exit statuses. However, unlike those
operators, **-a** and **-o** are only available inside a **test**
conditional expression.

Here is how we would use two of the file operators, a
logical operator, and a string operator to fix the problem
of duplicate stack entries in our _pushd_ function. Instead of
having **cd** determine whether the argument given is a
valid directory—i.e., by returning with a bad exit status if
it's not—we can do the checking ourselves. Here is the
code:

```
pushd ( )
{
dirname=$1
if [ -n "$dirname" ] && [ \( -d "$dirname" \) -a \
\( -x "$dirname" \) ]; then
DIR_STACK="$dirname ${DIR_STACK:-$PWD' '}"
cd $dirname
echo "$DIR_STACK"
else
echo "still in $PWD."
fi
}
```
The conditional expression evaluates to true only if the
argument **$1** is not null ( **-n** ), a directory ( **-d** ) _and_ the user
has permission to change to it ( **-x** ).[7] Notice that this
conditional handles the case where the argument is


missing ( **$dirname** is null) first; if it is, the rest of the
condition is _not_ executed. This is important because, if
we had just put:

```
if [ \( -n "$dirname"\) -a \( -d "$dirname" \) -a \
\( -x "$dirname" \) ]; then
```
the second condition, if null, would cause **test** to
complain and the function would exit prematurely.

Here is a more comprehensive example of the use of file
operators.

```
Task 5-1
```
```
Write a script that prints essentially the same
information as ls -l but in a more user-friendly way.
```
Although the code for this task looks at first sight quite
complicated, it is a straightforward application of many
of the file operators:

```
if [! -e "$1" ]; then
echo "file $1 does not exist."
exit 1
fi
if [ -d "$1" ]; then
echo -n "$1 is a directory that you may "
if [! -x "$1" ]; then
echo -n "not "
fi
```

```
echo "search."
elif [ -f "$1" ]; then
echo "$1 is a regular file."
else
echo "$1 is a special type of file."
fi
if [ -O "$1" ]; then
echo 'you own the file.'
else
echo 'you do not own the file.'
fi
if [ -r "$1" ]; then
echo 'you have read permission on the file.'
fi
if [ -w "$1" ]; then
echo 'you have write permission on the file.'
fi
if [ -x "$1" -a! -d "$1" ]; then
echo 'you have execute permission on the file.'
fi
```
We'll call this script _fileinfo_. Here's how it works:

- The first conditional tests if the file given as
    argument does not exist (the exclamation point is
    the "not" operator; the spaces around it are
    required). If the file does not exist, the script
    prints an error message and exits with error
    status.
- The second conditional tests if the file is a
    directory. If so, the first echo prints part of a
    message; remember that the **-n** option tells echo
    not to print a LINEFEED at the end. The inner
    conditional checks if you do not have search


```
permission on the directory. If you don't have
search permission, the word "not" is added to the
partial message. Then, the message is completed
with "search." and a LINEFEED.
```
- The **elif** clause checks if the file is a regular file;
    if so, it prints a message.
- The **else** clause accounts for the various special
    file types on recent UNIX systems, such as
    sockets, devices, FIFO files, etc. We assume that
    the casual user isn't interested in details of these.
- The next conditional tests to see if the file is
    owned by you (i.e., if its owner ID is the same as
    your login ID). If so, it prints a message saying
    that you own it.
- The next two conditionals test for your read and
    write permission on the file.
- The last conditional checks if you can execute the
    file. It checks to see if you have execute
    permission and that the file is not a directory. (If
    the file were a directory, execute permission
    would really mean directory search permission.)
    In this test we haven't used any brackets to group
    the tests and have relied on operator precedence.
    Simply put, operator precedence is the order in
    which the shell processes the operators. This is
    exactly the same concept as arithmetic


```
precedence in mathematics, where multiply and
divide are done before addition and subtraction.
In our case, [ -x "$1" -a! -d "$1" ] is equivalent
to [\( -x "$1" \) -a \(! -d "$1" \) ]. The file tests
are done first, followed by any negations (! ) and
followed by the AND and OR tests.
```
As an example of _fileinfo_ 's output, assume that you do an
**ls -l** of your current directory and it contains these lines:

```
-rwxr-xr-x 1 cam users 2987 Jan 10 20:43 adventure
-rw-r--r-- 1 cam users 30 Jan 10 21:45 alice
-r--r--r-- 1 root root 58379 Jan 11 21:30 core
drwxr-xr-x 2 cam users 1024 Jan 10 21:41 dodo
```
_alice_ and _core_ are regular files, _dodo_ is a directory, and
_adventure_ is a shell script. Typing **fileinfo adventure**
produces this output:

```
adventure is a regular file.
you own the file.
you have read permission on the file.
you have write permission on the file.
you have execute permission on the file.
```
Typing **fileinfo alice** results in this:

```
alice is a regular file.
you own the file.
you have read permission on the file.
you have write permission on the file.
```
Finally, typing **fileinfo dodo** results in this:


```
dodo is a directory that you may search.
you own the file.
you have read permission on the file.
you have write permission on the file.
```
Typing **fileinfo core** produces this:

```
core is a regular file.
you do not own the file.
you have read permission on the file.
```

## Integer Conditionals

The shell also provides a set of _arithmetic_ tests. These are
different from _character string_ comparisons like **<** and **>** ,
which compare _lexicographic_ values of strings,[8] not
numeric values. For example, "6" is greater than "57"
lexicographically, just as "p" is greater than "ox," but of
course the opposite is true when they're compared as
integers.

The integer comparison operators are summarized in
Table 5-3.

**Table 5-3. Arithmetic test operators**

```
Test Comparison
```
```
-lt Less than
```
```
-le Less than or equal
```
```
-eq Equal
```

```
Test Comparison
```
```
-ge Greater than or equal
```
```
-gt Greater than
```
```
-ne Not equal
```
You'll find these to be of the most use in the context of
the integer variables we'll see in the next chapter. They're
necessary if you want to combine integer tests with other
types of tests within the same conditional expression.

However, the shell has a separate syntax for conditional
expressions that involve integers only. It's considerably
more efficient, so you should use it in preference to the
arithmetic test operators listed above. Again, we'll cover
the shell's integer conditionals in the next chapter.

[1]Because this is a convention and not a "law," there are

exceptions. For example, _diff_ (find differences between
two files) returns 0 for "no differences," 1 for "differences
found," or 2 for an error such as an invalid filename
argument.


[2]The built-in command **test** is synonymous with [...].

For example, to test the equivalence of two strings you
can either put [ _string1_ = _string2_ ] or **test** _string1_ =
_string2_.

[3][[...]] is not available in versions of _bash_ prior to 2.05.

[5]NetPBM is a free, portable graphics conversion utility

package. Further details can be found on the NetPBM
homepagehttp://netpbm.sourceforge.net/

[7] Remember that the same permission flag that

determines execute permission on a regular file
determines search permission on a directory. This is why
the **-x** operator checks both things depending on file type.

[8]"Lexicographic order" is really just "dictionary order."


# for

The most obvious enhancement to make the previous
script is the ability to report on multiple files instead of
just one. Tests like **-e** and **-d** take only single arguments,
so we need a way of calling the code once for each file
given on the command line.

The way to do this—indeed, the way to do many things
with _bash—_ is with a looping construct. The simplest and
most widely applicable of the shell's looping constructs is
the **for** loop. We'll use **for** to enhance _fileinfo_ soon.

The **for** loop allows you to repeat a section of code a
fixed number of times. During each time through the code
(known as an _iteration_ ), a special variable called a _loop
variable_ is set to a different value; this way each iteration
can do something slightly different.

The **for** loop is somewhat, but not entirely, similar to its
counterparts in conventional languages like C and Pascal.
The chief difference is that the shell's standard **for** loop
doesn't let you specify a number of times to iterate or a
range of values over which to iterate; instead, it only lets
you give a fixed list of values. In other words, you can't
do anything like this Pascal-type code, which executes
_statements_ 10 times:


```
for x := 1 to 10 do
begin
statements...
end
```
However, the **for** loop is ideal for working with
arguments on the command line and with sets of files
(e.g., all files in a given directory). We'll look at an
example of each of these. But first, we'll show the syntax
for the **for** construct:

```
for name [in list ]
do
statements that can use
$name...
done
```
The _list_ is a list of names. (If **in** _list_ is omitted, the list
defaults to " **$@** ", i.e., the quoted list of command-line
arguments, but we'll always supply the **in** _list_ for the sake
of clarity.) In our solutions to the following task, we'll
show two simple ways to specify lists.

```
Task 5-2
```
```
Task 4-4 used pattern matching and substitution to list
the directories in PATH , one to a line. Unfortunately,
old versions of bash don't have that particular pattern
operator. Write a general shell script, listpath , that prints
each directory in PATH , one per line. In addition, have
```

```
it print out information about each directory, such as the
permissions and the modification times.
```
The easiest way to do this is by changing the **IFS** variable
we saw inChapter 4:

```
IFS=:
```
```
for dir in $PATH
do
ls -ld $dir
done
```
This sets the **IFS** to be a colon, which is the separator
used in **PATH**. The **for** loop loops through, setting **dir** to
each of the colon delimited fields in **PATH**. _ls_ is used to
print out the directory name and associated information.
The **-l** parameter specifies the "long" format and the **-d**
tells _ls_ to show only the directory itself and not its
contents.

In using this you might see an error generated by _ls_
saying, for example, **ls: /usr/TeX/bin: No such file or
directory**. It indicates that a directory in **PATH** doesn't
exist. We can modify the _listpath_ script to check the
**PATH** variable for nonexistent directories by adding
some of the tests we saw earlier:

```
IFS=:
```
```
for dir in $PATH; do
if [ -z "$dir" ]; then dir=.; fi
```

```
if! [ -e "$dir" ]; then
echo "$dir doesn't exist"
elif! [ -d "$dir" ]; then
echo "$dir isn't a directory"
else
ls -ld $dir
fi
done
```
This time, as the script loops, we first check to see if the
length of **$dir** is zero (caused by having a value of :: in
the **PATH** ). If it is, we set it to the current directory, then
check to see if the directory _doesn't_ exist. If it doesn't, we
print out an appropriate message. Otherwise, we check to
see if the file is not a directory. If it isn't, we say so.

The foregoing illustrated a simple use of **for** , but it's
much more common to use **for** to iterate through a list of
command-line arguments. To show this, we can enhance
the _fileinfo_ script above to accept multiple arguments.
First, we write a bit of "wrapper" code that does the
iteration:

```
for filename in "$@" ; do
finfo "$filename"
echo
done
```
Next, we make the original script into a function called
_finfo_ :[9]

```
finfo ( )
{
if [! -e "$1" ]; then
print "file $1 does not exist."
```

```
return 1
fi
...
}
```
The complete script consists of the **for** loop code and the
above function, in either order; good programming style
dictates that the function definition should go first.

The _fileinfo_ script works as follows: in the **for** statement,
" **$@** " is a list of all positional parameters. For each
argument, the body of the loop is run with **filename** set to
that argument. In other words, the function _finfo_ is called
once for each value of **$filename** as its first argument
( **$1** ). The call to _echo_ after the call to _finfo_ merely prints a
blank line between sets of information about each file.

Given a directory with the same files as the earlier
example, typing **fileinfo*** would produce the following
output:

```
adventure is a regular file.
you own the file.
you have read permission on the file.
you have write permission on the file.
you have execute permission on the file.
```
```
alice is a regular file.
you own the file.
you have read permission on the file.
you have write permission on the file.
```
```
core is a regular file.
you do not own the file.
you have read permission on the file.
```

```
dodo is a directory that you may search.
you own the file.
you have read permission on the file.
you have write permission on the file.
```
Here is a programming task that exploits the other major
use of **for**.

```
Task 5-3
```
```
It is possible to print out all of the directories below a
given one by using the -R option of ls. Unfortunately,
this doesn't give much idea about the directory structure
because it prints all the files and directories line by line.
Write a script that performs a recursive directory listing
and produces output that gives an idea of the structure
for a small number of subdirectories.
```
We'll probably want output that looks something like this:

```
.
adventure
aaiw
dodo
duchess
hatter
march_hare
queen
tarts
biog
ttlg
```

```
red_queen
tweedledee
tweedledum
lewis.carroll
```
Each column represents a directory level. Entries below
and to the right of an entry are files and directories under
that directory. Files are just listed with no entries to their
right. This example shows that the directory _adventure_
and the file _lewis.carroll_ are in the current directory; the
directories _aaiw_ and _ttlg_ , and the file _biog_ are under
_adventure_ , etc. To make life simple, we'll use TABs to
line the columns up and ignore any "bleed over" of
filenames from one column into an adjacent one.

We need to be able to traverse the directory hierarchy. To
do this easily we'll use a programming technique known
as _recursion_. Recursion is simply referencing something
from itself; in our case, calling a piece of code from itself.
For example, consider this script, _tracedir_ , in your home
directory:

```
file=$1
echo $file
```
```
if [ -d "$file" ]; then
cd $file
~/tracedir $(ls)
cd ..
fi
```
First we copy and print the first argument. Then we test to
see if it is a directory. If it is, we **cd** to it and call the
script again with an argument of the files in that


directory. This script is recursive; when the first argument
is a directory, a new shell is invoked and a new script is
run on the new directory. The old script waits until the
new script returns, then the old script executes a **cd** back
up one level and exits. This happens in each invocation of
the _tracedir_ script. The recursion will stop only when the
first argument isn't a directory.

Running this on the directory structure listed above with
the argument _adventure_ will produce:

```
adventure
aaiw
dodo
```
_dodo_ is a file and the script exits.

This script has a few problems, but it is the basis for the
solution to this task. One major problem with the script is
that it is very inefficient. Each time the script is called, a
new shell is created. We can improve on this by making
the script into a function, because (as you probably
remember fromChapter 4) functions are part of the shell
they are started from. We also need a way to set up the
TAB spacing. The easiest way is to have an initializing
script or function and call the recursive routine from that.
Let's look at this routine.

```
recls ( )
{
singletab="\t"
```
```
for tryfile in "$@"; do
```

```
echo $tryfile
if [ -d "$tryfile" ]; then
thisfile=$tryfile
recdir $(command ls $tryfile)
fi
done
```
```
unset dir singletab tab
}
```
First, we set up a variable to hold the TAB character for
the **echo** command (Chapter 7explains all of the options
and formatting commands you can use with **echo** ). Then
we loop through each argument supplied to the function
and print it out. If it is a directory, we call our recursive
routine, supplying the list of files with _ls_. We have
introduced a new command at this point: **command**.
**command** is a shell built-in that disables function and
alias look-up. In this case, it is used to make sure that the
_ls_ command is one from your command search path,
PATH, and not a function (for further information on
**command** seeChapter 7). After it's all over, we clean up
by unsetting the variables we have used.

Now we can expand on our earlier shell script.

```
recdir ( )
{
tab=$tab$singletab
```
```
for file in "$@"; do
echo -e $tab$file
thisfile=$thisfile/$file
```
```
if [ -d "$thisfile" ]; then
```

```
recdir $(command ls $thisfile)
fi
```
```
thisfile=${thisfile%/*}
done
```
```
tab=${tab%"$singletab"}
}
```
Each time it is called, _recdir_ loops through the files it is
given as arguments. For each one it prints the filename
and then, if the file is a directory, calls itself with
arguments set to the contents of the directory. There are
two details that have to be taken care of: the number of
TABs to use, and the pathname of the "current" directory
in the recursion.

Each time we go down a level in the directory hierarchy
we want to add a TAB character, so we append a TAB to
the variable **tab** every time we enter _recdir_. Likewise,
when we exit _recdir_ we are moving up a directory level,
so we remove the TAB when we leave the function.
Initially, **tab** is not set, so the first time _recdir_ is called,
**tab** will be set to one TAB. If we recurse into a lower
directory, _recdir_ will be called again and another TAB
will be appended. Remember that **tab** is a global variable,
so it will grow and shrink in TABs for every entry and
exit of _recdir_. The **-e** option to **echo** tells it to recognize
escaped formatting characters, in our case the TAB
character, **\t**.

In this version of the recursive routine we haven't used **cd**
to move between directories. That means that an **ls** of a


directory will have to be supplied with a relative path to
files further down in the hierarchy. To do this, we need to
keep track of the directory we are currently examining.
The initialization routine sets the variable **thisfile** to the
directory name each time a directory is found while
looping. This variable is then used in the recursive routine
to keep the relative pathname of the current file being
examined. On each iteration of the loop, **thisfile** has the
current filename appended to it, and at the end of the loop
the filename is removed.

You might like to think of ways to modify the behavior
and improve the output of this code. Here are some
programming challenges:

1. In the current version, there is no way to
    determine if _biog_ is a file or a directory. An
    empty directory looks no different to a file in the
    listing. Change the output so it appends a **/** to
    each directory name when it displays it.
2. Modify the code so that it only recurses down a
    maximum of eight subdirectories (which is about
    the maximum before the lines overflow the
    right-hand side of the screen). Hint: think about
    how TABs have been implemented.
3. Change the output so it includes dashed lines and
    adds a blank line after each directory, thus:

```
.
|
```

```
|-------adventure
| |
| |-------aaiw
| | |
| | |-------dodo
| | |-------duchess
| | |-------hatter
| | |-------march_hare
| | |-------queen
| | |-------tarts
| |
| |-------biog
...
```
4. Hint: you need at least two other variables that
    contain the characters " **|** " and " **-** ".

At the start of this section we pointed out that the **for** loop
in its standard form wasn't capable of iterating over a
specified range of values as can be done in most
programming languages. _bash_ 2.0 introduced a new style
of **for** loop which caters for this task; the _arithmetic_ **for**
loop. Well come back to it in the next chapter when we
look at arithmetic operations.

[9] A function can have the same name as a script;

however, this isn't good programming practice.


# case

The next flow-control construct we will cover is **case**.
While the **case** statement in Pascal and the similar **switch**
statement in Java and C can be used to test simple values
like integers and characters, _bash_ 's **case** construct lets you
test strings against patterns that can contain wildcard
characters. Like its conventional-language counterparts,
**case** lets you express a series of if-then-else type
statements in a concise way.

The syntax of **case** is as follows:

```
case expression
in
pattern1
)
statements ;;
pattern2
)
statements ;;
...
esac
```
Any of the _pattern_ s can actually be several patterns
separated by pipe characters ( **|** ). If _expression_ matches
one of the patterns, its corresponding statements are
executed. If there are several patterns separated by pipe
characters, the expression can match any of them in order
for the associated statements to be run. The patterns are


checked in order until a match is found; if none is found,
nothing happens.

This construct should become clearer with an example.
Let's revisit our solution to Task 4-2 and the additions to
it presented earlier in this chapter (our graphics utility).
Remember that we wrote some code that processed input
files according to their suffixes ( _.pcx_ for PCX format, _.gif_
for GIF format, etc.).

We can improve upon this solution in two ways. Firstly,
we can use a **for** loop to allow multiple files to be
processed one at a time; secondly, we can use the **case**
construct to streamline the code:

```
for filename in "$@"; do
pnmfile=${filename%.*}.ppm
case $filename in
*.jpg ) exit 0 ;;
*.tga ) tgatoppm $filename > $pnmfile ;;
*.xpm ) xpmtoppm $filename > $pnmfile ;;
*.pcx ) pcxtoppm $filename > $pnmfile ;;
*.tif ) tifftopnm $filename > $pnmfile ;;
*.gif ) giftopnm $filename > $pnmfile ;;
* ) echo "procfile: $filename is an unknown graphics file."
exit 1 ;;
esac
outfile=${pnmfile%.ppm}.new.jpg
pnmtojpeg $pnmfile > $outfile
rm $pnmfile
done
```

The **case** construct in this code does the same thing as the
**if** statements that we saw in the earlier version. It is,
however, clearer and easier to follow.

The first six patterns in the **case** statement match the
various file extensions that we wish to process. The last
pattern matches anything that hasn't already been
matched by the previous statements. It is essentially a
catchall and is analogous to the **default** case in C.

There is another slight difference to the previous version;
we have moved the pattern matching and replacement
inside the added **for** loop that processes all of the
command-line arguments. Each time we pass through the
loop, we want to create a temporary and final file with a
name based on the name in the current command-line
argument.

We'll return to this example in Chapter 6, when we
further develop the script and discuss how to handle dash
options on the command line. In the meantime, here is a
task that requires that we use **case**.

```
Task 5-4
```
```
Write a function that implements the Korn shell's cd old
new. cd takes the pathname of the current directory and
tries to find the string old. If it finds it, it substitutes new
and attempts to change to the resulting directory.
```

We can implement this by using a **case** statement to
check the number of arguments and the built-in **cd**
command to do the actual change of directory.

Here is the code:[10]

```
cd( )
{
case "$#" in
0 | 1) builtin cd $1 ;;
2 ) newdir=${PWD//$1/$2}
case "$newdir" in
$PWD) echo "bash: cd: bad substitution" >&2 ;
return 1 ;;
* ) builtin cd "$newdir" ;;
esac ;;
* ) echo "bash: cd: wrong arg count" 1>&2 ; return 1 ;;
esac
}
```
The **case** statement in this task tests the number of
arguments to our **cd** command against three alternatives.

For zero or one arguments, we want our **cd** to work just
like the built-in one. The first alternative in the **case**
statement does this. It includes something we haven't
used so far; the pipe symbol between the **0** and **1** means
that either pattern is an acceptable match. If the number
of arguments is either of these, the built-in **cd** is executed.

The next alternative is for two arguments, which is where
we'll add the new functionality to **cd**. The first thing that
has to be done is finding and replacing the old string with
the new one. We use the pattern matching and


replacement that we saw in the last chapter, the result
being assigned to **newdir**. If the substitution didn't take
place, the pathname will be unchanged. We'll use this fact
in the next few lines.

Another **case** statement chooses between performing the
**cd** or reporting an error because the new directory is
unchanged. The ***** alternative is a catchall for anything
other than the current pathname (caught by the first
alternative).

You might notice one small problem with this code: if
your old and new strings are the same you'll get **bash::
cd: bad substitution**. It should just leave you in the same
directory with no error message, but because the directory
path doesn't change, it uses the first alternative in the
inner **case** statement. The problem lies in knowing if _sed_
has performed a substitution or not. You might like to
think about ways to fix this problem (hint: you could use
_grep_ to check whether the pathname has the _old_ string in
it).

The last alternative in the outer **case** statement prints an
error message if there are more than two arguments.

[10]To make the function a little clearer, we've used some

advanced I/O redirection. I/O redirection is covered in
Chapter 7.


# select

All of the flow-control constructs we have seen so far are
also available in the Bourne shell, and the C shell has
equivalents with different syntax. Our next construct,
**select** , is available only in the Korn shell and _bash_ ;[11]
moreover, it has no analogy in conventional programming
languages.

**select** allows you to generate simple menus easily. It has
concise syntax, but it does quite a lot of work. The syntax
is:

```
select name
[in
list
]
do
statements that can use
$name...
done
```
This is the same syntax as **for** except for the keyword
**select**. And like **for** , you can omit the **in** _list_ and it will
default to " **$@** ", i.e., the list of quoted command-line
arguments. Here is what **select** does:

1. Generates a menu of each item in list, formatted
    with numbers for each choice
2. Prompts the user for a number


3. Stores the selected choice in the variable name
    and the selected number in the built-in variable
    **REPLY**
4. Executes the statements in the body
5. Repeats the process forever (but see below for
    how to exit)

Here is a task that adds another command to our _pushd_
and _popd_ utilities.

```
Task 5-5
```
```
Write a function that allows the user to select a directory
from a list of directories currently in the pushd directory
stack. The selected directory is moved to the front of the
stack and becomes the current working directory.
```
The display and selection of directories is best handled by
using **select**. We can start off with something along the
lines of:[12]

```
selectd ( )
{
PS3='directory? '
select selection in $DIR_STACK; do
if [ $selection ]; then
# statements that manipulate the stack...
break
```

```
else
echo 'invalid selection.'
fi
done
}
```
If you type **DIR_STACK=** " **/usr /home /bin** " and execute
this function, you'll see:

```
1) /usr
2) /home
3) /bin
directory?
```
The built-in shell variable **PS3** contains the prompt string
that **select** uses; its default value is the not particularly
useful " **#?** ". So the first line of the above code sets it to a
more relevant value.

The **select** statement constructs the menu from the list of
choices. If the user enters a valid number (from 1 to the
number of directories), then the variable **selection** is set
to the corresponding value; otherwise it is null. (If the
user just presses RETURN, the shell prints the menu
again.)

The code in the loop body checks if **selection** is non-null.
If so, it executes the statements we will add in a short
while; then the **break** statement exits the **select** loop. If
**selection** is null, the code prints an error message and
repeats the menu and prompt.

The **break** statement is the usual way of exiting a **select**
loop. Actually (like its analog in Java and C), it can be


used to exit any surrounding control structure we've seen
so far (except **case** , where the double semicolons act like
**break** ) as well as the **while** and **until** we will see soon.
We haven't introduced **break** until now because it is
considered bad coding style to use it to exit a loop.
However, it can make code easier to read if used
judiciously. _break_ is necessary for exiting **select** when the
user makes a valid choice.[13]

Now we'll add the missing pieces to the code:

```
selectd ( )
{
PS3='directory? '
dirstack=" $DIR_STACK "
```
```
select selection in $dirstack; do
if [ $selection ]; then
DIR_STACK="$selection${dirstack%% $selection *}"
DIR_STACK="$DIR_STACK ${dirstack##* $selection }"
DIR_STACK=${DIR_STACK% }
cd $selection
break
else
echo 'invalid selection.'
fi
done
}
```
The first two lines initialize environment variables.
**dirstack** is a copy of **DIR_STACK** with spaces
appended at the beginning and end so that each directory
in the list is of the form _space directory space_. This form
simplifies the code when we come to manipulating the
directory stack.


The **select** and **if** statements are the same as in our initial
function. The new code inside the **if** uses _bash_ 's
pattern-matching capability to manipulate the directory
stack.

The first statement sets **DIR_STACK** to **selection** ,
followed by **dirstack** with everything from **selection** to
the end of the list removed. The second statement adds
everything in the list from the directory following
**selection** to the end of **DIR_STACK**. The next line
removes the trailing space that was appended at the start.
To complete the operation, a **cd** is performed to the new
directory, followed by a **break** to exit the **select** code.

As an example of the list manipulation performed in this
function, consider a **DIR_STACK** set to _/home /bin
/usr2_. In this case, **dirstack** would become _/home /bin
/usr2_. Typing _selectd_ would result in:

```
$ selectd
1) /home
2) /bin
3) /usr2
directory?
```
After selecting _/bin_ from the list, the first statement inside
the _if_ section sets **DIR_STACK** to _/bin_ followed by
**dirstack** with everything from _/bin_ onwards removed,
i.e., _/home_.

The second statement then takes **DIR_STACK** and
appends everything in **dirstack** following _/bin_ (i.e., _/usr2_ )


to it. The value of **DIR_STACK** becomes _/bin /home
/usr2_. The trailing space is removed in the next line.

[11] **select** is not available in _bash_ versions prior to 1.14.

[12]Versions of _bash_ prior to 1.14.3 have a serious bug

with **select**. These versions will crash if the **select** list is
empty. In this case, surround **select** s with a test for a null
list.

[13]A user can also type CTRL-D (for end-of-input) to

get out of a **select** loop. This gives the user a uniform way
of exiting, but it doesn't help the shell programmer much.


# while and until

The remaining two flow control constructs _bash_ provides
are **while** and **until**. These are similar; they both allow a
section of code to be run repetitively while (or until) a
certain condition becomes true. They also resemble
analogous constructs in Pascal ( **while** / **do** and
**repeat** / **until** ) and C ( **while** and **do** / **until** ).

**while** and **until** are actually most useful when combined
with features we will see in the next chapter, such as
integer arithmetic, input/output of variables, and
command-line processing. Yet we can show a useful
example even with what we have covered so far.

The syntax for **while** is:

```
while condition
do
statements...
done
```
For **until** , just substitute **until** for **while** in the above
example. As with **if** , the _condition_ is really a list of
_statements_ that are run; the exit status of the last one is
used as the value of the condition. You can use a
conditional with **test** here, just as you can with **if**.

Note that the _only_ difference between **while** and **until** is
the way the condition is handled. In **while** , the loop


executes as long as the condition is true; in **until** , it runs
as long as the condition is false. The **until** condition is
checked at the _top_ of the loop, _not_ at the bottom as it is in
analogous constructs in C and Pascal.

The result is that you can convert any **until** into a **while**
by simply negating the condition. The only place where
**until** might be more meaningful is something like this:

```
until command
; do
statements...
done
```
The meaning of this is essentially, "Do _statements_ until
_command_ runs correctly." This is not a likely
contingency.

Here is an earlier task that can be rewritten using a **while**.

```
Task 5-6
```
```
Reimplement Task 5-2 without the use of the IFS
variable.
```
We can use the **while** construct and pattern matching to
traverse the **PATH** list:

```
path=$PATH:
```

```
while [ $path ]; do
ls -ld ${path%%:*}
path=${path#*:}
done
```
The first line copies **PATH** to a temporary copy, **path** ,
and appends a colon to it. Normally colons are used only
between directories in **PATH** ; adding one to the end
makes the code simple.

Inside the **while** loop we display the directory with _ls_ as
we did in Task 5-2. **path** is then updated by removing the
first directory pathname and colon (which is why we
needed to append the colon in the first line of the script).
The **while** will keep looping until **$path** expands to
nothing (the empty string ""), which occurs once the last
directory in **path** has been listed.

Here is another task that is a good candidate for **until**.

```
Task 5-7
```
```
Write a script that attempts to copy a file to a directory
and, if it fails, waits five seconds, then tries again,
continuing until it succeeds.
```
Here is the code:

```
until cp $1 $2; do
echo 'Attempt to copy failed. waiting...'
```

```
sleep 5
done
```
This is a fairly simple use of **until**. First, we use the _cp_
command to perform the copy for us. If it can't perform
the copy for any reason, it will return with a non-zero exit
code. We set our **until** loop so that if the result of the
copy is _not_ 0 then the script prints a message and waits
five seconds.

As we said earlier, an **until** loop can be converted to a
**while** by the use of the**!** operator:

```
while! cp $1 $2; do
echo 'Attempt to copy failed. waiting...'
sleep 5
done
```
In our opinion, you'll seldom need to use **until** ; therefore,
we'll use **while** throughout the rest of this book. We'll see
further use of the **while** construct inChapter 7.


# Chapter 6. Command-Line

# Options and Typed

# Variables

You should have a healthy grasp of shell programming
techniques now that you have gone through the previous
chapters. What you have learned up to this point enables
you to write many non-trivial, useful shell scripts and
functions.

Still, you may have noticed some remaining gaps in the
knowledge you need to write shell code that behaves like
the UNIX commands you are used to. In particular, if you
are an experienced UNIX user, it might have occurred to
you that none of the example scripts shown so far have
the ability to handle _options_ preceded by a dash ( **-** ) on the
command line. And if you program in a conventional
language like C or Pascal, you will have noticed that the
only type of data that we have seen in shell variables is
character strings; we haven't seen how to do arithmetic,
for example.

These capabilities are certainly crucial to the shell's
ability to function as a useful UNIX programming
language. In this chapter, we will show how _bash_
supports these and related features.


# Command-Line Options

We have already seen many examples of the _positional
parameters_ (variables called **1** , **2** , **3** , etc.) that the shell
uses to store the command-line arguments to a shell script
or function when it runs. We have also seen related
variables like ***** (for the string of all arguments) and **#** (for
the number of arguments).

Indeed, these variables hold all of the information on the
user's command-line. But consider what happens when
options are involved. Typical UNIX commands have the
form _command_ [- _options_ ] _args_ , meaning that there can be
0 or more options. If a shell script processes the
command **teatime alice hatter** , then **$1** is "alice" and **$2**
is "hatter". But if the command is **teatime -o alice hatter** ,
then **$1** is **-o** , **$2** is "alice", and **$3** is "hatter".

You might think you could write code like this to handle
it:

```
if [ $1 = -o ]; then
code that processes the -o option
1=$2
2=$3
fi
```
```
normal processing of $1 and $2...
```
But this code has several problems. First, assignments
like **1=$2** are illegal because positional parameters are
read-only. Even if they were legal, another problem is


that this kind of code imposes limitations on how many
arguments the script can handle—which is very unwise.
Furthermore, if this command had several possible
options, the code to handle all of them would get very
messy very quickly.

## shift

Luckily, the shell provides a way around this problem.
The command **shift** performs the function of:

```
1=$2
2=$3
...
```
for every argument, regardless of how many there are. If
you supply a numeric argument to **shift** , it will shift the
arguments that many times over; for example, **shift 3** has
this effect:

```
1=$4
2=$5
...
```
This leads immediately to some code that handles a single
option (call it **-o** ) and arbitrarily many arguments:

```
if [ $1 = -o ]; then
process the -o option
shift
fi
normal processing of arguments...
```

After the **if** construct, **$1** , **$2** , etc., are set to the correct
arguments.

We can use **shift** together with the programming features
we have seen so far to implement simple option schemes.
However, we will need additional help when things get
more complex. The **getopts** built-in command, which we
will introduce later, provides this help.

**shift** by itself gives us enough power to implement the **-**
_N_ option to the _highest_ script we saw inChapter 4(Task
4-1). Recall that this script takes an input file that lists
artists and the number of albums you have by them. It
sorts the list and prints out the _N_ highest numbers, in
descending order. The code that does the actual data
processing is:

```
filename=$1
howmany=${2:-10}
sort -nr $filename | head -$howmany
```
Our original syntax for calling this script was **highest**
_filename_ **[-** _N_ **]** , where _N_ defaults to 10 if omitted. Let's
change this to a more conventional UNIX syntax, in
which options are given before arguments: **highest [-** _N_ **]**
_filename_. Here is how we would write the script with this
syntax:

```
if [ -n "$(echo $1 | grep '^-[0-9][0-9]*$')" ]; then
howmany=$1
shift
elif [ -n "$(echo $1 | grep '^-')" ]; then
print 'usage: highest [-N] filename'
```

```
exit 1
else
howmany="-10"
fi
```
```
filename=$1
sort -nr $filename | head $howmany
```
This uses the _grep_ search utility to test if **$1** matches the
appropriate pattern. To do this we provide the regular
expression **^-[0-9][0-9]*$** to _grep_ , which is interpreted as
"an initial dash followed by a digit, optionally followed
by one or more digits." If a match is found then _grep_ will
return the match and the test will be true, otherwise _grep_
will return nothing and processing will pass to the **elif**
test. Notice that we have enclosed the regular expression
in single quotes to stop the shell from interpreting the **$**
and ***** , and pass them through to _grep_ unmodified.

If **$1** doesn't match, we test to see if it's an option at all,
i.e., if it matches the pattern **-** followed by anything else.
If it does, then it's invalid; we print an error message and
exit with error status. If we reach the final ( **else** ) case, we
assume that **$1** is a filename and treat it as such in the
ensuing code. The rest of the script processes the data as
before.

We can extend what we have learned so far to a general
technique for handling multiple options. For the sake of
concreteness, assume that our script is called _alice_ and we
want to handle the options **-a** , **-b** , and **-c** :


```
while [ -n "$(echo $1 | grep '-')" ]; do
case $1 in
-a ) process option -a
;;
-b ) process option -b
;;
-c ) process option -c
;;
* ) echo 'usage: alice [-a] [-b] [-c] args...'
exit 1
esac
shift
done
normal processing of arguments...
```
This code checks **$1** repeatedly as long as it starts with a
dash ( **-** ). Then the **case** construct runs the appropriate
code depending on which option **$1** is. If the option is
invalid—i.e., if it starts with a dash but isn't **-a** , **-b** , or
**-c—** then the script prints a usage message and returns
with an error exit status.

After each option is processed, the arguments are shifted
over. The result is that the positional parameters are set to
the actual arguments when the **while** loop finishes.

Notice that this code is capable of handling options of
arbitrary length, not just one letter (e.g., **-adventure**
instead of **-a** ).


## Options with Arguments

We need to add one more ingredient to make option
processing really useful. Recall that many commands
have options that take their _own_ arguments. For example,
the _cut_ command, on which we relied heavily inChapter
4, accepts the option **-d** with an argument that determines
the field delimiter (if it is not the default TAB). To handle
this type of option, we just use another **shift** when we are
processing the option.

Assume that, in our _alice_ script, the option **-b** requires its
own argument. Here is the modified code that will
process it:

```
while [ -n "$(echo $1 | grep '-')" ]; do
case $1 in
-a ) process option -a ;;
-b ) process option -b
$2 is the option's argument
shift ;;
-c ) process option -c ;;
* ) echo 'usage: alice [-a] [-b barg] [-c] args...'
exit 1
esac
shift
done
```
```
normal processing of arguments...
```

## getopts

So far, we have a complete, but constrained, way of
handling command-line options. The above code does not
allow a user to combine arguments with a single dash,
e.g., **-abc** instead of **-a -b -c**. It also doesn't allow one to
specify arguments to options without a space in between,
e.g., **-barg** in addition to **-b arg** .[1]

The shell provides a built-in way to deal with multiple
complex options without these constraints. The built-in
command **getopts** [2]can be used as the condition of the
**while** in an option-processing loop. Given a specification
of which options are valid and which require their own
arguments, it sets up the body of the loop to process each
option in turn.

**getopts** takes two arguments. The first is a string that can
contain letters and colons. Each letter is a valid option; if
a letter is followed by a colon, the option requires an
argument. **getopts** picks options off the command line
and assigns each one (without the leading dash) to a
variable whose name is **getopts** 's second argument. As
long as there are options left to process, **getopts** will
return exit status 0; when the options are exhausted, it
returns exit status 1, causing the **while** loop to exit.


**getopts** does a few other things that make option
processing easier; we'll encounter them as we examine
how to use **getopts** in this example:

```
while getopts ":ab:c" opt; do
case $opt in
a ) process option -a
;;
b ) process option -b
$OPTARG is the option's argument
;;
c ) process option -c
;;
\? ) echo 'usage: alice [-a] [-b barg] [-c] args...'
exit 1
esac
done
shift $(($OPTIND - 1))
normal processing of arguments...
```
The call to **getopts** in the **while** condition sets up the loop
to accept the options **-a** , **-b** , and **-c** , and specifies that **-b**
takes an argument. (We will explain the : that starts the
option string in a moment.) Each time the loop body is
executed, it will have the latest option available, without
a dash ( **-** ), in the variable **opt**.

If the user types an invalid option, **getopts** normally
prints an unfortunate error message (of the form **cmd:
getopts: illegal option — o** ) and sets **opt** to**?**. However
if you begin the option letter string with a colon, **getopts**
won't print the message.[3] We recommend that you
specify the colon and provide your own error message in
a case that handles**?** , as above.


We have modified the code in the **case** construct to reflect
what **getopts** does. But notice that there are no more **shift**
statements inside the **while** loop: **getopts** does not rely on
**shift** s to keep track of where it is. It is unnecessary to
shift arguments over until **getopts** is finished, i.e., until
the **while** loop exits.

If an option has an argument, **getopts** stores it in the
variable **OPTARG** , which can be used in the code that
processes the option.

The one **shift** statement left is after the **while** loop.
**getopts** stores in the variable **OPTIND** the number of the
next argument to be processed; in this case, that's the
number of the first (non-option) command-line argument.
For example, if the command line were **alice -ab rabbit** ,
then **$OPTIND** would be "3". If it were **alice -a -b
rabbit** , then **$OPTIND** would be "4".

The expression **$(($OPTIND - 1))** is an arithmetic
expression (as we'll see later in this chapter) equal to
**$OPTIND** minus 1. This value is used as the argument to
**shift**. The result is that the correct number of arguments
are shifted out of the way, leaving the "real" arguments as
**$1** , **$2** , etc.

Before we continue, now is a good time to summarize
everything **getopts** does:

1. Its first argument is a string containing all valid
    option letters. If an option requires an argument,


```
a colon follows its letter in the string. An initial
colon causes getopts not to print an error
message when the user gives an invalid option.
```
2. Its second argument is the name of a variable that
    will hold each option letter (without any leading
    dash) as it is processed.
3. If an option takes an argument, the argument is
    stored in the variable **OPTARG**.
4. The variable **OPTIND** contains a number equal
    to the next command-line argument to be
    processed. After **getopts** is done, it equals the
    number of the first "real" argument.

The advantages of **getopts** are that it minimizes extra
code necessary to process options and fully supports the
standard UNIX option syntax (as specified in _intro_ of the
_User's Manual_ ).

As a more concrete example, let's return to our graphics
utility (Task 4-2). So far, we have given our script the
ability to process various types of graphics files such as
PCX files (ending with _.pcx_ ), GIF files ( _.gif_ ), XPM files
( _.xpm_ ), etc. As a reminder, here is what we have coded in
the script so far:

```
filename=$1
```
```
if [ -z $filename ]; then
echo "procfile: No file specified"
```

```
exit 1
fi
```
```
for filename in "$@"; do
pnmfile=${filename%.*}.ppm
case $filename in
*.jpg ) exit 0 ;;
*.tga ) tgatoppm $filename > $pnmfile ;;
*.xpm ) xpmtoppm $filename > $pnmfile ;;
*.pcx ) pcxtoppm $filename > $pnmfile ;;
*.tif ) tifftopnm $filename > $pnmfile ;;
*.gif ) giftopnm $filename > $pnmfile ;;
* ) echo "procfile: $filename is an unknown graphics file."
exit 1 ;;
esac
outfile=${pnmfile%.ppm}.new.jpg
pnmtojpeg $pnmfile > $outfile
rm $pnmfile
done
```
This script works quite well, in that it will convert the
various different graphics files that we have lying around
into JPEG files suitable for our web page. However,
NetPBM has a whole range of useful utilities besides file
converters that we could use on the images. It would be
nice to be able to select some of them from our script.

Things we might wish to do to modify the images include
changing the size and placing a border around them. We
want to make the script as flexible as possible; we will
want to change the size of the resulting images and we
might not want a border around every one of them, so we
need to be able to specify to the script what it should do.
This is where the command-line option processing will
come in useful.


We can change the size of an image by using the
NetPBM utility _pnmscale_. You'll recall from the last
chapter that the NetPBM package has its own format
called PNM, the Portable Anymap. The fancy utilities
we'll be using to change the size and add borders work on
PNMs. Fortunately, our script already converts the
various formats we give it into PNMs. Besides a PNM
file, _pnmscale_ also requires some arguments telling it
how to scale the image.[4]There are various different
ways to do this, but the one we'll choose is _-xysize_ which
takes a horizontal and a vertical size in pixels for the final
image.[5]

The other utility we need is _pnmmargin_ , which places a
colored border around an image. Its arguments are the
width of the border in pixels and the color of the border.

Our graphics utility will need some options to reflect the
ones we have just seen. **-s** _size_ will specify a size into
which the final image will fit (minus any border), **-w**
_width_ will specify the width of the border around the
image, and **-c** _color-name_ will specify the color of the
border.

Here is the code for the script _procimage_ that includes the
option processing:

```
# Set up the defaults
size=320
width=1
colour="-color black"
usage="Usage: $0 [-s N] [-w N] [-c S] imagefile..."
```

```
while getopts ":s:w:c:" opt; do
case $opt in
s ) size=$OPTARG ;;
w ) width=$OPTARG ;;
c ) colour="-color $OPTARG" ;;
\? ) echo $usage
exit 1 ;;
esac
done
shift $(($OPTIND - 1))
if [ -z "$@" ]; then
echo $usage
exit 1
fi
# Process the input files
for filename in "$*"; do
ppmfile=${filename%.*}.ppm
case $filename in
*.gif ) giftopnm $filename > $ppmfile ;;
*.tga ) tgatoppm $filename > $ppmfile ;;
*.xpm ) xpmtoppm $filename > $ppmfile ;;
*.pcx ) pcxtoppm $filename > $ppmfile ;;
*.tif ) tifftopnm $filename > $ppmfile ;;
*.jpg ) jpegtopnm -quiet $filename > $ppmfile ;;
* ) echo "$0: Unknown filetype '${filename##*.}'"
exit 1;;
esac
outfile=${ppmfile%.ppm}.new.jpg
pnmscale -quiet -xysize $size $size $ppmfile |
pnmmargin $colour $width |
pnmtojpeg > $outfile
rm $ppmfile
done
```
The first several lines of this script initialize variables
with default settings. The defaults set the image size to
320 pixels and a black border of width 1 pixel.


The **while** , **getopts** , and **case** constructs process the
options in the same way as in the previous example. The
code for the first three options assigns the respective
argument to a variable (replacing the default value). The
last option is a catchall for any invalid options.

The rest of the code works in much the same way as in
the previous example except we have added the _pnmscale_
and _pnmmargin_ utilities in a processing pipeline at the
end.

The script also now generates a different filename; it
appends _.new.jpg_ to the basename. This allows us to
process a JPEG file as input, applying scaling and
borders, and write it out without destroying the original
file.

This version doesn't address every issue, e.g., what if we
don't want any scaling to be performed? We'll return to
this script and develop it further in the next chapter.

[1] Although most UNIX commands allow this, it is

actually contrary to the Command Syntax Standard Rules
in _intro_ of the _User's Manual_.

[2] **getopts** replaces the external command _getopt_ , used in

Bourne shell programming; **getopts** is better integrated
into the shell's syntax and runs more efficiently. C
programmers will recognize **getopts** as very similar to the
standard library routine _getopt_.


[3]You can also turn off the **getopts** messages by setting

the environment variable **OPTERR** to 0. We will
continue to use the colon method in this book.

[4]We'll also need the _-quiet_ option, which suppresses

diagnostic output from some NetPBM utilities.

[5]Actually, _-xysize_ fits the image into a box defined by

its arguments without changing the _aspect ratio_ of the
image, i.e., without stretching the image horizontally or
vertically. For example, if you had an image of size 200
by 100 pixels and you processed it with _pnmscale -xysize
100 100_ , you'd end up with an image of size 100 by 50
pixels.


# Typed Variables

So far we've seen how _bash_ variables can be assigned
textual values. Variables can also have other attributes,
including being _read only_ and being of type _integer_.

You can set variable attributes with the **declare** built-in.
[6] Table 6-1 summarizes the available options with

**declare** .[7]A **-** turns the option on, while **+** turns it off.

**Table 6-1. Declare options**

```
Option Meaning
```
```
-a The variables are treated as arrays
```
```
-f Use function names only
```
```
-F Display function names without definitions
```
```
-i The variables are treated as integers
```

```
Option Meaning
```
```
-r Makes the variables read-only
```
```
-x
Marks the variables for export via the
environment
```
Typing **declare** on its own displays the values of all
variables in the environment. The **-f** option limits this
display to the function names and definitions currently in
the environment. **-F** limits it further by displaying only
the function names.

The **-a** option declares _arrays—_ a variable type that we
haven't seen yet, but will be discussed shortly.

The **-i** option is used to create an integer variable, one that
holds numeric values and can be used in and modified by
arithmetic operations. Consider this example:

```
$ val1=12 val2=5
$ result1=val*val2
$ echo $result1
val1*val2
$
$ declare -i val3=12 val4=5
$ declare -i result2
$ result2=val3*val4
```

```
$ echo $result2
60
```
In the first example, the variables are ordinary shell
variables and the result is just the string "val1*val2". In
the second example, all of the variables have been
declared as type _integer_. The variable **result** contains the
result of the arithmetic computation twelve multiplied by
five. Actually, we didn't need to declare **val3** and **val4** as
type integer. Anything being assigned to **result2** is
interpreted as an arithmetic statement and evaluation is
attempted.

The **-x** option to **declare** operates in the same way as the
**export** built-in that we saw inChapter 3. It allows the
listed variables to be exported outside the current shell
environment.

The **-r** option creates a read-only variable, one that cannot
have its value changed by subsequent assignment
statements and cannot be **unset**.

A related built-in is **readonly** _name._ .. which operates in
exactly the same way as **declare -r**. **readonly** has three
options: **-f** , which makes **readonly** interpret the name
arguments as function names rather than variable names,
**-p** , which makes the built-in print a list of all read-only
names, and **-a** , which interprets the name arguments as
arrays.

Lastly, variables **declare** d in a function are local to that
function, just like using **local** to declare them.


[6]The **typeset** built-in is synonymous with **declare** but is

considered obsolete.

[7]The **-a** and **-F** options are not available in _bash_ prior to

version 2.0.


# Integer Variables and

# Arithmetic

The expression **$(($OPTIND - 1))** in the last graphics
utility example shows another way that the shell can do
integer arithmetic. As you might guess, the shell
interprets words surrounded by **$((** and **))** as arithmetic
expressions.[8]Variables in arithmetic expressions do _not_
need to be preceded by dollar signs, though it is not
wrong to do so.

Arithmetic expressions are evaluated inside double
quotes, like tildes, variables, and command substitutions.
We're _finally_ in a position to state the definitive rule about
quoting strings: when in doubt, enclose a string in single
quotes, unless it contains tildes or any expression
involving a dollar sign, in which case you should use
double quotes.

For example, the _date_ command on modern versions of
UNIX accepts arguments that tell it how to format its
output. The argument **+%j** tells it to print the day of the
year, i.e., the number of days since December 31st of the
previous year.

We can use **+%j** to print a little holiday anticipation
message:


```
echo "Only $(( (365-$(date +%j)) / 7 )) weeks until the New Year"
```
We'll show where this fits in the overall scheme of
command-line processing inChapter 7.

The arithmetic expression feature is built into _bash_ 's
syntax, and was available in the Bourne shell (most
versions) only through the external command _expr_. Thus
it is yet another example of a desirable feature provided
by an external command being better integrated into the
shell. **getopts** , as we have already seen, is another
example of this design trend.

_bash_ arithmetic expressions are equivalent to their
counterparts in the Java and C languages.[9]Precedence
and associativity are the same as in C.Table 6-2shows
the arithmetic operators that are supported. Although
some of these are (or contain) special characters, there is
no need to backslash-escape them, because they are
within the **$((** ... **))** syntax.

**Table 6-2. Arithmetic operators**

```
Operator Meaning
```
```
++ Increment by one (prefix and postfix)
```
```
— Decrement by one (prefix and postfix)
```

**Operator Meaning**

+ Plus

- Minus

***** Multiplication

/ Division (with truncation)

**%** Remainder

** Exponentiation[10]

<< Bit-shift left

>> Bit-shift right

**&** Bitwise and


```
Operator Meaning
```
```
| Bitwise or
```
```
~ Bitwise not
```
```
! Logical not
```
```
^ Bitwise exclusive or
```
```
, Sequential evaluation
```
```
[10]Note that ** is not in the C language.
```
The **++** and - operators are useful when you want to
increment or decrement a value by one.[11]They work the
same as in Java and C, e.g., _value++_ increments value by

1. This is called _post-increment_ ; there is also a
_pre-increment_ : _++value_. The difference becomes evident
with an example:

```
$ i=0
$ echo $i
```

```
0
$ echo $((i++))
0
$ echo $i
1
$ echo $((++i))
2
$ echo $i
2
```
In both cases the value has been incremented by one.
However, in the first case (post-increment) the value of
the variable was passed to echo and _then_ the variable was
incremented. In the second case (pre-increment) the
increment was performed and then the variable passed to
echo.

Parentheses can be used to group subexpressions. The
arithmetic expression syntax also (as in C) supports
relational operators as "truth values" of 1 for true and 0
for false.Table 6-3shows the relational operators and the
logical operators that can be used to combine relational
expressions.

**Table 6-3. Relational operators**

```
Operator Meaning
```
```
< Less than
```

```
Operator Meaning
```
```
> Greater than
```
```
<= Less than or equal to
```
```
>= Greater than or equal to
```
```
== Equal to
```
```
!= Not equal to
```
```
&& Logical and
```
```
|| Logical or
```
For example, **$((3 > 2))** has the value 1; **$(( (3 > 2) || (4
<= 1) ))** also has the value 1, since at least one of the two
subexpressions is true.


The shell also supports base _N_ numbers, where _N_ can be
from 2 to 36. The notation _B_ **#** _N_ means " _N_ base _B_ ". Of
course, if you omit the _B_ **#** , the base defaults to 10.

## Arithmetic Conditionals

InChapter 5, we saw how to compare strings by the use
of **[...]** notation (or with the **test** built-in). Arithmetic
conditions can also be tested in this way. However, the
tests have to be carried out with their own operators.
These are shown inTable 6-4.

**Table 6-4. Test relational operators**

```
Operator Meaning
```
```
-lt Less than
```
```
-gt Greater than
```
```
-le Less than or equal to
```
```
-ge Greater than or equal to
```

```
Operator Meaning
```
```
-eq Equal to
```
```
-ne Not equal to
```
And as with string comparisons, the arithmetic test
returns a result of true or false; 0 if true, 1 otherwise. So,
for example, **[ 3 -gt 2 ]** produces exit status 0, as does **[ \(
3 -gt 2 \) || \( 4 -le 1 \) ]** , but **[ \( 3 -gt 2 \) && \( 4 -le 1 \)
]** has exit status 1 since the second subexpression isn't
true.

In these examples we have had to escape the parentheses
and pass them to **test** as separate arguments. As you can
see, the result can look rather unreadable if there are
many parentheses.

Another way to make arithmetic tests is to use the **$((...))**
form to encapsulate the condition. For example: **[ $(((3 >
2) && (4 <= 1))) = 1 ]**. This evaluates the conditionals
and then compares the resulting value to 1 (true).[12]

There is an even neater and more efficient way of
performing an arithmetic test: by using the **((...))**
construct.[13] This returns an exit status of 0 if the
expression is true, and 1 otherwise.


The above expression using this construct becomes **(( (3
> 2) && (4 <= 1) ))**. This example returns with an exit
status of 1 because, as we said, the second subexpression
is false.


## Arithmetic Variables and

## Assignment

As we saw earlier, you can define integer variables by
using **declare**. You can also evaluate arithmetic
expressions and assign them to variables with the use of
**let**. The syntax is:

```
let intvar = expression
```
It is not necessary (because it's actually redundant) to
surround the expression with **$((** and **))** in a **let** statement.
**let** doesn't create a variable of type integer; it only causes
the expression following the assignment to be interpreted
as an arithmetic one. As with any variable assignment,
there must not be any space on either side of the equal
sign ( **=** ). It is good practice to surround expressions with
quotes, since many characters are treated as special by the
shell (e.g., ***** , **#** , and parentheses); furthermore, you must
quote expressions that include whitespace (spaces or
TABs). SeeTable 6-5for examples.

**Table 6-5. Sample integer expression
assignments**


**Assignment Value**

let x= **$x**

#### 1+4 5

#### `1 + 4' 5

#### `(2+3) * 5' 25

#### `2 + 3 * 5' 17

#### `17 / 3' 5

#### `17 % 3' 2

#### `1<<4' 16

#### `48>>3' 6


```
Assignment Value
```
#### `17 & 3' 1

#### `17 | 3' 19

#### `17 ^ 3' 18

```
Task 6-1
```
```
Here is a small task that makes use of integer arithmetic.
Write a script called ndu that prints a summary of the
disk space usage for each directory argument (and any
subdirectories), both in terms of bytes, and kilobytes or
megabytes (whichever is appropriate).
```
Here is the code:

```
for dir in ${*:-.}; do
if [ -e $dir ]; then
result=$(du -s $dir | cut -f 1)
let total=$result*1024
```
```
echo -n "Total for $dir = $total bytes"
```

```
if [ $total -ge 1048576 ]; then
echo " ($((total/1048576)) Mb)"
elif [ $total -ge 1024 ]; then
echo " ($((total/1024)) Kb)"
fi
fi
done
```
To obtain the disk usage of files and directories, we can
use the UNIX utility _du_. The default output of _du_ is a list
of directories with the amount of space each one uses,
and looks something like this:

```
6 ./toc
3 ./figlist
6 ./tablist
1 ./exlist
1 ./index/idx
22 ./index
39.
```
If you don't specify a directory to _du_ , it will use the
current directory (.). Each directory and subdirectory is
listed along with the amount of space it uses. The grand
total is given in the last line.

The amount of space used by each directory and all the
files in it is listed in terms of blocks. Depending on the
UNIX system you are running on, one block can
represent 512 or 1024 bytes. Each file and directory uses
at least one block. Even if a file or directory is empty, it is
still allocated a block of space in the filesystem.

In our case, we are only interested in the total usage,
given on the last line of _du_ 's output. To obtain only this


line, we can use the **-s** option of _du_. Once we have the
line, we want only the number of blocks and can throw
away the directory name. For this we use our old friend
_cut_ to extract the first field.

Once we have the total, we can multiply it by the number
of bytes in a block (1024 in this case) and print the result
in terms of bytes. We then test to see if the total is greater
than the number of bytes in one megabyte (1048576
bytes, which is 1024 **x** 1024) and if it is, we can print how
many megabytes it is by dividing the total by this large
number. If not, we see if it can be expressed in kilobytes,
otherwise nothing is printed.

We need to make sure that any specified directories exist,
otherwise _du_ will print an error message and the script
will fail. We do this by using the test for file or directory
existence ( **-e** ) that we saw inChapter 5before calling _du_.

To round out this script, it would be nice to imitate _du_ as
closely as possible by providing for multiple arguments.
To do this, we wrap the code in a **for** loop. Notice how
parameter substitution has been used to specify the
current directory if no arguments are given.

As a bigger example of integer arithmetic, we will
complete our emulation of the _pushd_ and _popd_ functions
(Task 4-8). Remember that these functions operate on
**DIR_STACK** , a stack of directories represented as a
string with the directory names separated by spaces.


_bash_ 's _pushd_ and _popd_ take additional types of
arguments, which are:

- **pushd +n** takes the nth directory in the stack
    (starting with 0), rotates it to the top, and **cd** s to
    it.
- **pushd** without arguments, instead of
    complaining, swaps the two top directories on the
    stack and **cd** s to the new top.
- **popd +n** takes the nth directory in the stack and
    just deletes it.

The most useful of these features is the ability to get at
the _n_ th directory in the stack. Here are the latest versions
of both functions:

```
.ps 8
pushd ( )
{
dirname=$1 if [ -n $dirname ] && [ \( -d $dirname \) -a
\( -x $dirname \) ]; then
DIR_STACK="$dirname ${DIR_STACK:-$PWD' '}"
cd $dirname
echo "$DIR_STACK"
else
echo "still in $PWD."
fi
}
```
```
popd ( )
{
if [ -n "$DIR_STACK" ]; then
```

```
DIR_STACK=${DIR_STACK#* }
```
```
cd ${DIR_STACK%% *}
echo "$PWD"
else
echo "stack empty, still in $PWD."
fi
}
```
To get at the _n_ th directory, we use a **while** loop that
transfers the top directory to a temporary copy of the
stack _n_ times. We'll put the loop into a function called
_getNdirs_ that looks like this:

```
getNdirs ( )
{
stackfront=''
let count=0
while [ $count -le $1 ]; do
target=${DIR_STACK%${DIR_STACK#* }}
stackfront="$stackfront$target"
DIR_STACK=${DIR_STACK#$target}
let count=count+1
done
```
```
stackfront=${stackfront%$target}
}
```
The argument passed to _getNdirs_ is the _n_ in question. The
variable **target** contains the directory currently being
moved from **DIR_STACK** to a temporary stack,
**stackfront**. **target** will contain the _n_ th directory and
**stackfront** will have all of the directories above (and
including) **target** when the loop finishes. **stackfront**
starts as null; **count** , which counts the number of loop
iterations, starts as 0.


The first line of the loop body copies the first directory on
the stack to **target**. The next line appends **target** to
**stackfront** and the following line removes **target** from
the stack **${DIR_STACK#$target}**. The last line
increments the counter for the next iteration. The entire
loop executes _n_ +1 times, for values of **count** from 0 to _N_.

When the loop finishes, the directory in **$target** is the _n_ th
directory. The expression **${stackfront%$target}**
removes this directory from **stackfront** so that
**stackfront** will contain the first _n_ -1 directories.
Furthermore, **DIR_STACK** now contains the "back" of
the stack, i.e., the stack _without_ the first _n_ directories.
With this in mind, we can now write the code for the
improved versions of _pushd_ and _popd_ :

```
pushd ( )
{
if [ $(echo $1 | grep '^+[0-9][0-9]*$') ]; then
```
```
# case of pushd +n: rotate n-th directory to top
let num=${1#+}
getNdirs $num
```
```
DIR_STACK="$target$stackfront$DIR_STACK"
cd $target
echo "$DIR_STACK"
```
```
elif [ -z "$1" ]; then
# case of pushd without args; swap top two directories
firstdir=${DIR_STACK%% *}
DIR_STACK=${DIR_STACK#* }
seconddir=${DIR_STACK%% *}
```

```
DIR_STACK=${DIR_STACK#* }
DIR_STACK="$seconddir $firstdir $DIR_STACK"
cd $seconddir
```
else
# normal case of pushd dirname
dirname=$1
if [ \( -d $dirname \) -a \( -x $dirname \) ]; then
DIR_STACK="$dirname ${DIR_STACK:-$PWD" "}"
cd $dirname
echo "$DIR_STACK"
else
echo still in "$PWD."
fi
fi
}

popd ( )
{
if [ $(echo $1 | grep '^+[0-9][0-9]*$') ]; then

```
# case of popd +n: delete n-th directory from stack
let num=${1#+}
getNdirs $num
DIR_STACK="$stackfront$DIR_STACK"
cd ${DIR_STACK%% *}
echo "$PWD"
```
```
else
```
```
# normal case of popd without argument
if [ -n "$DIR_STACK" ]; then
DIR_STACK=${DIR_STACK#* }
cd ${DIR_STACK%% *}
echo "$PWD"
else
echo "stack empty, still in $PWD."
```

```
fi
fi
}
```
These functions have grown rather large; let's look at
them in turn. The **if** at the beginning of _pushd_ checks if
the first argument is an option of the form **+** _N_. If so, the
first body of code is run. The first **let** simply strips the
plus sign (+) from the argument and assigns the
result—as an integer—to the variable **num**. This, in turn,
is passed to the _getNdirs_ function.

The next assignment statement sets **DIR_STACK** to the
new ordering of the list. Then the function **cd** s to the new
directory and prints the current directory stack.

The **elif** clause tests for no argument, in which case _pushd_
should swap the top two directories on the stack. The first
four lines of this clause assign the top two directories to
**firstdir** and **seconddir** , and delete these from the stack.
Then, as above, the code puts the stack back together in
the new order and **cd** s to the new top directory.

The **else** clause corresponds to the usual case, where the
user supplies a directory name as argument.

_popd_ works similarly. The **if** clause checks for the **+** _N_
option, which in this case means "delete the _n_ th
directory." A **let** extracts the _N_ as an integer; the _getNdirs_
function puts the first _n_ directories into **stackfront**.
Finally, the stack is put back together with the _n_ th


directory missing, and a _cd_ is performed in case the
deleted directory was the first in the list.

The **else** clause covers the usual case, where the user
doesn't supply an argument.

Before we leave this subject, here are a few exercises that
should test your understanding of this code:

1. Implement bash's **dirs** command and the options
    **+n** and **-l**. **dirs** by itself displays the list of
    currently remembered directories (those in the
    stack). The **+n** option prints out the _n_ th directory
    (starting at 0) and the **-l** option produces a long
    listing; any tildes ( **~** ) are replaced by the full
    pathname.
2. Modify the _getNdirs_ function so that it checks for
    _N_ exceeding the number of directories in the
    stack and exits with an appropriate error message
    if true.
3. Modify _pushd_ , _popd_ , and _getNdirs_ so that they
    use variables of type **integer** in the arithmetic
    expressions.
4. Change _getNdirs_ so that it uses _cut_ (with
    command substitution), instead of the **while** loop,
    to extract the first _N_ directories. This uses less
    code but runs more slowly because of the extra
    processes generated.


5. bash's versions of **pushd** and **popd** also have a
    _-N_ option. In both cases _-N_ causes the _n_ th
    directory from the right-hand side of the list to
    have the operation performed on it. As with _+N_ ,
    it starts at 0. Add this functionality.
6. Use _getNdirs_ to reimplement the selectd function
    from the last chapter.


## Arithmetic for Loops

Chapter 5introduced the **for** loop and briefly mentioned
another type of **for** loop, more akin to the construct found
in many programming languages like Java and C. This
type of **for** loop is called an _arithmetic_ **for** loop.[14]

The form of an arithmetic for loop is very similar to those
found in Java and C:

```
for (( initialisation ; ending condition ; update ))
do
statements...
done
```
There are four sections to the loop, the first three being
arithmetic expressions and the last being a set of
statements just as in the standard loop that we saw in the
last chapter.

The first expression, _initialisation_ , is something that is
done once at the start of the loop and if it evaluates to true
the loop continues its process; otherwise, it skips the loop
and continues with the next statement. When
_initialisation_ is true the loop then evaluates _ending
condition_. If this is true then it executes _statements_ ,
evaluates _update_ and repeats the cycle again by
evaluation _ending condition_. The loop continues until


_ending condition_ becomes false or the loop is exited via
one of the _statement_ s.

Usually _initialisation_ is used to set an arithmetic variable
to some initial value, _update_ updates that variable, and
_ending condition_ tests the variable. Any of the values may
be left out in which case they automatically evaluate to
true. The following simple example:

```
for ((;;))
do
read var
if [ "$var" = "." ]; then
break
fi
done
```
loops forever reading lines until a line consisting of a. is
found. We'll look at using the expressions in an arithmetic
**for** loop in our next task.

```
Task 6-2
```
```
Write a script that uses for loops to print out a
multiplication table for the numbers 1 to 12.
```
This task is best accomplished using nested **for** loops:

```
for (( i=1; i <= 12 ; i++ ))
do
for (( j=1 ; j <= 12 ; j++ ))
```

```
do
echo -ne "$(( j * i ))\t"
done
echo
done
```
The script begins with a **for** loop using a variable _i_ ; the
_initialisation_ clause sets _i_ to 1, the _ending condition_
clause tests _i_ against the limit (12 in our case), and the
_update_ clause adds 1 to _i_ each time around the loop. The
body of the loop is another **for** loop, this time with a
variable called _j_. This is identical to the _i_ **for** loop except
that _j_ is being updated.

The body of the _j_ loop has an echo statement where the
two variables are multiplied together and printed along
with a trailing tab. We deliberately don't print a newline
(with the - **n** option to echo) so that the numbers appear on
one line. Once the inner loop has finished a newline is
printed so that the set of numbers starts on the next line.

Arithmetic **for** loops are useful when dealing with arrays,
which we'll now look at.

[8]You can also use the older form **$[...]** , but we don't

recommend this because it will be phased out in future
versions of _bash_.

[9] The assignment forms of these operators are also

permitted. For example, **$((x += 2))** adds 2 to **x** and stores
the result back in **x**.


[11]++ and - are not available in versions of _bash_ prior to

2.04.

[12]Note that the truth values returned by **$((...))** are 1 for

true, 0 for false—the reverse of the **test** and exit statuses.

[13] **((...))** is not available in versions of _bash_ prior to 2.0.

[14]Versions of _bash_ prior to 2.04 do not have this type of

loop.


# Arrays

The _pushd_ and _popd_ functions use a string variable to
hold a list of directories and manipulate the list with the
string pattern-matching operators. Although this is quite
efficient for adding or retrieving items at the beginning or
end of the string, it becomes cumbersome when
attempting to access items that are anywhere else, e.g.,
obtaining item _N_ with the _getNdirs_ function. It would be
nice to be able to specify the number, or _index_ , of the
item and retrieve it. Arrays allow us to do this.[15]

An array is like a series of slots that hold values. Each
slot is known as an _element_ , and each element can be
accessed via a numerical index. An array element can
contain a string or a number, and you can use it just like
any other variable. The indices for arrays start at 0 and
continue up to a very large number.[16]So, for example,
the fifth element of array **names** would be **names[4]**.
Indices can be any valid arithmetic expression that
evaluates to a number greater than or equal to 0.

There are several ways to assign values to arrays. The
most straightforward way is with an assignment, just like
any other variable:

```
names[2]=alice
names[0]=hatter
names[1]=duchess
```

This assigns **hatter** to element **0** , **duchess** to element **1** ,
and **alice** to element **2** of the array **names**.

Another way to assign values is with a compound
assignment:

```
names=([2]=alice [0]=hatter [1]=duchess)
```
This is equivalent to the first example and is convenient
for initializing an array with a set of values. Notice that
we didn't have to specify the indices in numerical order.
In fact, we don't even have to supply the indices if we
reorder our values slightly:

```
names=(hatter duchess alice)
```
_bash_ automatically assigns the values to consecutive
elements starting at 0. If we provide an index at some
point in the compound assignment, the values get
assigned consecutively from that point on, so:

```
names=(hatter [5]=duchess alice)
```
assigns **hatter** to element **0** , **duchess** to element **5** , and
**alice** to element **6**.

An array is created automatically by any assignment of
these forms. To explicitly create an empty array, you can
use the **-a** option to **declare**. Any attributes that you set
for the array with **declare** (e.g., the read-only attribute)
apply to the entire array. For example, the statement
**declare -ar names** would create a read-only array called
**names**. Every element of the array would be read-only.


An element in an array may be referenced with the syntax
**${** _array_ [ _i_ ]}. So, from our last example above, the
statement **echo ${names[5]}** would print the string
"duchess". If no index is supplied, array element 0 is
assumed.

You can also use the special indices **@** and *****. These
return all of the values in the array and work in the same
way as for the positional parameters; when the array
reference is within double quotes, using ***** expands the
reference to one word consisting of all the values in the
array separated by the first character of the **IFS** variable,
while **@** expands the values in the array to separate
words. When unquoted, both of them expand the values
of the array to separate words. Just as with positional
parameters, this is useful for iterating through the values
with a **for** loop:

```
for i in "${names[@]}"; do
echo $i
done
```
Any array elements which are unassigned don't exist; they
default to null strings if you explicitly reference them.
Therefore, the previous looping example will print out
only the assigned elements in the array **names**. If there
were three values at indexes 1, 45, and 1005, only those
three values would be printed.

If you want to know what indices currently have values in
an array then you can use _${!array[@]}_. In the last
example this would return 1 45 1005.[17]


A useful operator that you can use with arrays is **#** , the
length operator that we saw inChapter 4. To find out the
length of any element in the array, you can use
_${#array[i]}_. Similarly, to find out how many values
there are in the array, use ***** or **@** as the index. So, for
**names=(hatter [5]=duchess alice)** , **${#names[5]}** has
the value 7, and **${#names[@]}** has the value 3.

Reassigning to an existing array with a compound array
statement replaces the old array with the new one. All of
the old values are lost, even if they were at different
indices to the new elements. For example, if we
reassigned **names** to be **([100]=tweedledee
tweedledum)** , the values **hatter** , **duchess** , and **alice**
would disappear.

You can destroy any element or the entire array by using
the **unset** built-in. If you specify an index, that particular
element will be unset. **unset names[100]** , for instance,
would remove the value at index **100** ; **tweedledee** in the
example above. However, unlike assignment, if you don't
specify an index the _entire_ array is unset, not just element

0. You can explicitly specify unsetting the entire array by
using ***** or **@** as the index.

Let's now look at a simple example that uses arrays to
match user IDs to account names on the system. The code
takes a user ID as an argument and prints the name of the
account plus the number of accounts currently on the
system:


```
for i in $(cut -f 1,3 -d: /etc/passwd) ; do
array[${i#*:}]=${i%:*}
done
```
```
echo "User ID $1 is ${array[$1]}."
echo "There are currently ${#array[@]} user accounts on the system."
```
We use _cut_ to create a list from fields 1 and 3 in the _/etc/
passwd_ file. Field 1 is the account name and field 3 is the
user ID for the account. The script loops through this list
using the user ID as an index for each array element and
assigns each account name to that element. The script
then uses the supplied argument as an index into the
array, prints out the value at that index, and prints the
number of existing array values.

We'll now look at combining our knowledge of arrays
with arithmetic **for** loops in the next task:

```
Task 6-3
```
```
Write a selection sort script that takes numbers in an
array and sorts them.
```
Selection sort is a common algorithm for quickly sorting
a set of elements. While it isn't the quickest sorting
algorithm available, it is easy to understand and
implement.


It works by selecting the smallest element in the set and
moving it to the head of the set. It then repeats the
process for the remainder of the set until the end of the set
is reached.

For example, to sort the set 21543 it would start at 2 and
then move down the set. 1 is less than 2 (and the other
elements) so 1 is moved to the start: 12543. Then looking
at 2 and moving down the list it finds nothing less than 2
so it moves to the next element, 5. Moving down the list 4
is less than 5, but 3 is less than 4, so 3 is moved: 12354.
The next element is 5, and 4 is less than this so 4 is
moved: 12345. Five is the last element so the sort is
finished.

The code for this is as follows:

```
values=(39 5 36 12 9 3 2 30 4 18 22 1 28 25)
numvalues=${#values[@]}
for (( i=0; i < numvalues; i++ )); do
lowest=$i
for (( j=i; j < numvalues; j++ )); do
if [ ${values[j]} -le ${values[$lowest]}; then
lowest=$j
fi
done
temp=${values[i]}
values[i]=${values[lowest]}
values[lowest]=$temp
done
for (( i=0; i < numvalues; i++ )); do
echo -ne "${values[$i]}\t"
done
echo
```

At the start of the script we set up an array of randomly
ordered values and a variable to hold the number of array
elements as a convenience.

The outer _i_ **for** loop is for looping over the entire array
and pointing to the current "head" (where we put any
value we need to swap). The variable _lowest_ is set to this
index.

The inner _j_ loop is for looping over the remainder of the
array. It compares the remaining elements with the value
at _lowest_ ; if a value is less then _lowest_ is set to the index
of that element.

Once the inner loop is finished the values of the "head" ( _i_ )
element and _lowest_ are swapped by using a temporary
variable _temp_.

On completing the outer loop, the script prints out the
sorted array elements.

Note that some of the environment variables in _bash_ are
arrays; **DIRSTACK** functions as a stack for the _pushd_
and _popd_ built-ins, **BASH_VERSINFO** is an array of
version information for the current instance of the shell,
and **PIPESTATUS** is an array of exit status values for
the last foreground pipe that was executed.

We'll see a further use of arrays when we build a _bash_
debugger inChapter 9.


To end this chapter, here are some problems relating to
what we've just covered:

1. Improve the account ID script so that it checks
    whether the argument is a number. Also, add a
    test to print an appropriate message if the user ID
    doesn't exist.
2. Make the script print out the username (field 5)
    as well. Hint: this isn't as easy as it sounds. A
    username can have spaces in it, causing the **for**
    loop to iterate on each part of the name.
3. As mentioned earlier, the built-in versions of
    **pushd** and **popd** use an array to implement the
    stack. Change the _pushd_ , _popd_ , and _getNdirs_
    code that we developed in this chapter so that it
    uses arrays.
4. Change the selection sort in the last task into a
    _bubble sort_. A bubble sort works by iterating
    over the list comparing pairs of elements and
    swapping them if they are in incorrect order. It
    then repeats the process from the start of the list
    and continues until the list is traversed with no
    swaps.

[15]Support for arrays is not available in versions of _bash_

prior to 2.0.


[16] Actually, up to 599147937791. That's almost six

hundred billion, so yes, it's pretty large.

[17]This is not available in versions of _bash_ prior to 3.0.


# Chapter 7. Input/Output

# and Command-Line

# Processing

The past few chapters have gone into detail about various
shell programming techniques, mostly focused on the
flow of data and control through shell programs. In this
chapter, we switch the focus to two related topics. The
first is the shell's mechanisms for doing file-oriented
input and output. We present information that expands on
what you already know about the shell's basic I/O
redirectors.

Second, we'll "zoom in" and talk about I/O at the line and
word level. This is a fundamentally different topic, since
it involves moving information between the domains of
files/terminals and shell variables. **echo** and command
substitution are two ways of doing this that we've seen so
far.

Our discussion of line and word I/O will lead into a more
detailed explanation of how the shell processes command
lines. This information is necessary so that you can
understand exactly how the shell deals with _quotation_ ,
and so that you can appreciate the power of an advanced
command called **eval** , which we will cover at the end of
the chapter.


# I/O Redirectors

In Chapter 1, you learned about the shell's basic I/O
redirectors: **>** , **<** , and **|**. Although these are enough to get
you through 95% of your UNIX life, you should know
that _bash_ supports many other redirectors.Table 7-1lists
them, including the three we've already seen. Although
some of the rest are broadly useful, others are mainly for
systems programmers.

**Table 7-1. I/O redirectors**

```
Redirector Function
```
```
cmd1 |
cmd2
```
```
Pipe; take standard output of cmd1 as
standard input to cmd2.
```
```
> file Direct standard output to file.
```
```
< file Take standard input from file.
```
```
>> file
Direct standard output to file ; append to
file if it already exists.
```

**Redirector Function**

**>|** _file_
Force standard output to _file_ even if
**noclobber** is set.

_n_ >| _file_
Force output to _file_ from file descriptor _n_
even if **noclobber** is set.

**<>** _file_
Use _file_ as both standard input and
standard output.

_n_ <> _file_
Use _file_ as both input and output for file
descriptor _n_.

**<<** _label_ Here-document; see text.

_n_ **>** _file_ Direct file descriptor _n_ to _file_.

_n_ **<** _file_ Take file descriptor _n_ from _file_.


**Redirector Function**

_n_ **>>** _file_
Direct file descriptor _n_ to _file_ ; append to
_file_ if it already exists.

_n_ >&
Duplicate standard output to file descriptor
_n_.

_n_ <&
Duplicate standard input from file
descriptor _n_.

_n_ >& _m_
File descriptor _n_ is made to be a copy of
the output file descriptor.

_n_ <& _m_
File descriptor _n_ is made to be a copy of
the input file descriptor.

_&>file_
Directs standard output and standard error
to _file_.

<&- Close the standard input.


**Redirector Function**

>&- Close the standard output.

_n_ >&- Close the output from file descriptor _n_.

_n_ <&- Close the input from file descriptor _n_.

_n>&word_

```
If n is not specified, the standard output
(file descriptor 1) is used. If the digits in
word do not specify a file descriptor open
for output, a redirection error occurs. As a
special case, if n is omitted, and word does
not expand to one or more digits, the
standard output and standard error are
redirected as described previously.
```
_n<&word_

```
If word expands to one or more digits, the
file descriptor denoted by n is made to be
a copy of that file descriptor. If the digits
in word do not specify a file descriptor
open for input, a redirection error occurs.
If word evaluates to -, file descriptor n is
```

```
Redirector Function
```
```
closed. If n is not specified, the standard
input (file descriptor 0) is used.
```
```
n>&digit-
```
```
Moves the file descriptor digit to file
descriptor n , or the standard output (file
descriptor 1) if n is not specified.
```
```
n<&digit-
```
```
Moves the file descriptor digit to file
descriptor n , or the standard input (file
descriptor 0) if n is not specified. digit is
closed after being duplicated to n.
```
Notice that some of the redirectors inTable 7-1contain a
digit _n_ , and that their descriptions contain the term _file
descriptor_ ; we'll cover that in a little while.

The first two new redirectors, **>>** and **>|** , are simple
variations on the standard output redirector **>**. The **>>**
appends to the output file (instead of overwriting it) if it
already exists; otherwise it acts exactly like **>**. A common
use of **>>** is for adding a line to an initialization file (such
as _.bashrc_ or _.mailrc_ ) when you don't want to bother with
a text editor. For example:


```
$ cat >> .bashrc
alias cdmnt='mount -t iso9660 /dev/sbpcd /cdrom'
^D
```
As we saw inChapter 1, _cat_ without an argument uses
standard input as its input. This allows you to type the
input and end it with CTRL-D on its own line. The **alias**
line will be appended to the file _.bashrc_ if it already
exists; if it doesn't, the file is created with that one line.

Recall from Chapter 3, that you can prevent the shell
from overwriting a file with **>** _file_ by typing **set -o
noclobber**. **>|** overrides **noclobber—** it's the "Do it
anyway, dammit!" redirector.

The redirector **<>** is mainly meant for use with device
files (in the _/dev_ directory), i.e., files that correspond to
hardware devices such as terminals and communication
lines. Low-level systems programmers can use it to test
device drivers; otherwise, it's not very useful.

The rest of the redirectors will only be useful in special
situations and you are unlikely to need them most of the
time.

## Here-documents

The **<<** _label_ redirector essentially forces the input to a
command to be the shell's standard input, which is read
until there is a line that contains only _label_. The input in
between is called a _here-document_. Here-documents


aren't very interesting when used from the command
prompt. In fact, it's the same as the normal use of
standard input except for the label. We could use a
here-document to simulate the _mail_ facility. When you
send a message to someone with the _mail_ utility, you end
the message with a dot (.). The body of the message is
saved in a file, _msgfile_ :

```
$ cat >> msgfile <<.
> this is the text of
> our message.
>.
```
Here-documents are meant to be used from within shell
scripts; they let you specify "batch" input to programs. A
common use of here-documents is with simple text
editors like _ed_. Task 7-1 is a programming task that uses a
here-document in this way.

```
Task 7-1
```
```
The s file command in mail saves the current message in
file. If the message came over a network (such as the
Internet), then it has several header lines prepended that
give information about network routing. Write a shell
script that deletes the header lines from the file.
```
We can use _ed_ to delete the header lines. To do this, we
need to know something about the syntax of mail
messages; specifically, that there is always a blank line


between the header lines and the message text. The _ed_
command **1,/^[]*** $/d does the trick: it means, "Delete
from line 1 until the first blank line." We also need the _ed_
commands **w** (write the changed file) and **q** (quit). Here is
the code that solves the task:

```
ed $1 << EOF
1,/^[ ]*$/d
w
q
EOF
```
The shell does parameter (variable) substitution and
command substitution on text in a here-document,
meaning that you can use shell variables and commands
to customize the text. A good example of this is the
_bashbug_ script, which sends a bug report to the _bash_
maintainer (see Chapter 11). Here is a stripped-down
version:

```
MACHINE="i586"
OS="linux-gnu"
CC="gcc"
CFLAGS=" -DPROGRAM='bash' -DHOSTTYPE='i586' -DOSTYPE='linux-gnu' \
-DMACHTYPE='i586-pc-linux-gnu' -DSHELL -DHAVE_CONFIG_H -I. \
-I. -I./lib -g -O2"
RELEASE="2.01"
PATCHLEVEL="0"
RELSTATUS="release"
MACHTYPE="i586-pc-linux-gnu"
```
```
TEMP=/tmp/bbug.$$
```
```
case "$RELSTATUS" in
alpha*|beta*) BUGBASH=chet@po.cwru.edu ;;
```

*) BUGBASH=bug-bash@prep.ai.mit.edu ;;
esac

BUGADDR="${1-$BUGBASH}"

UN=
if (uname) >/dev/null 2>&1; then
UN=`uname -a`
fi

cat > $TEMP <<EOF
From: ${USER}
To: ${BUGADDR}
Subject: [50 character or so descriptive subject here (for reference)]

Configuration Information [Automatically generated, do not change]:
Machine: $MACHINE
OS: $OS
Compiler: $CC
Compilation CFLAGS: $CFLAGS
uname output: $UN
Machine Type: $MACHTYPE

bash Version: $RELEASE
Patch Level: $PATCHLEVEL
Release Status: $RELSTATUS

Description:
[Detailed description of the problem, suggestion, or complaint.]

Repeat-By:
[Describe the sequence of events that causes the problem
to occur.]

Fix:
[Description of how to fix the problem. If you don't know a
fix for the problem, don't include this section.]


```
EOF
```
```
vi $TEMP
```
```
mail $BUGADDR < $TEMP
```
The first eight lines are generated when _bashbug_ is
installed. The shell will then substitute the appropriate
values for the variables in the text whenever the script is
run.

The redirector **<<** has two variations. First, you can
prevent the shell from doing parameter and command
substitution by surrounding the _label_ in single or double
quotes. In the above example, if you used the line **cat >
$TEMP <<** ` **EOF** ', then text like **$USER** and
**$MACHINE** would remain untouched (defeating the
purpose of this particular script).

The second variation is **<<-** , which deletes leading TABs
(but not blanks) from the here-document and the label
line. This allows you to indent the here-document's text,
making the shell script more readable:

```
cat > $TEMP <<-EOF
From: ${USER}
To: ${BUGADDR}
Subject: [50 character or so descriptive subject here]
```
```
Configuration Information [Automatically generated,
do not change]:
Machine: $MACHINE
OS: $OS
Compiler: $CC
```

```
Compilation CFLAGS: $CFLAGS
...
EOF
```
Make sure you are careful when choosing your _label_ so
that it doesn't appear as an actual input line.

A slight variation on this is provided by the _here string_. It
takes the form _<<<word_ ; the word is expanded and
supplied on the standard input.


## File Descriptors

The next few redirectors in Table 7-1 depend on the
notion of a _file descriptor_. Like the device files used with
**<>** , this is a low-level UNIX I/O concept that is of
interest only to systems programmers—and then only
occasionally. You can get by with a few basic facts about
them; for the whole story, look at the entries for _read_ ( ),
_write_ ( ), _fcntl_ ( ), and others in Section 2 of the UNIX
manual. You might wish to refer to _UNIX Power Tools_ by
Shelley Powers, Jerry Peek, Tim O'Reilly, and Mike
Loukides (O'Reilly).

File descriptors are integers starting at 0 that refer to
particular streams of data associated with a process.
When a process starts, it usually has three file descriptors
open. These correspond to the three _standards_ : standard
input (file descriptor 0), standard output (1), and standard
error (2). If a process opens additional files for input or
output, they are assigned to the next available file
descriptors, starting with 3.

By far the most common use of file descriptors with _bash_
is in saving standard error in a file. For example, if you
want to save the error messages from a long job in a file
so that they don't scroll off the screen, append **2>** _file_ to
your command. If you also want to save standard output,
append **>** _file1_ **2>** _file2_.


This leads to another programming task.

```
Task 7-2
```
```
You want to start a long job in the background (so that
your terminal is freed up) and save both standard output
and standard error in a single log file. Write a script that
does this.
```
We'll call this script _start_. The code is very terse:

```
"$@" > logfile 2>&1 &
```
This line executes whatever command and parameters
follow **start**. (The command cannot contain pipes or
output redirectors.) It sends the command's standard
output to _logfile_.

Then, the redirector **2>&1** says, "send standard error (file
descriptor 2) to the same place as standard output (file
descriptor 1)." Since standard output is redirected to
_logfile_ , standard error will go there too. The final **&** puts
the job in the background so that you get your shell
prompt back.

As a small variation on this theme, we can send both
standard output and standard error into a _pipe_ instead of a
file: _command_ **2>&** 1 | ... does this. (Make sure you
understand why.) Here is a script that sends both standard


output and standard error to the logfile (as above) and to
the terminal:

```
"$@" 2>&1 | tee logfile &
```
The command _tee_ takes its standard input and copies it to
standard output _and_ the file given as argument.

These scripts have one shortcoming: you must remain
logged in until the job completes. Although you can
always type **jobs** (seeChapter 1) to check on progress,
you can't leave your terminal until the job finishes, unless
you want to risk a breach of security.[1]We'll see how to
solve this problem in the next chapter.

The other file-descriptor-oriented redirectors (e.g., **<&** _n_ )
are usually used for reading input from (or writing output
to) more than one file at the same time. We'll see an
example later in this chapter. Otherwise, they're mainly
meant for systems programmers, as are **<&-** (force
standard input to close) and **>&-** (force standard output to
close).

Before we leave this topic, we should just note that **1>** is
the same as **>** , and **0<** is the same as **<**. If you understand
this, then you probably know all you need to know about
file descriptors.

[1]Don't put it past people to come up to your unattended

terminal and cause mischief!


# String I/O

Now we'll zoom back in to the string I/O level and
examine the **echo** and **read** statements, which give the
shell I/O capabilities that are more analogous to those of
conventional programming languages.

## echo

As we've seen countless times in this book, **echo** simply
prints its arguments to standard output. Now we'll explore
the command in greater detail.

## Options to echo

**echo** accepts a few dash options, listed inTable 7-2.

**Table 7-2. echo options**

```
Option Function
```
```
-e
```
```
Turns on the interpretation of
backslash-escaped characters
```

**Option Function**

#### -E

```
Turns off the interpretation of
backslash-escaped characters on systems
where this mode is the default
```
-n
Omits the final newline (same as the **\c** escape
sequence)


### echo escape sequences

**echo** accepts a number of _escape sequences_ that start with
a backslash.[2]They are listed inTable 7-3.

These sequences exhibit fairly predictable behavior,
except for **\f** : on some displays, it causes a screen clear,
while on others it causes a line feed. It ejects the page on
most printers. **\v** is somewhat obsolete; it usually causes a
line feed.

**Table 7-3. echo escape sequences**

```
Sequence Character printed
```
```
\a ALERT or CTRL-G (bell)
```
```
\b BACKSPACE or CTRL-H
```
```
\c Omit final NEWLINE
```
```
\e Escape character (same as \E)
```

**Sequence Character printed**

\E Escape character[3]

\f FORMFEED or CTRL-L

\n

```
NEWLINE (not at end of command) or
CTRL-J
```
\r RETURN (ENTER) or CTRL-M

\t TAB or CTRL-I

\v VERTICAL TAB or CTRL-K

**\** _n_
ASCII character with octal (base-8) value _n_ ,
where _n_ is 1 to 3 digits

\0 _nnn_

```
The eight-bit character whose value is the
octal (base-8) value nnn where nnn is 1 to 3
digits
```

```
Sequence Character printed
```
```
\x HH
```
```
The eight-bit character whose value is the
hexadecimal (base-16) value HH (one or
two digits)
```
```
\\ Single backslash
```
```
[3]Not available in versions of bash prior to 2.0.
```
The **\n** , **\0** , and **\x** sequences are even more
device-dependent and can be used for complex I/O, such
as cursor control and special graphics characters.


## printf

_bash_ 's **echo** command is quite powerful and for most
cases entirely adequate. However, there are occasions
where a more powerful and flexible approach is needed
for printing information, especially when the information
needs to be formatted. _bash_ provides this by giving access
to a powerful system-level printing library known as
_printf_ .[4]

The **printf** command can output a string similar to the
**echo** command:

```
printf "hello world"
```
Unlike the **echo** command, **printf** does not automatically
provide a newline. If we want to make it do the exactly
same as a standard **echo** then we must provide one by
adding \n to the end:

```
printf "hello world\n"
```
You may ask why this is any better than **echo**. The **printf**
command has two parts, which is what makes it so
powerful.

```
printf format-string [arguments]
```
The first part is a string that describes the format
specifications; this is best supplied as a string constant in


quotes. The second part is an argument list, such as a list
of strings or variable values that correspond to the format
specifications. (The format is reused as necessary to use
up all of the arguments. If the format requires more
arguments than are supplied, the extra format
specifications behave as if a zero value or null string, as
appropriate, had been supplied). A format specification is
preceded by a percent sign (%), and the specifier is one of
the characters described below. Two of the main format
specifiers are %s for strings and %d for decimal integers.

This sounds complicated but we can begin by re-casting
the last example:

```
printf "%s %s\n" hello world
```
This prints _hello world_ on a line of its own, just as the
previous example did. The word _hello_ has been assigned
to the first format specification, _%s_. Likewise, _world_ has
been assigned to the second _%s_. **printf** then prints these
two strings followed by the newline.

We could also achieve the same result by making _hello_ an
explicit part of the format string:

```
$ printf "hello %s\n" world
hello world
```
The allowed specifiers are shown inTable 7-4.

**Table 7-4. printf format specifiers**


**Specifier Description**

%c
ASCII character (prints first character of
corresponding argument)

%d Decimal integer

%i Same as %d

%e
Floating-point format ([-] _d_. _precision_ e[+-] _dd_ )
(see following text for meaning of _precision_ )

#### %E

```
Floating-point format
([-] d.precision E[+-] dd )
```
%f Floating-point format ([-] _ddd.precision_ )

%g
%e or %f conversion, whichever is shorter,
with trailing zeros removed


```
Specifier Description
```
#### %G

```
%E or %f conversion, whichever is shortest,
with trailing zeros removed
```
```
%o Unsigned octal value
```
```
%s String
```
```
%u Unsigned decimal value
```
```
%x
Unsigned hexadecimal number; uses a-f for
10 to 15
```
#### %X

```
Unsigned hexadecimal number; uses A-F for
10 to 15
```
```
%% Literal %
```
The **printf** command can be used to specify the width and
alignment of output fields. A format expression can take


three optional modifiers following % and preceding the
format specifier:

```
%flags width.precision format-specifier
```
The width of the output field is a numeric value. When
you specify a field width, the contents of the field are
right-justified by default. You must specify a flag of "-"
to get left-justification. (The rest of the flags are
discussed shortly.) Thus, "%-20s" outputs a left-justified
string in a field 20 characters wide. If the string is less
than 20 characters, the field is padded with whitespace to
fill. In the following examples, a | is output to indicate the
actual width of the field. The first example right-justifies
the text:

```
printf "|%10s|\n" hello
```
It produces:

```
| hello|
```
The next example left-justifies the text:

```
printf "|%-10s|\n" hello
```
It produces:

```
|hello |
```
The precision modifier, used for decimal or floating-point
values, controls the number of digits that appear in the
result. For string values, it controls the maximum number
of characters from the string that will be printed.


You can specify both the width and precision
dynamically, via values in the **printf** argument list. You
do this by specifying asterisks, instead of literal values.

```
$ myvar=42.123456
$ printf "|%*.*G|\n" 5 6 $myvar
|42.1235|
```
In this example, the width is 5, the precision is 6, and the
value to print comes from the value of **myvar**.

The _precision_ is optional. Its exact meaning varies by
control letter, as shown inTable 7-5.

**Table 7-5. Meaning of precision**

```
Conversion Precision means
```
```
%d, %I,
%o, %u,
%x, %X
```
```
The minimum number of digits to print.
When the value has fewer digits, it is
padded with leading zeros. The default
precision is 1.
```
```
%e, %E
```
```
The minimum number of digits to print.
When the value has fewer digits, it is
padded with zeros after the decimal point.
The default precision is 10. A precision of
0 inhibits printing of the decimal point.
```

```
Conversion Precision means
```
```
%f
The number of digits to the right of the
decimal point.
```
```
%g, %G
The maximum number of significant
digits.
```
```
%s
The maximum number of characters to
print.
```
Finally, one or more _flags_ may precede the field width
and the precision. We've already seen the "-" flag for
left-justification. The rest of the flags are shown inTable
7-6.

**Table 7-6. Flags for printf**

```
Character Description
```
#### -

```
Left-justify the formatted value within the
field.
```

```
Character Description
```
```
space
Prefix positive values with a space and
negative values with a minus.
```
#### +

```
Always prefix numeric values with a sign,
even if the value is positive.
```
#### #

```
Use an alternate form: %o has a preceding
0; %x and %X are prefixed with 0x and
0X, respectively; %e, %E and %f always
have a decimal point in the result; and %g
and %G do not have trailing zeros
removed.
```
#### 0

```
Pad output with zeros, not spaces. This
only happens when the field width is wider
than the converted result. In the C
language, this flag applies to all output
formats, even non-numeric ones. For bash ,
it only applies to the numeric formats.
```
If **printf** cannot perform a format conversion, it returns a
non-zero exit status.


### Additional bash printf specifiers

Besides the standard specifiers just described, the _bash_
shell (and other POSIX compliant shells) accepts two
additional specifiers. These provide useful features at the
expense of nonportability to versions of the **printf**
command found in some other shells and in other places
in UNIX:

**%b**

```
When used instead of %s, expands echo -style escape
sequences in the argument string. For example:
```
```
$ printf "%s\n" 'hello\nworld'
hello\nworld
$ printf "%b\n" 'hello\nworld'
hello
world
```
**%q**

```
When used instead of %s, prints the string argument
in such a way that it can be used for shell input. For
example:
```
```
$ printf "%q\n" "greetings to the world"
greetings\ to\ the\ world
```

## read

The other half of the shell's string I/O facilities is the
**read** command, which allows you to read values _into_
shell variables. The basic syntax is:

```
read var1 var2...
```
This statement takes a line from the standard input and
breaks it down into words delimited by any of the
characters in the value of the environment variable **IFS**
(see Chapter 4; these are usually a space, a TAB, and
NEWLINE). The words are assigned to variables _var1_ ,
_var2_ , etc. For example:

```
$ read character1 character2alice duchess $ echo $character1 alice
$ echo $character2 duchess
```
If there are more words than variables, then excess words
are assigned to the last variable. If you omit the variables
altogether, the entire line of input is assigned to the
variable **REPLY**.

You may have identified this as the "missing ingredient"
in the shell programming capabilities we have seen thus
far. It resembles input statements in conventional
languages, like its namesake in Pascal. So why did we
wait this long to introduce it?


Actually, **read** is sort of an "escape hatch" from
traditional shell programming philosophy, which dictates
that the most important unit of data to process is a _text
file_ , and that UNIX utilities such as _cut_ , _grep_ , _sort_ , etc.,
should be used as building blocks for writing programs.

**read** , on the other hand, implies line-by-line processing.
You could use it to write a shell script that does what a
pipeline of utilities would normally do, but such a script
would inevitably look like:

```
while (read a line) do
process the line
print the processed line
end
```
This type of script is usually much slower than a pipeline;
furthermore, it has the same form as a program someone
might write in C (or some similar language) that does the
same thing _much_ faster. In other words, if you are going
to write it in this line-by-line way, there is little point in
writing a shell script.

### Reading lines from files

Nevertheless, shell scripts with **read** are useful for certain
kinds of tasks. One is when you are reading data from a
file small enough so that efficiency isn't a concern (say a
few hundred lines or less), and it's _really necessary_ to get
bits of input into shell variables.


Consider the case of a UNIX machine that has terminals
that are hardwired to the terminal lines of the machine. It
would be nice if the TERM environment variable was set
to the correct terminal type when a user logged in.

One way to do this would be to have some code that sets
the terminal information when a user logs in. This code
would presumably reside in _/etc/profile_ , the system-wide
initialization file that _bash_ runs before running a user's
_.bash_profile_. If the terminals on the system change over
time—as surely they must—then the code would have to
be changed. It would be better to store the information in
a file and change just the file instead.

Assume we put the information in a file whose format is
typical of such UNIX "system configuration" files: each
line contains a device name, a TAB, and a **TERM** value.

We'll call the file _/etc/terms_ , and it would typically look
something like this:

```
console console
tty01 wy60
tty03 vt100
tty04 vt100
tty07 wy85
tty08 vt100
```
The values on the left are terminal lines and those on the
right are the terminal types that **TERM** can be set to. The
terminals connected to this system are a Wyse 60 (wy60),
three VT100s (vt100), and a Wyse 85 (wy85). The


machines' master terminal is the console, which has a
**TERM** value of **console**.

We can use **read** to get the data from this file, but first we
need to know how to test for the end-of-file condition.
Simple: **read** 's exit status is 1 (i.e., non-zero) when there
is nothing to read. This leads to a clean **while** loop:

```
TERM=vt100 # assume this as a default
line=$(tty)
while read dev termtype; do
if [ $dev = $line ]; then
TERM=$termtype
echo "TERM set to $TERM."
break
fi
done
```
The **while** loop reads each line of the input into the
variables **dev** and **termtype**. In each pass through the
loop, the **if** looks for a match between **$dev** and the user's
tty ( **$line** , obtained by command substitution from the _tty_
command). If a match is found, **TERM** is set, a message
is printed, and the loop exits; otherwise **TERM** remains
at the default setting of **vt100**.

We are not quite done, though: this code reads from the
standard input, not from _/etc/terms_! We need to know
how to redirect input to _multiple commands_. It turns out
that there are a few ways of doing this.


### I/O redirection and multiple

### commands

One way to solve the problem is with a _subshell_ , as we'll
see in the next chapter. This involves creating a separate
process to do the reading. However, it is usually more
efficient to do it in the same process; _bash_ gives us four
ways of doing this.

The first, which we have seen already, is with a function:

```
findterm ( ) {
TERM=vt100 # assume this as a default
line=$(tty)
while read dev termtype; do
if [ $dev = $line ]; then
TERM=$termtype
echo "TERM set to $TERM."
break;
fi
done
}
```
```
findterm < /etc/terms
```
A function acts like a script in that it has its own set of
standard I/O descriptors, which can be redirected in the
line of code that calls the function. In other words, you
can think of this code as if _findterm_ were a script and you
typed **findterm < /etc/terms** on the command line. The


**read** statement takes input from _/etc/terms_ a line at a
time, and the function runs correctly.

The second way is to simplify this slightly by placing the
redirection at the end of the function:

```
findterm ( ) {
TERM=vt100 # assume this as a default
line=$(tty)
while read dev termtype; do
if [ $dev = $line ]; then
TERM=$termtype
echo "TERM set to $TERM."
break;
fi
done
} < /etc/terms
```
Whenever _findterm_ is called, it takes its input from _/etc/
terms_.

The third way is by putting the I/O redirector at the end of
the loop, like this:

```
TERM=vt100 # assume this as a default
line=$(tty)
while read dev termtype; do
if [ $dev = $line ]; then
TERM=$termtype
echo "TERM set to $TERM."
break;
fi
done < /etc/terms
```
You can use this technique with any flow-control
construct, including **if** ... **fi** , **case** ... **esac** , **select** ... **done** , and


**until** ... **done**. This makes sense because these are all
_compound statements_ that the shell treats as single
commands for these purposes. This technique works
fine—the **read** command reads a line at a time—as long
as all of the input is done within the compound statement.


### Command blocks

But if you want to redirect I/O to or from an arbitrary
group of commands without creating a separate process,
you need to use a construct that we haven't seen yet. If
you surround some code with **{** and **}** , the code will
behave like a function that has no name. This is another
type of compound statement. In accordance with the
equivalent concept in the C language, we'll call this a
_command block_.

What good is a block? In this case, it means that the code
within the curly brackets ( **{}** ) will take standard I/O
descriptors just as we described in the last block of code.
This construct is appropriate for the current example
because the code needs to be called only once, and the
entire script is not really large enough to merit breaking
down into functions. Here is how we use a block in the
example:

```
{
TERM=vt100 # assume this as a default
line=$(tty)
while read dev termtype; do
if [ $dev = $line ]; then
TERM=$termtype
echo "TERM set to $TERM."
break;
fi
done
} < /etc/terms
```

To help you understand how this works, think of the curly
brackets and the code inside them as if they were one
command, i.e.:

```
{ TERM=vt100; line=$(tty); while ... } < /etc/terms;
```
Configuration files for system administration tasks like
this one are actually fairly common; a prominent example
is _/etc/hosts_ , which lists machines that are accessible in a
TCP/IP network. We can make _/etc/terms_ more like these
standard files by allowing comment lines in the file that
start with **#** , just as in shell scripts. This way _/etc/terms_
can look like this:

```
#
# System Console is console
console console
#
# Cameron's line has a Wyse 60
tty01 wy60
...
```
We can handle comment lines by modifying the **while**
loop so that it ignores lines beginning with **#**. We can
place a _grep_ in the test:

```
if [ -z "$(echo $dev | grep ^#)" ] && [ $dev = $line ]; then
...
```
As we saw in Chapter 5, the **&&** combines the two
conditions so that _both_ must be true for the entire
condition to be true.


As another example of command blocks, consider the
case of creating a standard algebraic notation frontend to
the _dc_ command. _dc_ is a UNIX utility that simulates a
Reverse Polish Notation (RPN) calculator:[5]

```
{ while read line; do
echo "$(alg2rpn $line)"
done
} | dc
```
We'll assume that the actual conversion from one notation
to the other is handled by a function called _alg2rpn_. It
takes a line of standard algebraic notation as an argument
and prints the RPN equivalent on the standard output. The
**while** loop reads lines and passes them through the
conversion function, until an EOF is typed. Everything is
executed inside the command block and the output is
piped to the _dc_ command for evaluation.


### Reading user input

The other type of task to which **read** is suited is
prompting a user for input. Think about it: we have
hardly seen any such scripts so far in this book. In fact,
the only ones were the modified solutions to Task 5-4,
which involved **select**.

As you've probably figured out, **read** can be used to get
user input into shell variables.

We can use **echo** to prompt the user, like this:

```
echo -n 'terminal? '
read TERM
echo "TERM is $TERM"
```
Here is what this looks like when it runs:

```
terminal? wy60 TERM is wy60
```
However, shell convention dictates that prompts should
go to standard _error_ , not standard output. (Recall that
**select** prompts to standard error.) We could just use file
descriptor 2 with the output redirector we saw earlier in
this chapter:

```
echo -n 'terminal? ' >&2
read TERM
echo TERM is $TERM
```

We'll now look at a more complex example by showing
how Task 5-5 would be done if **select** didn't exist.
Compare this with the code inChapter 5:

```
echo 'Select a directory:'
done=false
```
```
while [ $done = false ]; do
do=true
num=1
for direc in $DIR_STACK; do
echo $num) $direc
num=$((num+1))
done
echo -n 'directory? '
read REPLY
```
```
if [ $REPLY -lt $num ] && [ $REPLY -gt 0 ]; then
set - $DIR_STACK
```
```
# statements that manipulate the stack...
```
```
break
else
echo 'invalid selection.'
fi
done
```
The **while** loop is necessary so that the code repeats if the
user makes an invalid choice. **select** includes the ability to
construct multicolumn menus if there are many choices,
and better handling of null user input.

Before leaving **read** , we should note that it has eight
options: **-a** , **-d** , **-e** , **-n** , **-p** , **-r** , **-t** , and **-s** .[6]The first of
these options allows you to read values into an array.


Each successive item read in is assigned to the given
array starting at index 0. For example:

```
$ read -a people
alice duchess dodo
$ echo ${people[2]}
dodo
$
```
In this case, the array **people** now contains the items
_alice_ , _duchess_ , and _dodo_.

A delimiter can be specified with the - **d** option. This will
read a line up until the _first_ character of the delimiter is
reached. For example:

```
$ read -s stop aline
alice duches$
$ echo $aline
alice duche
$
```
The option **-e** can be used only with scripts run from
interactive shells. It causes _readline_ to be used to gather
the input line, which means that you can use any of the
_readline_ editing features that we looked at inChapter 2.

The - **n** option specifies how many characters will be read
by **read**. For example, if we specify that it should read
only ten characters in then it will return after reading that
many:

```
$ read -n 10 aline
abcdefghij$
```

```
$ echo $aline
abcdefghij
$
```
The **-p** option followed by a string argument prints the
string before reading input. We could have used this in
the earlier examples of **read** , where we printed out a
prompt before doing the read. For example, the directory
selection script could have used **read -p `directory?'
REPLY**.

**read** lets you input lines that are longer than the width of
your display by providing a backslash (\) as a
continuation character, just as in shell scripts. The **-r**
option overrides this, in case your script reads from a file
that may contain lines that happen to end in backslashes.
**read -r** also preserves any other escape sequences the
input might contain. For example, if the file _hatter_
contains this line:

```
A line with a\n escape sequence
```
Then **read -r aline** will include the backslash in the
variable **aline** , whereas without the **-r** , **read** will "eat" the
backslash. As a result:

```
$ read -r aline < hatter $ echo -e "$aline"
A line with a
escape sequence
$
```
However:


```
$ read aline < hatter $ echo -e "$aline"
A line with an escape sequence
$
```
The - **s** option forces read to not echo the characters that
are typed to the terminal. This can be useful in cases
where a shell may want to take single keystroke
commands without displaying the typed characters on the
terminal (e.g., moving something around with the arrow
keys). In this case it could be combined with the - **n** option
to read a single character each time in a loop: **read -s -n1
key**

The last option, - **t** , allows a time in seconds to be
specified. **read** will wait the specified time for input and
then finish. This is useful if you want a script to wait for
input but continue processing if nothing is supplied.

[2]You must use a double backslash if you don't surround

the string that contains them with quotes; otherwise, the
shell itself "steals" a backslash before passing the
arguments to **echo**.

[4] **printf** is not available in versions of _bash_ prior to

version 2.02.

[5]If you have ever owned a Hewlett-Packard calculator

you will be familiar with RPN. We'll discuss RPN further
in one of the exercises at the end of this chapter.


[6] **-a** , **-d** , **-e** , **-n** , - **p, -t** and **-s** are not available in versions

of _bash_ prior to 2.0.


# Command-Line

# Processing

We've seen how the shell uses **read** to process input lines:
it deals with single quotes (`'), double quotes (""), and
backslashes ( **\** ); it separates lines into words, according to
delimiters in the environment variable **IFS** ; and it assigns
the words to shell variables. We can think of this process
as a subset of the things the shell does when processing
_command lines_.

We've touched upon command-line processing
throughout this book; now is a good time to make the
whole thing explicit. Each line that the shell reads from
the standard input or a script is called a _pipeline_ ; it
contains one or more _commands_ separated by zero or
more pipe characters ( **|** ). For each pipeline it reads, the
shell breaks it up into commands, sets up the I/O for the
pipeline, then does the following for each command
(Figure 7-1):



**Figure 7-1. Steps in command-line
processing**

1. Splits the command into tokens that are separated
    by the fixed set of metacharacters: SPACE, TAB,
    NEWLINE, **;** , **(** , **)** , **<** , **>** , **|** , and **&**. Types of tokens
    include words, keywords, I/O redirectors, and
    semicolons.
2. Checks the first token of each command to see if
    it is a keyword with no quotes or backslashes. If
    it's an opening keyword, such as **if** and other
    control-structure openers, **function** , **{** , or **(** , then
    the command is actually a compound command.
    The shell sets things up internally for the
    compound command, reads the next command,
    and starts the process again. If the keyword isn't a
    compound command opener (e.g., is a
    control-structure "middle" like **then** , **else** , or **do** ,
    an "end" like **fi** or **done** , or a logical operator),
    the shell signals a syntax error.
3. Checks the first word of each command against
    the list of aliases. If a match is found, it
    substitutes the alias's definition and goes back to
    Step 1; otherwise, it goes on to Step 4. This
    scheme allows recursive aliases (seeChapter 3).


```
It also allows aliases for keywords to be defined,
e.g., alias aslongas=while or alias
procedure=function.
```
4. Performs brace expansion. For example, **a{b,c}**
    becomes **ab ac**.
5. Substitutes the user's home directory ( **$HOME** )
    for tilde if it is at the beginning of a word.
    Substitutes user's home directory for **~** user.[7]
6. Performs parameter (variable) substitution for
    any expression that starts with a dollar sign ( **$** ).
7. Does command substitution for any expression of
    the form **$(** string **)**.
8. Evaluates arithmetic expressions of the form
    **$((** string **))**.
9. Takes the parts of the line that resulted from
    parameter, command, and arithmetic substitution
    and splits them into words again. This time it
    uses the characters in **$IFS** as delimiters instead
    of the set of metacharacters in Step 1.
10. Performs pathname expansion, a.k.a. wildcard
expansion, for any occurrences of ***** ,**?** , and **[/]**
pairs.
11. Uses the first word as a command by looking up
its source according to the rest of the list in


```
Chapter 4, i.e., as a function command, then as a
built-in, then as a file in any of the directories in
$PATH.
```
12. Runs the command after setting up I/O
    redirection and other such things.

That's a lot of steps—and it's not even the whole story!
But before we go on, an example should make this
process clearer. Assume that the following command has
been run:

```
alias ll="ls -l"
```
Further assume that a file exists called _.hist537_ in user
**alice** 's home directory, which is _/home/alice_ , and that
there is a double-dollar-sign variable **$$** whose value is
**2537** (we'll see what this special variable is in the next
chapter).

Now let's see how the shell processes the following
command:

```
ll $(type -path cc) ~alice/.*$(($$%1000))
```
Here is what happens to this line:

1. ll $(type -path cc) ~alice/
    .*$(($$%1000))splits the input into words.
2. llis not a keyword, so Step 2 does nothing.


3. ls -l $(type -path cc) ~alice/
    .*$(($$%1000))substitutes **ls -l** for its alias
    "ll". The shell then repeats Steps 1 through 3;
    Step 2 splits the **ls -l** into two words.
4. ls -l $(type -path cc) ~alice/
    .*$(($$%1000))does nothing.
5. ls -l $(type -path cc) /home/alice/
    .*$(($$%1000)) expands **~alice** into
    /home/alice.
6. ls -l $(type -path cc) /home/alice/
    .*$((2537%1000))substitutes **2537** for **$$**.
7. ls -l /usr/bin/cc /home/alice/
    .*$((2537%1000)) does command
    substitution on "type -path cc".
8. ls -l /usr/bin/cc /home/alice/.*537
    evaluates the arithmetic expression **2537%1000**.
9. ls -l /usr/bin/cc /home/alice/.*537
    does nothing.
10. ls -l /usr/bin/cc /home/alice/

```
.hist537 substitutes the filename for the
wildcard expression.*537.
```
11. The command **ls** is found in/usr/bin.


12. /usr/bin/ls is run with the option-l and
    the two arguments.

Although this list of steps is fairly straightforward, it is
not the whole story. There are still five ways to _modify_
the process: quoting; using **command** , **builtin** , or **enable** ;
and using the advanced command **eval**.

## Quoting

You can think of quoting as a way of getting the shell to
skip some of the 12 steps above. In particular:

- Single quotes (`') bypass everything through Step
    10—including aliasing. All characters inside a
    pair of single quotes are untouched. You can't
    have single quotes inside single quotes—not even
    if you precede them with backslashes.[8]
- Double quotes ("") bypass Steps 1 through 4, plus
    steps 9 and 10. That is, they ignore pipe
    characters, aliases, tilde substitution, wildcard
    expansion, and splitting into words via delimiters
    (e.g., blanks) inside the double quotes. Single
    quotes inside double quotes have no effect. But
    double quotes do allow parameter substitution,
    command substitution, and arithmetic expression
    evaluation. You can include a double quote
    inside a double-quoted string by preceding it with
    a backslash ( **\** ). You must also backslash-escape


```
$ , ` (the archaic command substitution delimiter),
and \ itself.
```
Table 7-7has simple examples to show how these work;
they assume the statement **person=hatter** was run and
user **alice** 's home directory is _/home/alice_.

If you are wondering whether to use single or double
quotes in a particular shell programming situation, it is
safest to use single quotes unless you specifically need
parameter, command, or arithmetic substitution.

**Table 7-7. Examples of quoting rules**

```
Expression Value
```
```
$person hatter
```
```
"$person" hatter
```
```
\$person $person
```
```
`$person' $person
```

**Expression Value**

"'$person'" 'hatter'

~alice /home/alice

"~alice" ~alice

`~alice' ~alice


## command, builtin, and

## enable

Before moving on to the last part of the command-line
processing cycle, we'll take a look at the command
lookup order that we touched on inChapter 4and how it
can be altered with several shell built-ins.

The default order for command lookup is functions,
followed by built-ins, with scripts and executables last.
There are three built-ins that you can use to override this
order: **command** , **builtin** , and **enable**.

**command** removes alias and function lookup.[9] Only
built-ins and commands found in the search path are
executed. This is useful if you want to create functions
that have the same name as a shell built-in or a command
in the search path and you need to call the original
command from the function. For instance, we might want
to create a function called _cd_ that replaces the standard **cd**
command with one that does some fancy things and then
executes the built-in **cd** :

```
cd ( )
{
# Some fancy things
command cd
}
```

In this case we avoid plunging the function into a
recursive loop by placing **command** in front of **cd**. This
ensures that the built-in **cd** is called and not the function.

**command** has some options, listed inTable 7-8.

**Table 7-8. command options**

```
Option Description
```
```
-p Uses a default value for PATH
```
```
-v
```
```
Prints the command or pathname used to
invoke the command
```
```
-V A more verbose description than with -v
```
- Turns off further option checking

The **-p** option is a default path which guarantees that the
command lookup will find all of the standard UNIX
utilities. In this case, **command** will ignore the directories
in your **PATH** .[10]


**builtin** is very similar to **command** but is more
restrictive. It looks up only built-in commands, ignoring
functions and commands found in **PATH**. We could have
replaced **command** with **builtin** in the **cd** example above.

The last command enables and disables shell built-ins—it
is called **enable**. Disabling a built-in allows a shell script
or executable of the same name to be run without giving a
full pathname. Consider the problem many beginning
UNIX shell programmers have when they name a script
_test_. Much to their surprise, executing **test** usually results
in nothing, because the shell is executing the built-in **test** ,
rather than the shell script. Disabling the built-in with
**enable** overcomes this.[11]

Table 7-9 lists the options available with **enable** .[12]
Some options are for working with _dynamically loadable_
built-ins. SeeAppendix Cfor details on these options,
and how to create and load your own built-in commands.

**Table 7-9. enable options**

```
Option Description
```
```
-a
Displays every built-in and whether it is
enabled or not
```

```
Option Description
```
```
-d Deletes a built-in loaded with -f
```
```
-f
filename
```
```
Loads a new built-in from the
shared-object filename
```
```
-n
Disables a built-in or displays a list of
disabled built-ins
```
```
-p Displays a list of all of the built-ins
```
```
-s
Restricts the output to POSIX "special"
built-ins
```
Of these options, **-n** is the most useful; it is used to
disable a built-in. **enable** without an option enables a
built-in. More than one built-in can be given as arguments
to **enable** , so **enable -n pushd popd dirs** would disable
the **pushd** , **popd** , and **dirs** built-ins.[13]

You can find out what built-ins are currently enabled and
disabled by using the command on its own, or with the **-p**


option; **enable** or **enable -p** will list all enabled built-ins,
and **enable -n** will list all disabled built-ins. To get a
complete list with their current status, you can use **enable
-a**.

The **-s** option restricts the output to POSIX `special'
built-ins. These are :, ., **source** , **break** , **continue** , **eval** ,
**exec** , **exit** , **export** , **readonly** , **return** , **set** , **shift** , **trap** , and
**unset**.


## eval

We have seen that quoting lets you skip steps in
command-line processing. Then there's the **eval**
command, which lets you go through the process again.
Performing command-line processing twice may seem
strange, but it's actually very powerful: it lets you write
scripts that create command strings on the fly and then
pass them to the shell for execution. This means that you
can give scripts "intelligence" to modify their own
behavior as they are running.

The **eval** statement tells the shell to take **eval** 's arguments
and run them through the command-line processing steps
all over again. To help you understand the implications of
**eval** , we'll start with a trivial example and work our way
up to a situation in which we're constructing and running
commands on the fly.

**eval ls** passes the string **ls** to the shell to execute; the shell
prints a list of files in the current directory. Very simple;
there is nothing about the string **ls** that needs to be sent
through the command-processing steps twice. But
consider this:

```
listpage="ls | more"
$listpage
```

Instead of producing a paginated file listing, the shell will
treat **|** and **more** as arguments to _ls_ , and _ls_ will complain
that no files of those names exist. Why? Because the pipe
character "appears" in Step 6 when the shell evaluates the
variable, _after_ it has actually looked for pipe characters.
The variable's expansion isn't even parsed until Step 9. As
a result, the shell will treat **|** and **more** as arguments to _ls_ ,
so that _ls_ will try to find files called **|** and _more_ in the
current directory!

Now consider **eval $listpage** instead of just **$listpage**.
When the shell gets to the last step, it will run the
command **eval** with arguments **ls** , **|** , and **more**. This
causes the shell to go back to Step 1 with a line that
consists of these arguments. It finds **|** in Step 2 and splits
the line into two commands, _ls_ and _more_. Each command
is processed in the normal (and in both cases trivial) way.
The result is a paginated list of the files in your current
directory.

Now you may start to see how powerful **eval** can be. It is
an advanced feature that requires considerable
programming cleverness to be used most effectively. It
even has a bit of the flavor of artificial intelligence, in
that it enables you to write programs that can "write" and
execute other programs.[14]You probably won't use **eval**
for everyday shell programming, but it's worth taking the
time to understand what it can do.

As a more interesting example, we'll revisit Task 4-1, the
very first task in the book. In it, we constructed a simple


pipeline that sorts a file and prints out the first _N_ lines,
where _N_ defaults to 10. The resulting pipeline was:

```
sort -nr $1 | head -${2:-10}
```
The first argument specified the file to sort; **$2** is the
number of lines to print.

Now suppose we change the task just a bit so that the
default is to print the _entire file_ instead of 10 lines. This
means that we don't want to use _head_ at all in the default
case. We could do this in the following way:

```
if [ -n "$2" ]; then
sort -nr $1 | head -$2
else
sort -nr $1
fi
```
In other words, we decide which pipeline to run
according to whether **$2** is null. But here is a more
compact solution:

```
eval sort -nr \$1 ${2:+"| head -\$2"}
```
The last expression in this line evaluates to the string **|
head -\$2** if **$2** exists (is not null); if **$2** is null, then the
expression is null too. We backslash-escape dollar signs
( **\$** ) before variable names to prevent unpredictable
results if the variables' values contain special characters
like **>** or **|**. The backslash effectively puts off the
variables' evaluation until the **eval** command itself runs.
So the entire line is either:


```
eval sort -nr \$1 | head -\$2
```
if **$2** is given, or:

```
eval sort -nr \$1
```
if **$2** is null. Once again, we can't just run this command
without **eval** because the pipe is "uncovered" after the
shell tries to break the line up into commands. **eval**
causes the shell to run the correct pipeline when **$2** is
given.

Next, we'll revisit Task 7-2 from earlier in this chapter,
the _start_ script that lets you start a command in the
background and save its standard output and standard
error in a logfile. Recall that the one-line solution to this
task had the restriction that the command could not
contain output redirectors or pipes. Although the former
doesn't make sense when you think about it, you certainly
would want the ability to start a pipeline in this way.

**eval** is the obvious way to solve this problem:

```
eval "$@" > logfile 2>&1 &
```
The only restriction that this imposes on the user is that
pipes and other such special characters be quoted
(surrounded by quotes or preceded by backslashes).

Here's a way to apply **eval** in conjunction with various
other interesting shell programming concepts.


```
Task 7-3
```
```
Implement the core of the make utility as a shell script.
```
_make_ is known primarily as a programmer's tool, but it
seems as though someone finds a new use for it every
day. Without going into too much extraneous detail, _make_
basically keeps track of multiple files in a particular
project, some of which depend on others (e.g., a
document depends on its word processor input file(s)). It
makes sure that when you change a file, all of the other
files that depend on it are processed.

For example, assume you're using the _troff_ word
processor to write a book. You have files for the book's
chapters called _ch1.t_ , _ch2.t_ , and so on; the _troff_ output for
these files are _ch1.out_ , _ch2.out_ , etc. You run commands
like **troff ch** _N_ **.t > ch** _N_ **.out** to do the processing. While
you're working on the book, you tend to make changes to
several files at a time.

In this situation, you can use _make_ to keep track of which
files need to be reprocessed, so that all you need to do is
type **make** , and it will figure out what needs to be done.
You don't need to remember to reprocess the files that
have changed.

How does _make_ do this? Simple: it compares the
_modification times_ of the input and output files (called


_sources_ and _targets_ in _make_ terminology), and if the input
file is newer, then _make_ reprocesses it.

You tell _make_ which files to check by building a file
called _makefile_ that has constructs like this:

```
target : source1 source2 ...
commands to make target
```
This essentially says, "For _target_ to be up to date, it must
be newer than all of the _source_ s. If it's not, run the
_commands_ to bring it up to date." The _commands_ are on
one or more lines that must start with TABs: e.g., to make
_ch7.out_ :

```
ch7.out : ch7.t
troff ch7.t > ch7.out
```
Now suppose that we write a shell function called
_makecmd_ that reads and executes a single construct of
this form. Assume that the _makefile_ is read from standard
input. The function would look like the following code.

```
makecmd ( )
{
read target colon sources
for src in $sources; do
if [ $src -nt $target ]; then
while read cmd && [ $(grep \t* $cmd) ]; do
echo "$cmd"
eval ${cmd#\t}
done
break
fi
```

```
done
}
```
This function reads the line with the target and sources;
the variable **colon** is just a placeholder for the :. Then it
checks each source to see if it's newer than the target,
using the **-nt** file attribute test operator that we saw in
Chapter 5. If the source is newer, it reads, prints, and
executes the commands until it finds a line that doesn't
start with a TAB or it reaches end-of-file. (The real _make_
does more than this; see the exercises at the end of this
chapter.) After running the commands (which are stripped
of the initial TAB), it breaks out of the **for** loop, so that it
doesn't run the commands more than once.

As a final example of **eval** , we'll look again at _procimage_ ,
the graphics utility that we developed in the last three
chapters. Recall that one of the problems with the script
as it stands is that it performs the process of scaling and
bordering regardless of whether you want them. If no
command-line options are present, a default size, border
width, and border color are used. Rather than invent some
**if then** logic to get around this, we'll look at how you can
dynamically build a pipeline of commands in the script;
those commands that aren't needed simply disappear
when the time comes to execute them. As an added
bonus, we'll add another capability to our script: _image
enhancement_.

Looking at the _procimage_ script you'll notice that the
NetPBM commands form a nice pipeline; the output of
one operation becomes the input to the next, until we end


up with the final image. If it weren't for having to use a
particular conversion utility, we could reduce the script to
the following pipeline (ignoring options for now):

```
cat $filename | convertimage | pnmscale | pnmmargin |\
pnmtojpeg > $outfile
```
Or, better yet:

```
convertimage $filename | pnmscale | pnmmargin | pnmtojpeg \
> $outfile
```
As we've already seen, this is equivalent to:

```
eval convertimage $filename | pnmscale | pnmmargin |\
pnmtojpeg > $outfile
```
And knowing what we do about how **eval** operates, we
can transform this into:

```
eval "convertimage" $filename " | pnmscale" " | pnmmargin" \
" | pnmtojpeg " > $outfile
```
And thence to:

```
convert='convertimage'
scale=' | pnmscale'
border=' | pnmmargin'
standardise=' | pnmtojpeg
```
```
eval $convert $filename $scale $border $standardise > $outfile
```
Now consider what happens when we don't want to scale
the image. We do this:

```
scale=""
```

```
while getopts ":s:w:c:" opt; do
case $opt in
s ) scale=' | pnmscale' ;;
```
```
...
```
```
eval $convert $filename $scale $border $standardise > $outfile
```
In this code fragment, **scale** is set to a default of the
empty string. If **-s** is not given on the command line, then
the final line evaluates with **$scale** as the empty string
and the pipeline will "collapse" into:

```
$convert $filename $border $standardise > $outfile
```
Using this principle, we can modify the previous version
of the _procimage_ script and produce a pipeline version.
For each input file we need to construct and run a
pipeline based upon the options given on the command
line. Here is the new version:

```
# Set up the defaults
width=1
colour='-color grey'
usage="Usage: $0 [-s N] [-w N] [-c S] imagefile..."
# Initialise the pipeline components
standardise=' | pnmtojpeg -quiet'
while getopts ":s:w:c:" opt; do
case $opt in
s ) size=$OPTARG
scale=' | pnmscale -quiet -xysize $size $size' ;;
w ) width=$OPTARG
border=' | pnmmargin $colour $width' ;;
c ) colour="-color $OPTARG"
border=' | pnmmargin $colour $width' ;;
\? ) echo $usage
```

```
exit 1 ;;
esac
done
shift $(($OPTIND - 1))
if [ -z "$@" ]; then
echo $usage
exit 1
fi
# Process the input files
for filename in "$@"; do
case $filename in
*.gif ) convert='giftopnm' ;;
*.tga ) convert='tgatoppm' ;;
*.xpm ) convert='xpmtoppm' ;;
*.pcx ) convert='pcxtoppm' ;;
*.tif ) convert='tifftopnm' ;;
*.jpg ) convert='jpegtopnm -quiet' ;;
* ) echo "$0: Unknown filetype '${filename##*.}'"
exit 1;;
esac
outfile=${filename%.*}.new.jpg
eval $convert $filename $scale $border $standardise > $outfile
done
```
This version has been simplified somewhat from the
previous one in that it no longer needs a temporary file to
hold the converted file. It is also a lot easier to read and
understand. To show how easy it is to add further
processing to the script, we'll now add one more NetPBM
utility.

NetPBM provides a utility to enhance an image and make
it sharper: _pnmnlfilt_. This utility is an image filter that
samples the image and can enhance edges in the image (it
can also smooth the image if given the appropriate


values). It takes two parameters that tell it how much to
enhance the image. For the purposes of our script, we'll
just choose some optimal values and provide an option to
switch enhancement on and off in the script.

To put the new capability in place all we have to do is
add the new option ( **-S** ) to the **getopts case** statement,
update the usage line, and add a new variable to the
pipeline. Here is the new code:

```
# Set up the defaults
width=1
colour='-color grey'
usage="Usage: $0 [-S] [-s N] [-w N] [-c S] imagefile..."
# Initialise the pipeline components
standardise=' | pnmtojpeg -quiet'
while getopts ":Ss:w:c:" opt; do
case $opt in
S ) sharpness=' | pnmnlfilt -0.7 0.45' ;;
s ) size=$OPTARG
scale=' | pnmscale -quiet -xysize $size $size' ;;
w ) width=$OPTARG
border=' | pnmmargin $colour $width' ;;
c ) colour="-color $OPTARG"
border=' | pnmmargin $colour $width' ;;
\? ) echo $usage
exit 1 ;;
esac
done
shift $(($OPTIND - 1))
if [ -z "$@" ]; then
echo $usage
exit 1
fi
# Process the input files
for filename in "$@"; do
```

```
case $filename in
*.gif ) convert='giftopnm' ;;
*.tga ) convert='tgatoppm' ;;
*.xpm ) convert='xpmtoppm' ;;
*.pcx ) convert='pcxtoppm' ;;
*.tif ) convert='tifftopnm' ;;
*.jpg ) convert='jpegtopnm -quiet' ;;
* ) echo "$0: Unknown filetype '${filename##*.}'"
exit 1;;
esac
outfile=${filename%.*}.new.jpg
eval $convert $filename $scale $border $sharpness $standardise > $outfile
done
```
We could go on forever with increasingly complex
examples of **eval** , but we'll settle for concluding the
chapter with a few exercises. The questions in Exercise 3
are really more like items on the menu of food for
thought.

1. Here are a couple of ways to enhance procimage,
    the graphics utility:
       1. Add an option, **-q** , that allows the user to
          turn on and off the printing of diagnostic
          information from the NetPBM utilities.
          You'll need to map **-q** to the **-quiet**
          option of the utilities. Also, add your
          own diagnostic output for those utilities
          that don't print anything, e.g., the format
          conversions.
       2. Add an option that allows the user to
          specify the order that the NetPBM


```
processes take place, i.e., whether
enhancing the image comes before
bordering, or bordering comes before
resizing. Rather than using an if
construct to make the choice amongst
hard-coded orders, construct a string
dynamically which will look similar to
this:
```
```
"eval $convert $filename $scale $border $sharpness
$standardise > $outfile"
```
3. You'll then need **eval** to evaluate this
    string.
2. The function _makecmd_ in the solution to Task 7-3
represents an oversimplification of the real
make's functionality. make actually checks file
dependencies recursively, meaning that a source
on one line in a makefile can be a target on
another line. For example, the book chapters in
the example could themselves depend on some
figures in separate files that were made with a
graphics package.
1. Write a function called **readtargets** that
goes through the makefile and stores all
of the targets in a variable or temporary
file.
2. _makecmd_ merely checks to see if any of
the sources are newer than the given


```
target. It should really be a recursive
routine that looks like this:
```
```
function makecmd ( )
{
target=$1
get sources for $target
for each source src; do
if $src is also a target in this makefile then
makecmd $src
fi
if [ $src -nt $target ]; then
run commands to make target
return
fi
done
}
```
3. Implement this.
4. Write the "driver" script that turns the
    _makecmd_ function into a full make
    program. This should make the target
    given as argument, or if none is given,
    the first target listed in the makefile.
5. The above _makecmd_ still doesn't do one
    important thing that the real make does:
    allow for "symbolic" targets that aren't
    files. These give make much of the
    power that makes it applicable to such an
    incredible variety of situations. Symbolic
    targets always have a modification time
    of 0, so that make always runs the


```
commands to make them. Modify
makecmd so that it allows for symbolic
targets. (Hint: the crux of this problem is
to figure out how to get a file's
modification time. This is quite
difficult.)
```
3. Here are some problems that really test your
    knowledge of **eval** and the shell's command-line
    processing rules. Solve these and you're a true
    bash hacker!
       1. Advanced shell programmers sometimes
          use a little trick that includes **eval** : using
          the value of a variable as the name of
          another variable. In other words, you can
          give a shell script control over the names
          of variables to which it assigns values.
          The latest version of bash has this built
          in in the form of **${!** _varname_ }, where
          varname contains the name of another
          variable that will be the target of the
          operation. This is known as indirect
          expansion. How would you do this _only_
          using **eval**? (Hint: if **$object** equals
          "person", and **$person** is "alice", then
          you might think that you could type **echo**
          **$$object** and get the response **alice**. This
          doesn't actually work, but it's on the right
          track.)


2. You could use the above technique
    together with other **eval** tricks to
    implement new control structures for the
    shell. For example, see if you can write a
    script that emulates the behavior of a **for**
    loop in a conventional language like C or
    Pascal, i.e., a loop that iterates a fixed
    number of times, with a loop variable
    that steps from 1 to the number of
    iterations (or, for C fans, 0 to
    iterations-1). Call your script **loop** to
    avoid clashes with the keywords **for** and
    **do**.
3. The **pushd** , **popd** , and **dirs** functions
    that we built up in previous chapters
    can't handle directories with spaces in
    their names (because DIR_STACK uses
    a space as a delimiter). Use **eval** to
    overcome this limitation. (Hint: use **eval**
    to implement an array. Each array
    element is called array1, array2, ...
    arrayn, and each array element contains a
    directory name.)
4. (The following doesn't have that much to
    do with the material in this chapter per
    se, but it is a classic programming
    exercise:) Write the function _alg2rpn_
    used in the section on command blocks.
    Here's how to do this: Arithmetic


```
expressions in algebraic notation have
the form expr op expr , where each expr
is either a number or another expression
(perhaps in parentheses), and op is +, -,
x, /, or % (remainder). In RPN,
expressions have the form expr expr op.
For example: the algebraic expression
2 + 3 is 2 3 + in RPN; the RPN equivalent
of (2+3) x (9-5) is 2 3 + 9 5 - x. The
main advantage of RPN is that it
obviates the need for parentheses and
operator precedence rules (e.g., x is
evaluated before +). The dc program
accepts standard RPN, but each
expression should have "p" appended to
it, which tells dc to print its result; e.g.,
the first example above should be given
to dc as 2 3 + p.
```
5. You need to write a routine that converts
    algebraic notation to RPN. This should
    be (or include) a function that calls itself
    (a recursive function) whenever it
    encounters a subexpression. It is
    especially important that this function
    keep track of where it is in the input
    string and how much of the string it "eats
    up" during its processing. (Hint: make
    use of the pattern-matching operators
    discussed inChapter 4to ease the task of
    parsing input strings.) To make your life


```
easier, don't worry about operator
precedence for now; just convert to RPN
from left to right: e.g., treat 3+4x5 as
(3+ 4) x5 and 3x4+5 as (3x4)+5. This
makes it possible for you to convert the
input string on the fly, i.e., without
having to read in the whole thing before
doing any processing.
```
6. Enhance your solution to the previous
    exercise so that it supports operator
    precedence in the "usual" order: x, /, **%**
    (remainder) +, -. For example, treat
    **3+4x5** as **3+(4x5)** and **3x4+5** as **(3x4)+5**.
7. Here is something else to really test your
    skills; write a graphics utility script,
    index, that takes a list of image files,
    reduces them in size and creates an
    "index" image. An index image is
    comprised of thumbnail-sized versions of
    the original images, placed neatly in
    columns and rows, and with a caption
    underneath (usually the name of the
    original file). Besides the list of files,
    you'll need some options, including the
    number of columns to create and the size
    of the thumbnail images. You might also
    like to include an option to specify the
    gap between each image. The new
    NetPBM utilities you'll need are _pbmtext_


and _pnmcat_. You'll also need _pnmscale_
and one or more of the conversion
utilities, depending upon whether you
decide to take in various formats (as we
did for _procimage_ ) and what output
format you decide on. _pbmtext_ takes as
an argument some text and converts the
text into a PNM bitmap. _pnmcat_ is a little
more complex. Like **cat** , it concatenates
things; in this case, images. You can
specify as many PNM files as you like as
arguments and _pnmcat_ will put them
together into one long image. By using
the **-lr** and **-tb** options, you can specify
whether you want the images to be
placed one after the other going from left
to right, or from top to bottom. The first
option to _pnmcat_ is the background
color. It can be either **-black** for a black
background, or **-white** for a white
background. We suggest **-white** to match
the _pbmtext_ black text on a white
background. You'll need to take each
file, run the filename through _pbmtext_ ,
and use _pnmcat_ to place it underneath a
scaled down version of the original
image. Then you'll need to continue
doing this for each file and use _pnmcat_ to
connect them together. In addition, you'll
have to keep tabs on how many columns
you have completed and when to start a


```
new row. Note that you'll need to build
up the rows individually and use pnmcat
to connect them together. pnmcat won't
do this for you automatically.
```
[7]Two obscure variations on this: the shell substitutes

the current directory ( **$PWD** ) for **~+** and the previous
directory ( **$OLDPWD** ) for **~-**. In _bash_ 2.0 there are two
more: ~N+ and ~N-. These are replaced by the
corresponding element in the directory stack as given by
the **dirs** command.

[8]However, as we saw in Chapter 1, ` **\** '' (i.e., single

quote, backslash, single quote, single quote) acts pretty
much like a single quote in the middle of a single-quoted
string; e.g., ` **abc** ` **\** '` **def** ' evaluates to **abc** ` **def**.

[9] **command** removes alias lookup as a side effect.

Because the first argument of **command** is no longer the
first word that _bash_ parses, it is not subjected to alias
lookup.

[10]Unless _bash_ has been compiled with a brain-dead

value for the default. SeeChapter 11for how to change
the default value.

[11]Note that the wrong _test_ may still be run. If your

current directory is the last in **PATH** you'll probably
execute the system file _test_. _test_ is not a good name for a
program.


[12]The **-d** , **-f** , **-p** , and **-s** options are not available in

versions of _bash_ prior to 2.0.

[13]Be careful—it is possible to disable **enable** ( **enable**

**-n enable** ). There is a compile-time option that allows
**builtin** to act as an escape-hatch. For more details, see
Chapter 11.

[14]You could actually do this without **eval** , by **echo** ing

commands to a temporary file and then "sourcing" that
file with. _filename_. But that is _much_ less efficient.


# Chapter 8. Process

# Handling

The UNIX operating system built its reputation on a
small number of concepts, all of which are simple yet
powerful. We've seen most of them by now: standard
input/output, pipes, text-filtering utilities, the
tree-structured file system, and so on. UNIX also gained
notoriety as the first small-computer operating system to
give each user control over more than one process. We
call this capability _user-controlled multitasking_.

You may not think that multitasking is a big deal. You're
probably used to the idea of running a process in the
background by putting an ampersand (&) at the end of the
command line. You have also seen the idea of a _subshell_
inChapter 4, when we showed how shell scripts run.

In this chapter, we will cover most of _bash_ 's features that
relate to multitasking and process handling in general.
We say "most" because some of these features are, like
the file descriptors we saw in the previous chapter, of
interest only to low-level systems programmers.

We'll start out by looking at certain important primitives
for identifying processes and for controlling them during
login sessions and within shell scripts. Then we will
move out to a higher-level perspective, looking at ways to


get processes to communicate with each other. We'll look
in more detail at concepts we've already seen, like pipes
and subshells.

Don't worry about getting bogged down in low-level
technical details about UNIX. We will provide only the
technical information that is necessary to explain
higher-level features, plus a few other tidbits designed to
pique your curiosity. If you are interested in finding out
more about these areas, refer to your UNIX Programmer's
Manual or a book on UNIX internals that pertains to your
version of UNIX. You might also find _UNIX Power Tools_
of value.

We strongly recommend that you try out the examples in
this chapter. The behavior of code that involves multiple
processes is not as easy to understand on paper as most of
the other examples in this book.

# Process IDs and Job

# Numbers

UNIX gives all processes numbers, called _process IDs_ ,
when they are created. You will notice that when you run
a command in the background by appending **&** to it, the
shell responds with a line that looks like this:

```
$ alice & [1] 93
```

In this example, 93 is the process ID for the **alice** process.
The **[1]** is a _job number_ assigned by the shell (not the
operating system). What's the difference? Job numbers
refer to background processes that are currently running
under your shell, while process IDs refer to all processes
currently running on the entire system, for all users. The
term _job_ basically refers to a command line that was
invoked from your shell.

If you start up additional background jobs while the first
one is still running, the shell will number them 2, 3, etc.
For example:

```
$ duchess & [2] 102
$ hatter & [3] 104
```
Clearly, 1, 2, and 3 are easier to remember than 93, 102,
and 104!

The shell includes job numbers in messages it prints
when a background job completes:[1]

```
[1]+ Done alice
```
We'll explain what the plus sign means soon. If the job
exits with non-zero status (seeChapter 5), the shell will
indicate the exit status:[2]

```
[1]+ Exit 1 alice
```
The shell prints other types of messages when certain
abnormal things happen to background jobs; we'll see
these later in this chapter.


[1]The messages are, by default, printed before the next

prompt is displayed so as not to interrupt any output on
the display. You can make the notification messages
display immediately by using **set -b**.

[2]In POSIX mode, the message is slightly different:

" **[1]+ Done(1) alice** ". The number in parentheses is the
exit status of the job. POSIX mode can be selected via the
**set** command or by starting _bash_ in POSIX mode. For
further information, seeTable 2-1andTable 2-5


# Job Control

Why should you care about process IDs or job numbers?
Actually, you could probably get along fine through your
UNIX life without ever referring to process IDs (unless
you use a windowing workstation—as we'll see soon).
Job numbers are more important, however: you can use
them with the shell commands for _job control_ .[3]

You already know the most obvious way of controlling a
job: create one in the background with **&**. Once a job is
running in the background, you can let it run to
completion, bring it into the _foreground_ , or send it a
message called a _signal_.

## Foreground and

## Background

The built-in command **fg** brings a background job into the
foreground. Normally this means that the job will have
control of your terminal or window and therefore will be
able to accept your input. In other words, the job will
begin to act as if you typed its command without the **&**.

If you have only one background job running, you can
use **fg** without arguments, and the shell will bring that job


into the foreground. But if you have several jobs running
in the background, the shell will pick the one that you put
into the background most recently. If you want some
other job put into the foreground, you need to use the
job's command name, preceded by a percent sign (%), or
you can use its job number, also preceded by %, or its
process ID without a percent sign. If you don't remember
which jobs are running, you can use the command **jobs** to
list them.

A few examples should make this clearer. Let's say you
created three background jobs as above. Then if you type
**jobs** , you will see this:

```
[1] Running alice &
[2]- Running duchess &
[3]+ Running hatter &
```
**jobs** has a few interesting options. **jobs -l** also lists
process IDs:

```
[1] 93 Running alice &
[2]- 102 Running duchess &
[3]+ 104 Running hatter &
```
The **-p** option tells **jobs** to list _only_ process IDs:

```
93
102
104
```
(This could be useful with command substitution; see
Task 8-1.) The **-n** option lists only those jobs whose
status has changed since the shell last reported


it—whether with a **jobs** command or otherwise. **-r**
restricts the list to jobs that are running, while **-s** restricts
the list to those jobs which are stopped, e.g., waiting for
input from the keyboard.[4]Finally, you can use the **-x**
option to execute a command. Any job number provided
to the command will be substituted with the process ID of
the job. For example, if _alice_ is running in the
background, then executing **jobs -x echo %1** will print
the process ID of _alice_.

If you type **fg** without an argument, the shell will put
**hatter** in the foreground, because it was put in the
background most recently. But if you type **fg %duchess**
(or **fg %2** ), **duchess** will go in the foreground.

You can also refer to the job most recently put in the
background by **%+**. Similarly, **%-** refers to the
_next_ -most-recently backgrounded job ( **duchess** in this
case). That explains the plus and minus signs in the
above: the plus sign shows the most recent job whose
status has changed; the minus sign shows the
next-most-recently invoked job.[5]

If more than one background job has the same command,
then **%** _command_ will distinguish between them by
choosing the most recently invoked job (as you'd expect).
If this isn't what you want, you need to use the job
number instead of the command name. However, if the
commands have different _arguments_ , you can use **%?**
_string_ instead of **%** _command_. **%?** _string_ refers to the job


whose command contains the string. For example,
assume you started these background jobs:

```
$ hatter mad & [1] 189
$ hatter teatime & [2] 190
$
```
Then you can use **%?mad** and **%?teatime** to refer to
each of them, although actually **%?ma** and **%?tea** are
sufficient to uniquely identify them.

Table 8-1lists all of the ways to refer to background jobs.
Given how infrequently people use job control
commands, job numbers or command names are
sufficient, and the other ways are superfluous.

**Table 8-1. Ways to refer to background jobs**

```
Reference Background job
```
```
%N Job number N
```
```
%string Job whose command begins with string
```
```
%?string Job whose command contains string
```

**Reference Background job**

%+ Most recently invoked background job

%% Same as above

#### % -

```
Second most recently invoked background
job
```

## Suspending a Job

Just as you can put background jobs into the foreground
with **fg** , you can also put a foreground job into the
background. This involves suspending a job, so that the
shell regains control of your terminal.

To suspend a job, type CTRL-Z while it is running.[6]
This is analogous to typing CTRL-C (or whatever your
interrupt key is), except that you can resume the job after
you have stopped it. When you type CTRL-Z, the shell
responds with a message like this:

```
[1]+ Stopped command
```
Then it gives you your prompt back. To resume a
suspended job so that it continues to run in the
foreground, just type **fg**. If, for some reason, you put
other jobs in the background after you typed CTRL-Z,
use **fg** with a job name or number.

For example:

```
alice is running... CTRL-Z [1]+ Stopped alice
$ hatter & [2] 145
$ fg %alice alice resumes in the foreground...
```
The ability to suspend jobs and resume them in the
foreground comes in very handy when you have a
conventional terminal (as opposed to a windowing


workstation) and you are using a text editor like _vi_ on a
file that needs to be processed. For example, if you are
editing a file for the _troff_ text processor, you can do the
following:

```
$ vi myfile edit the file... CTRL-Z Stopped [1] vi
$ troff myfile troff reports an error $ fg vi comes back up in the same place in your file
```
Programmers often use the same technique when
debugging source code.

You will probably also find it useful to suspend a job and
resume it in the background instead of the foreground.
You may start a command in the foreground (i.e.,
normally) and find that it takes much longer than you
expected—for example, a _grep_ , _sort_ , or database query.
You need the command to finish, but you would also like
control of your terminal back so that you can do other
work. If you type CTRL-Z followed by **bg** , you will
move the job to the background.[7]

You can also suspend a job with CTRL-Y. This is slightly
different from CTRL-Z in that the process is only stopped
when it attempts to read input from the terminal.

[3]If you have an older version of UNIX, it is possible

that your system does not support job control. This is
particularly true for many systems derived from Xenix,
System III, or early versions of System V. On such
systems, _bash_ does not have the **fg** and **bg** commands, job


number arguments to **kill** and **wait** , typing CTRL-Z to
suspend a job, or the TSTP signal.

[4]Options **-r** and **-s** are not available in _bash_ prior to

version 2.0.

[5]This is analogous to **~+** and **~-** as references to the

current and previous directory; see the footnote in
Chapter 7. Also: **%%** is a synonym for **%+**.

[6]This assumes that the CTRL-Z key is set up as your

suspend key; just as with CTRL-C and interrupts, this is
conventional but by no means required.

[7] Be warned, however, that not all commands are

"well-behaved" when you do this. Be especially careful
with commands that run over a network on a remote
machine; you may end up confusing the remote program.


# Signals

We mentioned earlier that typing CTRL-Z to suspend a
job is similar to typing CTRL-C to stop a job, except that
you can resume the job later. They are actually similar in
a deeper way: both are particular cases of the act of
sending a _signal_ to a process.

A signal is a message that one process sends to another
when some abnormal event takes place or when it wants
the other process to do something. Most of the time, a
process sends a signal to a subprocess it created. You're
undoubtedly already comfortable with the idea that one
process can communicate with another through an I/O
pipeline; think of a signal as another way for processes to
communicate with each other. (In fact, any textbook on
operating systems will tell you that both are examples of
the general concept of _interprocess communication_ , or
IPC.[8])

Depending on the version of UNIX, there are two or three
dozen types of signals, including a few that can be used
for whatever purpose a programmer wishes. Signals have
numbers (from 1 to the number of signals the system
supports) and names; we'll use the latter. You can get a
list of all the signals on your system, by name and
number, by typing **kill -l**. Bear in mind, when you write
shell code involving signals, that signal names are more
portable to other versions of UNIX than signal numbers.


## Control-Key Signals

When you type CTRL-C, you tell the shell to send the
INT (for "interrupt") signal to the current job; CTRL-Z
sends TSTP (on most systems, for "terminal stop"). You
can also send the current job a QUIT signal by typing
CTRL-\ (control-backslash); this is sort of like a
"stronger" version of CTRL-C.[9]You would normally
use CTRL-\ when (and _only_ when) CTRL-C doesn't
work.

As we'll see soon, there is also a "panic" signal called
KILL that you can send to a process when even CTRL-\
doesn't work. But it isn't attached to any control key,
which means that you can't use it to stop the currently
running process. INT, TSTP, and QUIT are the only
signals you can use with control keys.[10]

You can customize the control keys used to send signals
with options of the _stty_ command. These vary from
system to system—consult your manpage for the
command—but the usual syntax is **stty** _signame char_.
_signame_ is a name for the signal that, unfortunately, is
often not the same as the names we use here.Table 1-7in
Chapter 1 lists _stty_ names for signals found on all
versions of UNIX. _char_ is the control character, which
you can give using the convention that ^(circumflex)
represents "control." For example, to set your INT key to
CTRL-X on most systems, use:


```
stty intr ^X
```
Now that we've told you how to do this, we should add
that we don't recommend it. Changing your signal keys
could lead to trouble if someone else has to stop a
runaway process on your machine.

Most of the other signals are used by the operating system
to advise processes of error conditions, like a bad
machine code instruction, bad memory address, or
division by zero, or "interesting" events such as a timer
("alarm") going off. The remaining signals are used for
esoteric error conditions of interest only to low-level
systems programmers; newer versions of UNIX have
even more signal types.


## kill

You can use the built-in shell command **kill** to send a
signal to any process you created—not just the currently
running job. **kill** takes as an argument the process ID, job
number, or command name of the process to which you
want to send the signal. By default, **kill** sends the TERM
("terminate") signal, which usually has the same effect as
the INT signal you send with CTRL-C. But you can
specify a different signal by using the signal name (or
number) as an option, preceded by a dash.

**kill** is so named because of the nature of the default
TERM signal, but there is another reason, which has to do
with the way UNIX handles signals in general. The full
details are too complex to go into here, but the following
explanation should suffice.

Most signals cause a process that receives them to die;
therefore, if you send any one of these signals, you "kill"
the process that receives it. However, programs can be set
up toSection 8.4 specific signals and take some other
action. For example, a text editor would do well to save
the file being edited before terminating when it receives a
signal such as INT, TERM, or QUIT. Determining what
to do when various signals come in is part of the fun of
UNIX systems programming.


Here is an example of **kill**. Say you have an **alice** process
in the background, with process ID 150 and job number
1, which needs to be stopped. You would start with this
command:

```
$ kill %1
```
If you were successful, you would see a message like
this:

```
[1]+ Terminated alice
```
If you don't see this, then the TERM signal failed to
terminate the job. The next step would be to try QUIT:

```
$ kill -QUIT %1
```
If that worked, you would see this message:

```
[1]+ Exit 131 alice
```
The 131 is the exit status returned by **alice** .[11]But if
even QUIT doesn't work, the "last-ditch" method would
be to use KILL:

```
$ kill -KILL %1
```
This produces the message:

```
[1]+ Killed alice
```
It is impossible for a process to Section 8.4 a KILL
signal—the operating system should terminate the
process immediately and unconditionally. If it doesn't,
then either your process is in one of the "funny states"


we'll see later in this chapter, or (far less likely) there's a
bug in your version of UNIX.

Here's another example.

```
Task 8-1
```
```
Write a script called killalljobs that kills all background
jobs.
```
The solution to this task is simple, relying on **jobs -p** :

```
kill "$@" $(jobs -p)
```
You may be tempted to use the KILL signal immediately,
instead of trying TERM (the default) and QUIT first.
Don't do this. TERM and QUIT are designed to give a
process the chance to "clean up" before exiting, whereas
KILL will stop the process, wherever it may be in its
computation. _Use KILL only as a last resort!_

You can use the **kill** command with any process you
create, not just jobs in the background of your current
shell. For example, if you use a windowing system, then
you may have several terminal windows, each of which
runs its own shell. If one shell is running a process that
you want to stop, you can **kill** it from another
window—but you can't refer to it with a job number
because it's running under a different shell. You must
instead use its process ID.


## ps

This is probably the only situation in which a casual user
would need to know the ID of a process. The command
_ps_ gives you this information; however, it can give you
lots of extra information as well.

_ps_ is a complex command. It takes several options, some
of which differ from one version of UNIX to another. To
add to the confusion, you may need different options on
different UNIX versions to get the same information! We
will use options available on the two major types of
UNIX systems, those derived from System V (such as
many of the versions for Intel Pentium PCs, as well as
IBM's AIX and Hewlett-Packard's HP/UX) and BSD
(Mac OS X, SunOS, BSD/OS). If you aren't sure which
kind of UNIX version you have, try the System V options
first.

You can invoke _ps_ in its simplest form without any
options. In this case, it will print a line of information
about the current login shell and any processes running
under it (i.e., background jobs). For example, if you were
to invoke three background jobs, as we saw earlier in the
chapter, the _ps_ command on System V-derived versions
of UNIX would produce output that looks something like
this:


```
PID TTY TIME COMD
146 pts/10 0:03 -bash
2349 pts/10 0:03 alice
2367 pts/10 0:17 hatter
2389 pts/10 0:09 duchess
2390 pts/10 0:00 ps
```
The output on BSD-derived systems looks like this:

```
PID TT STAT TIME COMMAND
146 10 S 0:03 /bin/bash
2349 10 R 0:03 alice
2367 10 D 0:17 hatter teatime
2389 10 R 0:09 duchess
2390 10 R 0:00 ps
```
(You can ignore the STAT column.) This is a bit like the
**jobs** command. PID is the process ID; TTY (or TT) is the
terminal (or pseudo-terminal, if you are using a
windowing system) the process was invoked from; TIME
is the amount of processor time (not real or "wall clock"
time) the process has used so far; COMD (or
COMMAND) is the command. Notice that the BSD
version includes the command's arguments, if any; also
notice that the first line reports on the parent shell
process, and in the last line, _ps_ reports on itself.

_ps_ without arguments lists all processes started from the
current terminal or pseudo-terminal. But since _ps_ is not a
shell command, it doesn't correlate process IDs with the
shell's job numbers. It also doesn't help you find the ID of
the runaway process in another shell window.


To get this information, use **ps -a** (for "all"); this lists
information on a different set of processes, depending on
your UNIX version.

### System V

Instead of listing all processes that were started under a
specific terminal, **ps -a** on System V-derived systems lists
all processes associated with any terminal that aren't
group leaders. For our purposes, a "group leader" is the
parent shell of a terminal or window. Therefore, if you
are using a windowing system, **ps -a** lists all jobs started
in all windows (by all users), but not their parent shells.

Assume that, in the previous example, you have only one
terminal or window. Then **ps -a** will print the same output
as plain **ps** except for the first line, since that's the parent
shell. This doesn't seem to be very useful.

But consider what happens when you have multiple
windows open. Let's say you have three windows, all
running terminal emulators like _xterm_ for the X Window
System. You start background jobs **alice** , **duchess** , and
**hatter** in windows with pseudo-terminal numbers 1, 2,
and 3, respectively. This situation is shown inFigure 8-1.


```
Figure 8-1. Background jobs in multiple
windows
```
Assume you are in the uppermost window. If you type **ps** ,
you will see something like this:

```
PID TTY TIME COMD
146 pts/1 0:03 bash
2349 pts/1 0:03 alice
2390 pts/1 0:00 ps
```
But if you type **ps -a** , you will see this:

```
PID TTY TIME COMD
146 pts/1 0:03 bash
2349 pts/1 0:03 alice
2367 pts/2 0:17 duchess
```

```
2389 pts/3 0:09 hatter
2390 pts/1 0:00 ps
```
Now you should see how **ps -a** can help you track down a
runaway process. If it's **hatter** , you can type **kill 2389**. If
that doesn't work, try **kill -QUIT 2389** , or in the worst
case, **kill -KILL 2389**.


### BSD

On BSD-derived systems, **ps -a** lists all jobs that were
started on any terminal; in other words, it's a bit like
concatenating the the results of plain **ps** for every user on
the system. Given the above scenario, **ps -a** will show
you all processes that the System V version shows, plus
the group leaders (parent shells).

Unfortunately, **ps -a** (on any version of UNIX) will not
report processes that are in certain conditions where they
"forget" things like what shell invoked them and what
terminal they belong to. Such processes are known as
"zombies" or "orphans." If you have a serious runaway
process problem, it's possible that the process has entered
one of these states.

Let's not worry about why or how a process gets this way.
All you need to understand is that the process doesn't
show up when you type **ps -a**. You need another option to
_ps_ to see it: on System V, it's **ps -e** ("everything"),
whereas on BSD, it's **ps -ax**.

These options tell _ps_ to list processes that either weren't
started from terminals or "forgot" what terminal they
were started from. The former category includes lots of
processes that you probably didn't even know existed:
these include basic processes that run the system and
so-called _daemons_ (pronounced "demons") that handle


system services like mail, printing, network filesystems,
etc.

In fact, the output of **ps -e** or **ps -ax** is an excellent source
of education about UNIX system internals, if you're
curious about them. Run the command on your system
and, for each line of the listing that looks interesting,
invoke _man_ on the process name or look it up in the
_UNIX Programmer's Manual_ for your system.

User shells and processes are listed at the very bottom of
**ps -e** or **ps -ax** output; this is where you should look for
runaway processes. Notice that many processes in the
listing have**?** instead of a terminal. Either these aren't
supposed to have one (such as the basic daemons) or
they're runaways. Therefore it's likely that if **ps -a** doesn't
find a process you're trying to kill, **ps -e** (or **ps -ax** ) will
list it with**?** in the TTY (or TT) column. You can
determine which process you want by looking at the
COMD (or COMMAND) column.

[8]Pipes and signals were the only IPC mechanisms in

early versions of UNIX. More modern versions like
System V and BSD have additional mechanisms, such as
sockets, named pipes, and shared memory. Named pipes
are accessible to shell programmers through the _mknod_ (1)
command, which is beyond the scope of this book.

[9]CTRL-\ can also cause the shell to leave a file called

_core_ in your current directory. This file contains an image


of the process to which you sent the signal; a programmer
could use it to help debug the program that was running.
The file's name is a (very) old-fashioned term for a
computer's memory. Other signals leave these "core
dumps" as well; unless you require them, or someone else
does, just delete them.

[10] Some BSD-derived systems have additional

control-key signals.

[11]When a shell script is sent a signal, it exits with status

128+ _N_ , where _N_ is the number of the signal it received. In
this case, **alice** is a shell script, and QUIT happens to be
signal number 3.


# trap

We've been discussing how signals affect the casual user;
now let's talk a bit about how shell programmers can use
them. We won't go into too much depth about this,
because it's really the domain of systems programmers.

We mentioned above that programs in general can be set
up toSection 8.4 specific signals and process them in
their own way. The **trap** built-in command lets you do
this from within a shell script. **trap** is most important for
"bullet-proofing" large shell programs so that they react
appropriately to abnormal events—just as programs in
any language should guard against invalid input. It's also
important for certain systems programming tasks, as we'll
see in the next chapter.

The syntax of **trap** is:

```
trap cmd sig1 sig2 ...
```
That is, when any of _sig1_ , _sig2_ , etc., are received, run
_cmd_ , then resume execution. After _cmd_ finishes, the script
resumes execution just after the command that was
interrupted.[12]

Of course, _cmd_ can be a script or function. The _sig_ s can
be specified by name or by number. You can also invoke
**trap** without arguments, in which case the shell will print


a list of any traps that have been set, using symbolic
names for the signals.

Here's a simple example that shows how **trap** works.
Suppose we have a shell script called **loop** with this code:

```
while true; do
sleep 60
done
```
This will just pause for 60 seconds (the _sleep_ command)
and repeat indefinitely. **true** is a "do-nothing" command
whose exit status is always 0.[13]Try typing in this script.
Invoke it, let it run for a little while, then type CTRL-C
(assuming that is your interrupt key). It should stop, and
you should get your shell prompt back.

Now insert this line at the beginning of the script:

```
trap "echo 'You hit control-C!'" INT
```
Invoke the script again. Now hit CTRL-C. The odds are
overwhelming that you are interrupting the _sleep_
command (as opposed to **true** ). You should see the
message "You hit control-C!", and the script will not stop
running; instead, the _sleep_ command will abort, and it
will loop around and start another _sleep_. Hit CTRL-Z to
get it to stop and then type **kill %1**.

Next, run the script in the background by typing **loop &**.
Type **kill %loop** (i.e., send it the TERM signal); the
script will terminate. Add TERM to the **trap** command,
so that it looks like this:


```
trap "echo 'You hit control-C!'" INT TERM
```
Now repeat the process: run it in the background and type
**kill %loop**. As before, you will see the message and the
process will keep on running. Type **kill -KILL %loop** to
stop it.

Notice that the message isn't really appropriate when you
use **kill**. We'll change the script so it prints a better
message in the **kill** case:

```
trap "echo 'You hit control-C!'" INT
trap "echo 'You tried to kill me!'" TERM
```
```
while true; do
sleep 60
done
```
Now try it both ways: in the foreground with CTRL-C
and in the background with **kill**. You'll see different
messages.

## Traps and Functions

The relationship between traps and shell functions is
straightforward, but it has certain nuances that are worth
discussing. The most important thing to understand is that
functions are considered part of the shell that invokes
them. This means that traps defined in the invoking shell
will be recognized inside the function, and more
importantly, any traps defined in the function will be


recognized by the invoking shell once the function has
been called. Consider this code:

```
settrap ( ) {
trap "echo 'You hit control-C!'" INT
}
```
```
settrap
while true; do
sleep 60
done
```
If you invoke this script and hit your interrupt key, it will
print "You hit control-C!" In this case the trap defined in
**settrap** still exists when the function exits.

Now consider:

```
loop ( ) {
trap "echo 'How dare you!'" INT
while true; do
sleep 60
done
}
```
```
trap "echo 'You hit control-C!'" INT
loop
```
When you run this script and hit your interrupt key, it will
print "How dare you!" In this case the trap is defined in
the calling script, but when the function is called the trap
is redefined. The first definition is lost. A similar thing
happens with:

```
loop ( ) {
trap "echo 'How dare you!'" INT
```

```
}
```
```
trap "echo 'You hit control-C!'" INT
loop
while true; do
sleep 60
done
```
Once again, the trap is redefined in the function; this is
the definition used once the loop is entered.

We'll now show a more practical example of traps.

```
Task 8-2
```
```
As part of an electronic mail system, write the shell
code that lets a user compose a message.
```
The basic idea is to use _cat_ to create the message in a
temporary file and then hand the file's name off to a
program that actually sends the message to its destination.
The code to create the file is very simple:

```
msgfile=/tmp/msg$$
cat > $msgfile
```
Since _cat_ without an argument reads from the standard
input, this will just wait for the user to type a message
and end it with the end-of-text character CTRL-D.


## Process ID Variables and

## Temporary Files

The only thing new about this script is **$$** in the filename
expression. This is a special shell variable whose value is
the process ID of the current shell.

To see how **$$** works, type **ps** and note the process ID of
your shell process ( _bash_ ). Then type **echo** " **$$** "; the shell
will respond with that same number. Now type **bash** to
start a subshell, and when you get a prompt, repeat the
process. You should see a different number, probably
slightly higher than the last one.

A related built-in shell variable is**!** (i.e., its value is **$!** ),
which contains the process ID of the most recently
invoked background job. To see how this works, invoke
any job in the background and note the process ID printed
by the shell next to **[1]**. Then type **echo** " **$!** "; you should
see the same number.

To return to our mail example: since all processes on the
system must have unique process IDs, **$$** is excellent for
constructing names of temporary files.


The directory _/tmp_ is conventionally used for temporary
files. Many systems also have another directory, _/var/tmp_ ,
for the same purpose.

Nevertheless, a program should clean up such files before
it exits, to avoid taking up unnecessary disk space. We
could do this in our code very easily by adding the line
**rm $msgfile** after the code that actually sends the
message. But what if the program receives a signal during
execution? For example, what if a user changes her mind
about sending the message and hits CTRL-C to stop the
process? We would need to clean up before exiting. We'll
emulate the actual UNIX _mail_ system by saving the
message being written in a file called _dead.letter_ in the
current directory. We can do this by using **trap** with a
command string that includes an **exit** command:

```
trap 'mv $msgfile dead.letter; exit' INT TERM
msgfile=/tmp/msg$$
cat > $msgfile
# send the contents of $msgfile to the specified mail address...
rm $msgfile
```
When the script receives an INT or TERM signal, it will
remove the temp file and then exit. Note that the
command string isn't _evaluated_ until it needs to be run, so
**$msgfile** will contain the correct value; that's why we
surround the string in single quotes.

But what if the script receives a signal before **msgfile** is
created—unlikely though that may be? Then **mv** will try
to rename a file that doesn't exist. To fix this, we need to
test for the existence of the file **$msgfile** before trying to


delete it. The code for this is a bit unwieldy to put in a
single command string, so we'll use a function instead:

```
function cleanup {
if [ -e $msgfile ]; then
mv $msgfile dead.letter
fi
exit
}
```
```
trap cleanup INT TERM
```
```
msgfile=/tmp/msg$$
cat > $msgfile
# send the contents of $msgfile to the specified mail address...
rm $msgfile
```

## Ignoring Signals

Sometimes a signal comes in that you don't want to do
anything about. If you give the null string ("" or `') as the
command argument to **trap** , then the shell will effectively
ignore that signal. The classic example of a signal you
may want to ignore is HUP (hangup). This can occur on
some UNIX systems when a hangup (disconnection while
using a modem—literally "hanging up") or some other
network outage takes place.

HUP has the usual default behavior: it will kill the
process that receives it. But there are bound to be times
when you don't want a background job to terminate when
it receives a hangup signal.

To do this, you could write a simple function that looks
like this:

```
function ignorehup {
trap "" HUP
eval "$@"
}
```
We write this as a function instead of a script for reasons
that will become clearer when we look in detail at
subshells at the end of this chapter.


Actually, there is a UNIX command called _nohup_ that
does precisely this. The **start** script from the last chapter
could include **nohup** :

```
eval nohup "$@" > logfile 2>&1 &
```
This prevents HUP from terminating your command and
saves its standard and error output in a file. Actually, the
following is just as good:

```
nohup "$@" > logfile 2>&1 &
```
If you understand why **eval** is essentially redundant when
you use **nohup** in this case, then you have a firm grasp on
the material in the previous chapter. Note that if you don't
specify a redirection for any output from the command,
_nohup_ places it in a file called _nohup.out_.


## disown

Another way to ignore the HUP signal is with the **disown**
built-in.[14] **disown** takes as an argument a job
specification, such as the process ID or job ID, and
removes the process from the list of jobs. The process is
effectively "disowned" by the shell from that point on,
i.e., you can only refer to it by its process ID since it is no
longer in the job table.

**disown** 's **-h** option performs the same function as _nohup_ ;
it specifies that the shell should stop the hangup signal
from reaching the process under certain circumstances.
Unlike _nohup_ , it is up to you to specify where the output
from the process is to go.

**disown** also provides two options which can be of use. - **a**
with no other arguments applies the operation to all jobs
owned by the shell. The - **r** option with does the same but
only for currently running jobs.


## Resetting Traps

Another "special case" of the **trap** command occurs when
you give a dash ( **-** ) as the command argument. This resets
the action taken when the signal is received to the default,
which usually is termination of the process.

As an example of this, let's return to Task 8-2, our mail
program. After the user has finished sending the message,
the temporary file is erased. At that point, since there is
no longer any need to clean up, we can reset the signal
trap to its default state. The code for this, apart from
function definitions, is:

```
trap abortmsg INT
trap cleanup TERM
```
```
msgfile=/tmp/msg$$
cat > $msgfile
# send the contents of $msgfile to the specified mail address...
rm $msgfile
```
```
trap - INT TERM
```
The last line of this code resets the handlers for the INT
and TERM signals.

At this point you may be thinking that you could get
seriously carried away with signal handling in a shell
script. It is true that "industrial strength" programs devote


considerable amounts of code to dealing with signals. But
these programs are almost always large enough so that
the signal-handling code is a tiny fraction of the whole
thing. For example, you can bet that the real UNIX _mail_
system is pretty darn bullet-proof.

However, you will probably never write a shell script that
is complex enough, and that needs to be robust enough, to
merit lots of signal handling. You may write a _prototype_
for a program as large as _mail_ in shell code, but
prototypes by definition do not need to be bullet-proofed.

Therefore, you shouldn't worry about putting
signal-handling code in every 20-line shell script you
write. Our advice is to determine if there are any
situations in which a signal could cause your program to
do something seriously bad and add code to deal with
those contingencies. What is "seriously bad"? Well, with
respect to the above examples, we'd say that the case
where HUP causes your job to terminate is seriously bad,
while the temporary file situation in our mail program is
not.

[12] This is what _usually_ happens. Sometimes the

command currently running will abort ( _sleep_ acts like
this, as we'll see soon); at other times it will finish
running. Further details are beyond the scope of this
book.


[13]This command is the same as the built-in shell no-op

command ":".

[14] **disown** is not available in versions of _bash_ prior to

2.0.


# Coroutines

We've spent the last several pages on almost microscopic
details of process behavior. Rather than continue our
descent into the murky depths, we'll revert to a
higher-level view of processes.

Earlier in this chapter, we covered ways of controlling
multiple simultaneous jobs within an interactive login
session; now we'll consider multiple process control
within shell programs. When two (or more) processes are
explicitly programmed to run simultaneously and
possibly communicate with each other, we call them
_coroutines_.

This is actually nothing new: a pipeline is an example of
coroutines. The shell's pipeline construct encapsulates a
fairly sophisticated set of rules about how processes
interact with each other. If we take a closer look at these
rules, we'll be better able to understand other ways of
handling coroutines—most of which turn out to be
simpler than pipelines.

When you invoke a simple pipeline—say, **ls | more—** the
shell invokes a series of UNIX primitive operations, or
_system calls_. In effect, the shell tells UNIX to do the
following things; in case you're interested, we include in
parentheses the actual system call used at each step:


1. Create two subprocesses, which we'll call P1 and
    P2 (the fork system call).
2. Set up I/O between the processes so that P1's
    standard output feeds into P2's standard input
    (pipe).
3. Start _/bin/ls_ in process P1 (exec).
4. Start _/bin/more_ in process P2 (exec).
5. Wait for both processes to finish (wait).

You can probably imagine how the above steps change
when the pipeline involves more than two processes.

Now let's make things simpler. We'll see how to get
multiple processes to run at the same time if the processes
do not need to communicate. For example, we want the
processes **alice** and **hatter** to run as coroutines, without
communication, in a shell script. Our initial solution
would be this:

```
alice &
hatter
```
Assume for the moment that **hatter** is the last command
in the script. The above will work—but only if **alice**
finishes first. If **alice** is still running when the script
finishes, then it becomes an _orphan_ , i.e., it enters one of
the "funny states" we mentioned earlier in this chapter.
Never mind the details of orphanhood; just believe that


you don't want this to happen, and if it does, you may
need to use the "runaway process" method of stopping it,
discussed earlier in this chapter.

## wait

There is a way of making sure the script doesn't finish
before **alice** does: the built-in command **wait**. Without
arguments, **wait** simply waits until all background jobs
have finished. So to make sure the above code behaves
properly, we would add **wait** , like this:

```
alice &
hatter
wait
```
Here, if **hatter** finishes first, the parent shell will wait for
**alice** to finish before finishing itself.

If your script has more than one background job and you
need to wait for specific ones to finish, you can give **wait**
the process ID of the job.

However, you will probably find that **wait** without
arguments suffices for all coroutines you will ever
program. Situations in which you would need to wait for
specific background jobs are quite complex and beyond
the scope of this book.


## Advantages and

## Disadvantages of

## Coroutines

In fact, you may be wondering why you would ever need
to program coroutines that don't communicate with each
other. For example, why not just run **hatter** after **alice** in
the usual way? What advantage is there in running the
two jobs simultaneously?

Even if you are running on a computer with only one
processor (CPU), then there may be a performance
advantage.

Roughly speaking, you can characterize a process in
terms of how it uses system resources in three ways:
whether it is _CPU-intensive_ (e.g., does lots of number
crunching), _I/O-intensive_ (does a lot of reading or writing
to the disk), or _interactive_ (requires user intervention).

We already know fromChapter 1that it makes no sense
to run an interactive job in the background. But apart
from that, the more two or more processes differ with
respect to these three criteria, the more advantage there is
in running them simultaneously. For example, a
number-crunching statistical calculation would do well


when running at the same time as a long, I/O-intensive
database query.

On the other hand, if two processes use resources in
similar ways, it may even be less efficient to run them at
the same time as it would be to run them sequentially.
Why? Basically, because under such circumstances, the
operating system often has to "time-slice" the resource(s)
in contention.

For example, if both processes are "disk hogs," the
operating system may enter a mode where it constantly
switches control of the disk back and forth between the
two competing processes; the system ends up spending at
least as much time doing the switching as it does on the
processes themselves. This phenomenon is known as
_thrashing_ ; at its most severe, it can cause a system to
come to a virtual standstill. Thrashing is a common
problem; system administrators and operating system
designers both spend lots of time trying to minimize it.


## Parallelization

If you have a computer with multiple CPUs you should
be less concerned about thrashing. Furthermore,
coroutines can provide dramatic increases in speed on this
type of machine, which is often called a _parallel_
computer; analogously, breaking up a process into
coroutines is sometimes called _parallelizing_ the job.

Normally, when you start a background job on a
multiple-CPU machine, the computer will assign it to the
next available processor. This means that the two jobs are
actually—not just metaphorically—running at the same
time.

In this case, the running time of the coroutines is
essentially equal to that of the longest-running job plus a
bit of overhead, instead of the sum of the runtimes of all
processes (although if the CPUs all share a common disk
drive, the possibility of I/O-related thrashing still exists).
In the best case—all jobs having the same runtime and no
I/O contention—you get a speedup factor equal to the
number of CPUs.

Parallelizing a program is often not easy; there are several
subtle issues involved and there's plenty of room for
error. Nevertheless, it's worthwhile to know how to
parallelize a shell script whether or not you have a


parallel machine, especially since such machines are
becoming more and more common.

We'll show how to do this—and give you an idea of some
problems involved—by means of a simple task whose
solution is amenable to parallelization.

```
Task 8-3
```
```
Write a utility that allows you to make multiple copies
of a file at the same time.
```
We'll call this script _mcp_. The command **mcp** _filename
dest1 dest2._ .. should copy _filename_ to all of the
destinations given. The code for this should be fairly
obvious:

```
file=$1
shift
for dest in "$@"; do
cp $file $dest
done
```
Now let's say we have a parallel computer and we want
this command to run as fast as possible. To parallelize
this script, it's a simple matter of firing off the _cp_
commands in the background and adding a **wait** at the
end:

```
file=$1
shift
```

```
for dest in "$@"; do
cp $file $dest &
done
wait
```
Simple, right? Well, there is one little problem: what
happens if the user specifies duplicate destinations? If
you're lucky, the file just gets copied to the same place
twice. Otherwise, the identical _cp_ commands will
interfere with each other, possibly resulting in a file that
contains two interspersed copies of the original file. In
contrast, if you give the regular _cp_ command two
arguments that point to the same file, it will print an error
message and do nothing.

To fix this problem, we would have to write code that
checks the argument list for duplicates. Although this isn't
too hard to do (see the exercises at the end of this
chapter), the time it takes that code to run might offset
any gain in speed from parallelization; furthermore, the
code that does the checking detracts from the simple
elegance of the script.

As you can see, even a seemingly trivial parallelization
task has problems resulting from multiple processes that
have concurrent access to a given system resource (a file
in this case). Such problems, known as _concurrency
control_ issues, become much more difficult as the
complexity of the application increases. Complex
concurrent programs often have much more code for
handling the special cases than for the actual job the
program is supposed to do!


Therefore, it shouldn't surprise you that much research
has been and is being done on parallelization, the ultimate
goal being to devise a tool that parallelizes code
automatically. (Such tools do exist; they usually work in
the confines of some narrow subset of the problem.) Even
if you don't have access to a multiple-CPU machine,
parallelizing a shell script is an interesting exercise that
should acquaint you with some of the issues that surround
coroutines.


# Subshells

To conclude this chapter, we will look at a simple type of
interprocess relationship: that of a subshell with its parent
shell. We saw inChapter 3that whenever you run a shell
script, you actually invoke another copy of the shell that
is a subprocess of the main, or _parent_ , shell process. Now
let's look at subshells in more detail.

## Subshell Inheritance

The most important things you need to know about
subshells are what characteristics they get, or _inherit_ ,
from their parents. These are as follows:

- The current directory
- Environment variables
- Standard input, output, and error, plus any other
    open file descriptors
- Signals that are ignored

Just as important are the things that a subshell does not
inherit from its parent:


- Shell variables, except environment variables and
    those defined in the environment file (usually
    _.bashrc_ )
- Handling of signals that are not ignored

We covered some of this inChapter 3, but these points
are common sources of confusion, so they bear repeating.


## Nested Subshells

Subshells need not be in separate scripts; you can also
start a subshell within the same script (or function) as the
parent. You do this in a manner very similar to the
command blocks we saw in the last chapter. Just surround
some shell code with parentheses (instead of curly
brackets), and that code will run in a subshell. We'll call
this a _nested_ subshell.

For example, here is the calculator program from the last
chapter, with a subshell instead of a command block:

```
( while read line; do
echo "$(alg2rpn $line)"
done
) | dc
```
The code inside the parentheses will run as a separate
process. This is usually less efficient than a command
block. The differences in functionality between subshells
and command blocks are very few; they primarily pertain
to issues of scope, i.e., the domains in which definitions
of things like shell variables and signal traps are known.
First, code inside a nested subshell obeys the above rules
of subshell inheritance, except that it knows about
variables defined in the surrounding shell; in contrast,
think of blocks as code units that inherit _everything_ from
the outer shell. Second, variables and traps defined inside


a command block are known to the shell code after the
block, whereas those defined in a subshell are not.

For example, consider this code:

```
{
hatter=mad
trap "echo 'You hit CTRL-C!'" INT
}
while true; do
echo "\$hatter is $hatter"
sleep 60
done
```
If you run this code, you will see the message **$hatter is
mad** every 60 seconds, and if you hit CTRL-C, you will
see the message, **You hit CTRL-C!**. You will need to hit
CTRL-Z to stop it (don't forget to kill it with **kill %+** ).
Now let's change it to a nested subshell:

```
(
hatter=mad
trap "echo 'You hit CTRL-C!'" INT
)
while true; do
echo "\$hatter is $hatter"
sleep 60
done
```
If you run this, you will see the message **$hatter is** ; the
outer shell doesn't know about the subshell's definition of
**hatter** and therefore thinks it's null. Furthermore, the
outer shell doesn't know about the subshell's trap of the
INT signal, so if you hit CTRL-C, the script will
terminate.


If a language supports code nesting, then it's considered
desirable that definitions inside a nested unit have a scope
limited to that nested unit. In other words, nested
subshells give you better control than command blocks
over the scope of variables and signal traps. Therefore,
we feel that you should use subshells instead of command
blocks if they are to contain variable definitions or signal
traps—unless efficiency is a concern.


# Process Substitution

A unique but rarely used feature of _bash_ is _process
substitution_. Let's say that you had two versions of a
program that produced large quantities of output. You
want to see the differences between the output from each
version. You could run the two programs, redirecting
their output to files, and then use the _cmp_ utility to see
what the differences were.

Another way would be to use process substitution. There
are two forms of this substitution. One is for input to a
process: **>** ( _list_ ); the other is for output from a process:
**<** ( _list_ ). _list_ is a process that has its input or output
connected to something via a _named pipe_. A named pipe
is simply a temporary file that acts like a pipe with a
name.

In our case, we could connect the outputs of the two
programs to the input of _cmp_ via named pipes:

```
cmp <(prog1) <(prog2)
```
_prog1_ and _prog2_ are run concurrently and connect their
outputs to named pipes. _cmp_ reads from each of the pipes
and compares the information, printing any differences as
it does so.

This chapter has covered a lot of territory. Here are some
exercises that should help you make sure you have a firm


grasp on the material. Don't worry if you have trouble
with the last one; it's especially difficult.

1. Write a shell script called _pinfo_ that combines the
    **jobs** and **ps** commands by printing a list of jobs
    with their job numbers, corresponding process
    IDs, running times, and full commands.
2. Take a non-trivial shell script and "bullet-proof"
    it with signal traps.
3. Take a non-trivial shell script and parallelize it as
    much as possible.
4. Write the code that checks for duplicate
    arguments to the _mcp_ script. Bear in mind that
    different pathnames can point to the same file.
    (Hint: if **$i** is "1", then **eval** `echo \${$i}' prints
    the first command-line argument. Make sure you
    understand why.)


# Chapter 9. Debugging

# Shell Programs

We hope that we have convinced you that _bash_ can be
used as a serious UNIX programming environment. It
certainly has enough features, control structures, etc. But
another essential part of a programming environment is a
set of powerful, integrated _support tools_. For example,
there is a wide assortment of screen editors, compilers,
debuggers, profilers, cross-referencers, etc., for languages
like C and C++. If you program in one of these
languages, you probably take such tools for granted, and
you would undoubtedly cringe at the thought of having to
develop code with, say, the _ed_ editor and the _adb_
machine-language debugger.

But what about programming support tools for _bash_? Of
course, you can use any editor you like, including _vi_ and
_emacs_. And because the shell is an interpreted language,
you don't need a compiler.[1]But there are no other tools
available.

This chapter looks at some useful features that you can
use to debug shell programs. We'll look at how you can
utilize them in the first part of this chapter. We'll then
look at some powerful new features of _bash_ , not present
in most Bourne shell workalikes, which will help in
building a shell script debugging tool. At the end of the


chapter, we'll show step by step how to build a debugger
for _bash_. The debugger, called _bashdb_ , is a basic yet
functional program that will not only serve as an
extended example of various shell programming
techniques, but will also provide you with a useful tool
for examining the workings of your own shell scripts.

# Basic Debugging Aids

What sort of functionality do you need to debug a
program? At the most empirical level, you need a way of
determining _what_ is causing your program to behave
badly, and _where_ the problem is in the code. You usually
start with an obvious _what_ (such as an error message,
inappropriate output, infinite loop, etc.), try to work
backwards until you find a _what_ that is closer to the
actual problem (e.g., a variable with a bad value, a bad
option to a command), and eventually arrive at the exact
_where_ in your program. Then you can worry about _how_ to
fix it.

Notice that these steps represent a process of starting with
obvious information and ending up with often obscure
facts gleaned through deduction and intuition. Debugging
aids make it easier to deduce and intuit by providing
relevant information easily or even automatically,
preferably without modifying your code.

The simplest debugging aid (for any language) is the
output statement, **echo** , in the shell's case. Indeed,


old-time programmers debugged their FORTRAN code
by inserting **WRITE** cards into their decks. You can
debug by putting lots of **echo** statements in your code
(and removing them later), but you will have to spend lots
of time narrowing down not only _what_ exact information
you want but also _where_ you need to see it. You will also
probably have to wade through lots and lots of output to
find the information you really want.

## Set Options

Luckily, the shell has a few basic features that give you
debugging functionality beyond that of **echo**. The most
basic of these are options to the **set -o** command (as
covered inChapter 3). These options can also be used on
the command line when running a script, asTable 9-1
shows.

**Table 9-1. Debugging options**

```
set -o
option
```
```
Command-line
option
Action
```
```
noexec -n
Don't run commands; check
for syntax errors only
```

```
set -o
option
```
```
Command-line
option
Action
```
```
verbose -v
Echo commands before
running them
```
```
xtrace -x
Echo commands after
command-line processing
```
The **verbose** option simply echoes (to standard error)
whatever input the shell gets. It is useful for finding the
exact point at which a script is bombing. For example,
assume your script looks like this:

```
alice
hatter
march
teatime
treacle
well
```
None of these commands is a standard UNIX program,
and each does its work silently. Say the script crashes
with a cryptic message like "segmentation violation."
This tells you nothing about which command caused the
error. If you type **bash -v** _scriptname_ , you might see this:


```
alice
hatter
march
segmentation violation
teatime
treacle
well
```
Now you know that **march** is the probable
culprit—though it is also possible that **march** bombed
because of something it expected **alice** or **hatter** to do
(e.g., create an input file) that they did incorrectly.

The **xtrace** option is more powerful: it echoes command
lines after they have been through parameter substitution,
command substitution, and the other steps of
command-line processing (as listed in Chapter 7). For
example:

```
.ps 8
$ set -o xtrace $ alice=girl + alice=girl
$ echo "$alice" + echo girl
girl
$ ls -l $(type -path vi) ++ type -path vi
+ ls -F -l /usr/bin/vi
lrwxrwxrwx 1 root root 5 Jul 26 20:59 /usr/bin/vi -> elvis*
$
```
As you can see, **xtrace** starts each line it prints with **+**
(each **+** representing a level of expansion). This is
actually customizable: it's the value of the built-in shell
variable **PS4**. So if you set **PS4** to " **xtrace—>** " (e.g., in
your _.bash_profile_ or _.bashrc_ ), then you'll get **xtrace**
listings that look like this:


```
.ps 8
$ ls -l $(type -path vi) xxtrace--> type -path vi
xtrace--> ls -l /usr/bin/vi
lrwxrwxrwx 1 root root 5 Jul 26 20:59 /usr/bin/vi -> elvis*
$
```
Notice that for multiple levels of expansion, only the first
character of **PS4** is printed. This makes the output more
readable.

An even better way of customizing **PS4** is to use a
built-in variable we haven't seen yet: **LINENO** , which
holds the number of the currently running line in a shell
script.[2] Put this line in your _.bash_profile_ or
environment file:

```
PS4='line $LINENO: '
```
We use the same technique as we did with **PS1** in
Chapter 3: using single quotes to postpone the evaluation
of the string until each time the shell prints the prompt.
This will print messages of the form **line** _N_ : in your trace
output. You could even include the name of the shell
script you're debugging in this prompt by using the
positional parameter **$0** :

```
PS4='$0 line $LINENO: '
```
As another example, say you are trying to track down a
bug in a script called **alice** that contains this code:

```
dbfmq=$1.fmq
...
fndrs=$(cut -f3 -d' ' $dfbmq)
```

You type **alice teatime** to run it in the normal way, and it
hangs. Then you type **bash -x alice teatime** , and you see
this:

```
+ dbfmq=teatime.fmq
...
+ + cut -f3 -d
```
It hangs again at this point. You notice that _cut_ doesn't
have a filename argument, which means that there must
be something wrong with the variable **dbfmq**. But it has
executed the assignment statement **dbfmq=teatime.fmq**
properly... ah- _hah_! You made a typo in the variable name
inside the command substitution construct.[3]You fix it,
and the script works properly.

The last option is **noexec** , which reads in the shell script
and checks for syntax errors, but doesn't execute
anything. It's worth using if your script is syntactically
complex (lots of loops, command blocks, string operators,
etc.) and the bug has side effects (like creating a large file
or hanging up the system).

You can turn on these options with **set -o** _option_ in your
shell scripts, and, as explained inChapter 3, turn them off
with **set +o** _option_. For example, if you're debugging a
chunk of code, you can precede it with **set -o xtrace** to
print out the executed commands, and end the chunk with
**set +o xtrace**.


Note, however, that once you have turned **noexec** on, you
won't be able to turn it off; a **set +o noexec** will never be
executed.


## Fake Signals

_Fake signals_ are more sophisticated set of debugging
aids. They can be used in **trap** statements to get the shell
to act under certain conditions. Recall from the previous
chapter that **trap** allows you to install some code that
runs when a particular signal is sent to your script.

Fake signals work in the same way, but they are
generated by the shell itself, as opposed to the other
signals which are generated externally. They represent
runtime events that are likely to be of interest to
debuggers—both human ones and software tools—and
can be treated just like real signals within shell scripts.
Table 9-2lists the four fake signals available in _bash_.

**Table 9-2. Fake signals**

```
Fake
signal
Sent when
```
```
EXIT The shell exits from script
```
```
ERR A command returning a non-zero exit status
```

```
Fake
signal
Sent when
```
```
DEBUG The shell has executed a statement[4]
```
#### RETURN

```
A shell function or a script executed with
the. or source builtins finishes executing[5]
```
```
[4]The DEBUG signal is not available in bash versions
prior to 2.0.
```
```
[5] The RETURN signal is not available in bash
versions prior to 3.0.
```
### EXIT

The EXIT trap, when set, will run its code whenever the
script within which it was set exits.[6]

Here's a simple example:

```
trap 'echo exiting from the script' EXIT
echo 'start of the script'
```
If you run this script, you will see this output:


```
start of the script
exiting from the script
```
In other words, the script starts by setting the trap for its
own exit, then prints a message. The script then exits,
which causes the shell to generate the signal EXIT, which
in turn runs the code **echo exiting from the script**.

An EXIT trap occurs no matter how the script
exits—whether normally (by finishing the last statement),
by an explicit **exit** or **return** statement, or by receiving a
"real" signal such as INT or TERM. Consider this inane
number-guessing program:

```
trap 'echo Thank you for playing!' EXIT
```
```
magicnum=$(($RANDOM%10+1))
echo 'Guess a number between 1 and 10:'
while read -p 'Guess: ' guess ; do
sleep 4
if [ "$guess" = $magicnum ]; then
echo 'Right!'
exit
fi
echo 'Wrong!'
done
```
This program picks a number between 1 and 10 by
getting a random number (the built-in variable
**RANDOM** ), extracting the last digit (the remainder when
divided by 10), and adding 1. Then it prompts you for a
guess, and after 4 seconds, it will tell you if you guessed
right.


If you did, the program will exit with the message,
"Thank you for playing!", i.e., it will run the EXIT trap
code. If you were wrong, it will prompt you again and
repeat the process until you get it right. If you get bored
with this little game and hit CTRL-C or CTRL-D while
waiting for it to tell you whether you were right, you will
also see the message.

The EXIT trap is especially useful when you want to print
out the values of variables at the point that your script
exits. For example, by printing the value of loop counter
variables, you can find the most appropriate places in a
complicated script, with many nested **for** loops, to enable
**xtrace** or place debug output.


### ERR

The fake signal ERR enables you to run code whenever a
command in the surrounding script or function exits with
non-zero status. Trap code for ERR can take advantage of
the built-in variable**?** , which holds the exit status of the
previous command. It survives the trap and is accessible
at the beginning of the trap-handling code.

A simple but effective use of this is to put the following
code into a script you want to debug:

```
function errtrap {
es=$?
echo "ERROR: Command exited with status $es."
}
trap errtrap ERR
```
The first line saves the nonzero exit status in the local
variable **es**.

For example, if the shell can't find a command, it returns
status 127. If you put the code in a script with a line of
gibberish (like "nhbdeuje"), the shell responds with:

```
scriptname: line N: nhbdeuje: command not found
ERROR: command exited with status 127.
```
N is the number of the line in the script that contains the
bad command. In this case, the shell prints the line
number as part of its own error-reporting mechanism,


since the error was a command that the shell could not
find. But if the nonzero exit status comes from another
program, the shell doesn't report the line number. For
example:

```
function errtrap {
es=$?
echo "ERROR: Command exited with status $es."
}
trap errtrap ERR
function bad {
return 17
}
bad
```
This only prints **ERROR: Command exited with status
17**.

It would obviously be an improvement to include the line
number in this error message. The built-in variable
LINENO exists, but if you use it inside a function, it
evaluates to the line number in the function, not in the
overall file. In other words, if you used $LINENO in the
**echo** statement in the **errtrap** routine, it would always
evaluate to 2.

To get around this problem, we simply pass $LINENO as
an argument to the trap handler, surrounding it in single
quotes so that it doesn't get evaluated until the fake signal
actually comes in:

```
function errtrap {
es=$?
echo "ERROR line $1: Command exited with status $es."
```

```
}
trap 'errtrap $LINENO' ERR
...
```
If you use this with the above example, the result is the
message **, ERROR line 12: Command exited with
status 17**. This is much more useful. We'll see a variation
on this technique shortly.

This simple code is actually not a bad all-purpose
debugging mechanism. It takes into account that a
nonzero exit status does not necessarily indicate an
undesirable condition or event: remember that every
control construct with a conditional ( **if** , **while** , etc.) uses a
nonzero exit status to mean "false." Accordingly, the shell
doesn't generate ERR traps when statements or
expressions in the "condition" parts of control structures
produce nonzero exit statuses. Also, an ERR trap is not
inherited by shell functions, command substitutions, and
commands executed in a subshell. However this
inheritance behaviour can be turned on by using **set -o
errtrace** (or **set -E** ).[7]

One disadvantage is that exit statuses are not as uniform
(or even as meaningful) as they should be, as we
explained inChapter 5. A particular exit status need not
say anything about the nature of the error or even that
there was an error.


### DEBUG

Another fake signal, DEBUG, causes the trap code to be
executed before every statement in a function or script.[8]
This has two main uses. First is the use for humans, as a
sort of "brute force" method of tracking a certain element
of a program's state that you notice has gone awry.

For example, you notice the value of a particular variable
is running amok. The naive approach is to put in a lot of
**echo** statements to check the variable's value at several
points. The DEBUG trap makes this easier by letting you
do this:

```
function dbgtrap
{
echo " badvar
is
$badvar "
}
```
```
trap dbgtrap DEBUG
...section of code in which the problem occurs...
trap - DEBUG # turn off the DEBUG trap
```
This code will print the value of the wayward variable
before every statement between the two **trap** s.

One important point to remember when using DEBUG is
that it is not inherited by functions called from the shell in
which it is set. In other words, if your shell sets a


DEBUG trap and then calls a function, the statements
within the function will not execute the trap. There are
three ways around this. Firstly you can set a trap for
DEBUG explicitly within the function. Alternately you
can **declare** the function with the - **t** option which turns on
debug inheritance in functions and allows a function to
inherit a DEBUG trap from the caller. Lastly you can use
**set -o functrace** (or **set -T** ) which does the same thing as
**declare** but applies to all functions.[9]

The second use of the DEBUG signal is as a primitive for
implementing a _bash_ debugger. We'll look at doing just
that shortly.


### RETURN

A RETURN trap is executed each time a shell function or
a script executed with the. or **source** commands finishes
executing.

As with DEBUG, the RETURN trap is not inherited by
functions. You again have the options of setting the trap
for RETURN within the function, **declare** the function
with the - **t** option so that that function inherits the trap, or
use **set -o functrace** to turn on the inheritance for all
functions.

Here is a simple example of a RETURN trap:

```
function returntrap {
echo "A return occurred"
}
trap returntrap RETURN
function hello {
echo "hello world"
}
hello
```
When the script is executed it executes the _hello_ function
and then runs the trap:

```
$ ./returndemo
hello world
A return occurred
$
```

Notice that it didn't trap when the script itself finished.
The trap would only have run at the end of the script if
we'd **source** d the script. Normally, to trap at the exiting of
the script we'd also need to define a trap for the EXIT
signal that we looked at earlier.

In addition to these fake signals, _bash_ 3.0 added some
other features to help with writing a full-scale debugger
for _bash_. The first of these is the **extdebug** option to the
**shopt** command, which switches on certain things that
are useful for a debugger. These include:

- The -F option to **declare** displays the source
    filename and line number corresponding to each
    function name supplied as an argument.
- If the command that is run by the DEBUG trap
    returns a non-zero value, the next command is
    skipped and not executed.
- If the command run by the DEBUG trap returns a
    value of 2, and the shell is executing in a
    subroutine (a shell function or a shell script
    executed by the. or **source** commands), a call to
    **return** is simulated.

The shell also has a new option, _—debugger_ , which
switches on both the **extdebug** and **functrace**
functionality.


## Debugging Variables

_Bash_ 3.0 added some useful environment variables to aid
in writing a debugger. These include BASH_SOURCE,
which contains an array of filenames that correspond to
what is currently executing; BASH_LINENO, which is
an array of line numbers that correspond to function calls
that have been made; BASH_ARGC and BASH_ARGV
array variables, the first holding the number of
parameters in each frame and the second the parameters
themselves.

We'll now look at writing a debugger, although we'll keep
things simple and avoid using these variables. This also
means the debugger will work with earlier versions of
_bash_.

[1]Actually, if you are really concerned about efficiency,

there are shell code compilers on the market; they convert
shell scripts to C code that often runs quite a bit faster.

[2]In versions of _bash_ prior to 2.0, **LINENO** won't give

you the current line in a function. **LINENO** , instead,
gives an approximation of the number of simple
commands executed so far in the current function.


[3]We should admit that if you had turned on the **nounset**

option at the top of this script, the shell would have
flagged this error.

[6]You can use this signal only for the exiting of a script.

Functions don't generate the EXIT signal, as they are part
of the current shell invocation.

[7]Inheritance of the ERR trap is not available in versions

of _bash_ prior to 3.0.

[8]Warning: the **DEBUG** trap was run _after_ statements in

versions of _bash_ prior to 2.05b. The debugger in this
chapter has been written for the current version of _bash_
where the trap is run before each statement.

[9]Inheritance of the DEBUG trap, **declare -t** , **set -o**

**functrace** , and **set -T** are not available in _bash_ prior to
version 3.0.


# A bash Debugger

In this section we'll develop a very basic debugger for
_bash_ .[10]Most debuggers have numerous sophisticated
features that help a programmer in dissecting a program,
but just about all of them include the ability to step
through a running program, stop it at selected places, and
examine the values of variables. These simple features
are what we will concentrate on providing in our
debugger. Specifically, we'll provide the ability to:

- Specify places in the program at which to stop
    execution. These are called _breakpoints_.
- Execute a specified number of statements in the
    program. This is called _stepping_.
- Examine and change the state of the program
    during its execution. This includes being able to
    print out the values of variables and change them
    when the program is stopped at a breakpoint or
    after stepping.
- Print out the source code we are debugging along
    with indications of where breakpoints are and
    what line in the program we are currently
    executing.


- Provide the debugging capability without having
    to change the original source code of the program
    we wish to debug in any way.

As you will see, the capability to do all of these things
(and more) is easily provided by the constructs and
methods we have seen in previous chapters.

## Structure of the Debugger

The _bashdb_ debugger works by taking a shell script and
turning it into a debugger for itself. It does this by
concatenating debugger functionality and the target
script, which we'll call the guinea pig script, and storing it
in another file that then gets executed. The process is
transparent to users—they will be unaware that the code
that is executing is actually a modified copy of their
script.

The _bash_ debugger has three main sections: the _driver_ ,
the _preamble_ , and the _debugger functions_.

### The driver script

The driver script is responsible for setting everything up.
It is a script called _bashdb_ and looks like this:

```
# bashdb - a bash debugger
# Driver Script: concatenates the preamble and the target script
```

```
# and then executes the new script.
```
```
echo 'bash Debugger version 1.0'
```
```
_dbname=${0##*/}
```
```
if (( $# < 1 )) ; then
echo "$_dbname: Usage: $_dbname filename" >&2
exit 1
fi
```
```
_guineapig=$1
```
```
if [! -r $1 ]; then
echo "$_dbname: Cannot read file '$_guineapig'." >&2
exit 1
fi
```
```
shift
```
```
_tmpdir=/tmp
_libdir=.
_debugfile=$_tmpdir/bashdb.$$ # temporary file for script that is
being debugged
cat $_libdir/bashdb.pre $_guineapig > $_debugfile
exec bash $_debugfile $_guineapig $_tmpdir $_libdir "$@"
```
_bashdb_ takes as the first argument the name of guinea pig
file. Any subsequent arguments are passed on to the
guinea pig as its positional parameters.

If no arguments are given, _bashdb_ prints out a usage line
and exits with an error status. Otherwise, it checks to see
if the file exists. If it doesn't, exist then _bashdb_ prints a
message and exits with an error status. If all is in order,
_bashdb_ constructs a temporary file in the way we saw in


the last chapter. If you don't have (or don't have access to)
_/tmp_ on your system, then you can substitute a different
directory for **_tmpdir** .[11] The variable **_libdir** is the
name of the directory that contains files needed by
_bashdb_ ( _bashdb.pre_ and _bashdb.fns_ ). If you are installing
_bashdb_ on your system for everyone to use, you might
want to place them in _/usr/lib_.

The **cat** statement builds the modified copy of the guinea
pig file: it contains the script found in _bashdb.pre_ (which
we'll look at shortly) followed by a copy of the guinea
pig.


### exec

The last line runs the newly created script with **exec** , a
statement we haven't discussed yet. We've chosen to wait
until now to introduce it because—as we think you'll
agree—it can be dangerous. **exec** takes its arguments as a
command line and runs the command in place of the
current program, in the same process. In other words, a
shell that runs **exec** will _terminate immediately_ and be
replaced by **exec** 's arguments.[12]

In our script, **exec** just runs the newly constructed shell
script, i.e., the guinea pig with its debugger, in another
shell. It passes the new script three arguments—the name
of the original guinea pig file ( **$_guineapig** ), the name of
the temporary directory ( **$_tmpdir** ), and the name of the
library directory ( **$_libdir** )—followed by the user's
positional parameters, if any.


## The Preamble

Now we'll look at the code that gets prepended to the
guinea pig script; we call this the preamble. It's kept in
the file _bashdb.pre_ and looks like this:

```
# bashdb preamble
# This file gets prepended to the shell script being debugged.
# Arguments:
# $1 = the name of the original guinea pig script
# $2 = the directory where temporary files are stored
# $3 = the directory where bashdb.pre and bashdb.fns are stored
```
```
_debugfile=$0
_guineapig=$1
```
```
_tmpdir=$2
_libdir=$3
```
```
shift 3
```
```
source $_libdir/bashdb.fns
_linebp=
let _trace=0
let _i=1
```
```
while read; do
_lines[$_i]=$REPLY
let _i=$_i+1
done < $_guineapig
```
```
trap _cleanup EXIT
```

```
let _steps=1
trap '_steptrap $(( $LINENO -29 ))' DEBUG
```
The first few lines save the three fixed arguments in
variables and shift them out of the way, so that the
positional parameters (if any) are those that the user
supplied on the command line as arguments to the guinea
pig. Then, the preamble reads in another file, _bashdb.fns_ ,
which contains all of the functions necessary for the
operation of the debugger itself. We put this code in a
separate file to minimize the size of the temporary file.
We'll examine _bashdb.fns_ shortly.

Next, _bashdb.pre_ initializes a breakpoint array to empty
and execution tracing to off (see the following
discussion), then reads the original guinea pig script into
an array of lines. We need the source lines from the
original script for two reasons: to allow the debugger to
print out the script showing where the breakpoints are,
and to print out the lines of code as they execute if tracing
is turned on. You'll notice that we assign the script lines
to **_lines** from the environment variable **$REPLY** rather
than reading them into the array directly. This is because
**$REPLY** preserves any leading whitespace in the lines,
i.e., it preserves the indentation and layout of the original
script.

The last five lines of code set up the conditions necessary
for the debugger to begin working. The first **trap**
command sets up a clean-up routine that runs when the
fake signal EXIT occurs. The clean-up routine, normally
called when the debugger and guinea pig script finish,


just erases the temporary file. The next line sets the
variable **_steps** to 1 so that when the debugger is first
entered, it will stop after the first line.

The next line sets up the routine **_steptrap** to run when
the fake signal DEBUG occurs.

The built-in variable **LINENO** , which we saw earlier in
the chapter, is used to provide line numbers in the
debugger. However, if we just used **LINENO** as is, we'd
get line numbers above 30 because **LINENO** would be
including the lines in the preamble. To get around this,
we can pass **LINENO** minus the number of lines in the
preamble to the trap.[13]


## Debugger Functions

The function **_steptrap** is the entry point into the
debugger; it is defined in the file _bashdb.fns_. Here is
**_steptrap** :

```
# After each line of the test script is executed the shell traps to
# this function.
```
```
function _steptrap
{
_curline=$1 # the number of the line that just ran
```
```
(( $_trace )) && _msg "$PS4 line $_curline: ${_lines[$_curline]}"
```
```
if (( $_steps >= 0 )); then
let _steps="$_steps - 1"
fi
```
```
# First check to see if a line number breakpoint was reached.
# If it was, then enter the debugger.
if _at_linenumbp ; then
_msg "Reached breakpoint at line $_curline"
_cmdloop
```
```
# It wasn't, so check whether a break condition exists and is true.
# If it is, then enter the debugger.
elif [ -n "$_brcond" ] && eval $_brcond; then
_msg "Break condition $_brcond true at line $_curline"
_cmdloop
```
```
# It wasn't, so check if we are in step mode and the number of steps
# is up. If it is then enter the debugger.
```

```
elif (( $_steps == 0 )); then
_msg "Stopped at line $_curline"
_cmdloop
fi
}
```
**_steptrap** starts by setting **_curline** to the number of the
guinea pig line that just ran. If execution tracing is on, it
prints the PS4 execution trace prompt (like the shell's
**xtrace** mode), line number, and line of code itself. It then
decrements the number of steps if the number of steps
still left is greater than or equal to zero.

Then it does one of two things: it enters the debugger via
**_cmdloop** , or it returns so the shell can execute the next
statement. It chooses the former if a breakpoint or break
condition has been reached, or if the user stepped into this
statement.

### Commands

We will explain shortly how **_steptrap** determines these
things; now we will look at **_cmdloop**. It's a simple
combination of the case statements we saw inChapter 5,
and the calculator loop we saw in the previous chapter.

```
# The Debugger Command Loop
```
```
function _cmdloop {
local cmd args
```
```
while read -e -p "bashdb> " cmd args; do
case $cmd in
```

```
\? | h ) _menu ;; # print command menu
bc ) _setbc $args ;; # set a break condition
bp ) _setbp $args ;; # set a breakpoint at the given
# line
cb ) _clearbp $args ;; # clear one or all breakpoints
ds ) _displayscript ;; # list the script and show the
# breakpoints
g ) return ;; # "go": start/resume execution of
# the script
q ) exit ;; # quit
```
```
s ) let _steps=${args:-1} # single step N times
# (default = 1)
return ;;
x ) _xtrace ;; # toggle execution trace
!* ) eval ${cmd#!} $args ;; # pass to the shell
* ) _msg "Invalid command: '$cmd'" ;;
esac
done
}
```
At each iteration, **_cmdloop** prints a prompt, reads a
command, and processes it. We use **read -e** so that the
user can take advantage of the _readline_ command-line
editing. The commands are all one- or two-letter
abbreviations; quick for typing, but terse in the UNIX
style.[14]

Table 9-3summarizes the debugger commands.

**Table 9-3. bashdb commands**


**Command Action**

**bp** _N_ Set breakpoint at line _N_

bp List breakpoints and break condition

**bc** _string_ Set break condition to _string_

bc Clear break condition

**cb** _N_ Clear breakpoint at line _N_

cb Clear all breakpoints

ds Display the test script and breakpoints

g Start/resume execution

**s** [ _N_ ] Execute _N_ statements (default 1)


```
Command Action
```
```
x Toggle execution trace on/off
```
```
h ,? Print the help menu
```
```
! string Pass string to a shell
```
```
q Quit
```
Before looking at the individual commands, it is
important that you understand how control passes through
**_steptrap** , the command loop, and the guinea pig.

**_steptrap** runs after every statement in the guinea pig as
a result of the trap on DEBUG in the preamble. If a
breakpoint has been reached or the user previously typed
in a step command(s), **_steptrap** calls the command loop.
In doing so, it effectively "interrupts" the shell that is
running the guinea pig to hand control over to the user.

The user can invoke debugger commands as well as shell
commands that run in the same shell as the guinea pig.
This means that you can use shell commands to check
values of variables, signal traps, and any other


information local to the script being debugged. The
command loop continues to run, and the user stays in
control, until he types **g** , **q** , or **s**. We'll now look in detail
at what happens in each of these cases.

Typing **g** has the effect of running the guinea pig
uninterrupted until it finishes or hits a breakpoint. It
simply exits the command loop and returns to **_steptrap** ,
which exits as well. The shell then regains control and
runs the next statement in the guinea pig script. Another
DEBUG signal occurs and the shell traps to **_steptrap**
again. If there are no breakpoints then **_steptrap** will just
exit. This process will repeat until a breakpoint is reached
or the guinea pig finishes.

The **q** command calls the function **_cleanup** , which
erases the temporary file and exits the program.


### Stepping

When the user types **s** , the command loop code sets the
variable **_steps** to the number of steps the user wants to
execute, i.e., to the argument given. Assume at first that
the user omits the argument, meaning that **_steps** is set to

1. Then the command loop exits and returns control to
**_steptrap** , which (as above) exits and hands control back
to the shell. The shell runs the next statement and returns
to **_steptrap** , which then decrements **_steps** to 0. Then
the second **elif** conditional becomes true because **_steps**
is 0 and prints a "stopped" message and then calls the
command loop.

Now assume that the user supplies an argument to **s** , say

3. **_steps** is set to 3. Then the following happens:
    1. After the next statement runs, **_steptrap** is called
       again. It enters the first **if** clause, since **_steps** is
       greater than 0. **_steptrap** decrements **_steps** to 2
       and exits, returning control to the shell.
    2. This process repeats, another step in the guinea
       pig is run, and **_steps** becomes 1.
    3. A third statement is run and we're back in
       **_steptrap**. **_steps** is decremented to 0, the
       second **elif** clause is run, and **_steptrap** breaks
       out to the command loop again.


The overall effect is that the three steps run and then the
debugger takes over again.

All of the other debugger commands cause the shell to
stay in the command loop, meaning that the user prolongs
the "interruption" of the shell.


### Breakpoints

Now we'll examine the breakpoint-related commands and
the breakpoint mechanism in general. The **bp** command
calls the function **_setbp** , which can do two things,
depending on whether an argument is supplied or not.
Here is the code for **_setbp** :

```
# Set a breakpoint at the given line number or list breakpoints
function _setbp
{
local i
```
```
if [ -z "$1" ]; then
_listbp
elif [ $(echo $1 | grep '^[0-9]*') ]; then
if [ -n "${_lines[$1]}" ]; then
_linebp=($(echo $( (for i in ${_linebp[*]} $1; do
echo $i; done) | sort -n) ))
_msg "Breakpoint set at line $1"
else
_msg "Breakpoints can only be set on non-blank lines"
fi
else
_msg "Please specify a numeric line number"
fi
}
```
If no argument is supplied, **_setbp** calls **_listbp** , which
prints the line numbers that have breakpoints set. If
anything other than a number is supplied as an argument,
an error message is printed and control returns to the
command loop. Providing a number as the argument


allows us to set a breakpoint; however, we have to do
another test before doing so.

What happens if the user decides to set a breakpoint at a
nonsensical point: a blank line, or at line 1,000 of a
10-line program? If the breakpoint is set well beyond the
end of the program, it will never be reached and will
cause no problem. If, however, a breakpoint is set at a
blank line, it will cause problems. The reason is that the
DEBUG trap only occurs after each executed _simple
command_ in a script, not each line. Blank lines never
generate the DEBUG signal. The user could set a
breakpoint on a blank line, in which case continuing
execution with the **g** command would never break back
out to the debugger.

We can fix both of these problems by making sure that
breakpoints are set only on lines with text.[15] After
making the tests, we can add the breakpoint to the
breakpoint array, **_linebp**. This is a little more complex
than it sounds. In order to make the code in other sections
of the debugger simpler, we should maintain a sorted
array of breakpoints. To do this, we echo all of the line
numbers currently in the array, along with the new
number, in a subshell and pipe them into the UNIX **sort**
command. **sort -n** sorts a list into numerically ascending
order. The result of this is a list of ordered numbers which
we then assign back to the **_linebp** array with a
compound assignment.


To complement the user's ability to add breakpoints, we
also allow the user to delete them. The **cb** command
allows the user to clear single breakpoints or all
breakpoints, depending on whether a line number
argument is supplied or not. For example, **cb 12** clears a
breakpoint at line 12 (if a breakpoint was set at that line).
**cb** on its own would clear all of the breakpoints that have
been set. It is useful to look briefly at how this works;
here is the code for the function that is called with the **cb**
command, **_clearbp** :

```
function _clearbp
{
local i
```
```
if [ -z "$1" ]; then
unset _linebp[*]
_msg "All breakpoints have been cleared"
elif [ $(echo $1 | grep '^[0-9]*') ]; then
_linebp=($(echo $(for i in ${_linebp[*]}; do
if (( $1 != $i )); then echo $i; fi; done) ))
_msg "Breakpoint cleared at line $1"
else
_msg "Please specify a numeric line number"
fi
}
```
The structure of the code is similar to that used for setting
the breakpoints. If no argument was supplied to the
command, the breakpoint array is unset, effectively
deleting all the breakpoints. If an argument was supplied
and is not a number, we print out an error message and
exit.


A numeric argument to the **cb** command means the code
has to search the list of breakpoints and delete the
specified one. We can easily make the deletion by
following a procedure similar to the one we used when
we added a breakpoint in **_setbp**. We execute a loop in a
subshell, printing out the line numbers in the breakpoints
list and ignoring any that match the provided argument.
The echoed values once again form a compound
statement, which can then be assigned to an array
variable.[16]

The function **_at_linenumbp** is called by **_steptrap** after
every statement; it checks whether the shell has arrived at
a line number breakpoint. The code for the function is:

```
# See if this line number has a breakpoint
function _at_linenumbp
{
local i=0
```
```
if [ "$_linebp" ]; then
while (( $i < ${#_linebp[@]} )); do
if (( ${_linebp[$i]} == $_curline )); then
return 0
fi
let i=$i+1
done
fi
return 1
}
```
The function simply loops through the breakpoint array
and checks the current line number against each one. If a
match is found, it returns **true** (i.e., returns 0). Otherwise,


it continues looping, looking for a match until the end of
the array is reached. It then returns **false**.

It is possible to find out exactly what line the debugger is
up to and where the breakpoints have been set in the
guinea pig by using the **ds** command. We'll see an
example of the output later, when we run a sample
_bashdb_ debugging session. The code for this function is
fairly straightforward:

```
# Print out the shell script and mark the location of breakpoints
# and the current line
function _displayscript
{
local i=1 j=0 bp cl
```
```
( while (( $i < ${#_lines[@]} )); do
if [ ${_linebp[$j]} ] && (( ${_linebp[$j]} == $i )); then
bp='*'
let j=$j+1
else
bp=' '
fi
```
```
if (( $_curline == $i )); then
cl=">"
else
cl=" "
fi
```
```
echo "$i:$bp $cl ${_lines[$i]}"
let i=$i+1
done
) | more
}
```

This function contains a subshell, the output of which is
piped to the UNIX **more** command. We have done this
for user-friendly reasons; a long script would scroll up the
screen quickly and the users may not have displays that
allow them to scroll back to previous pages of screen
output. **more** displays one screenful of output at a time.

The core of the subshell code loops through the lines of
the guinea pig script. It first tests to see if the line it is
about to display is in the array of breakpoints. If it is, a
breakpoint character ( ***** ) is set and the local variable **j** is
incremented. **j** was initialized to 0 at the beginning of the
function; it contains the current breakpoint that we are up
to. It should now be apparent why we went to the trouble
of sorting the breakpoints in **_setbp** : both the line
numbers and the breakpoint numbers increment
sequentially, and once we pass a line number that has a
breakpoint and find it in the breakpoint array, we know
that future breakpoints in the script must be further on in
the array. If the breakpoint array contained line numbers
in a random order, we'd have to search the entire array to
find out if a line number was in the array or not.

The core of the subshell code then checks to see if the
current line and the line it is about to display are the
same. If they are, a "current line" character (>) is set. The
current displayed line number (stored in **i** ), breakpoint
character, current line character, and script line are then
printed out.


We think you'll agree that the added complexity in the
handling of breakpoints is well worth it. Being able to
display the script and the location of breakpoints is an
important feature in any debugger.


### Break conditions

_bashdb_ provides another method of breaking out of the
guinea pig script: the _break condition_. This is a string that
the user can specify that is evaluated as a command; if it
is true (i.e., returns exit status 0), the debugger enters the
command loop.

Since the break condition can be any line of shell code,
there's a lot of flexibility in what can be tested. For
example, you can break when a variable reaches a certain
value—e.g., **(( $x < 0 ))—** or when a particular piece of
text has been written to a file ( **grep** _string file_ ). You will
probably think of all kinds of uses for this feature.[17]To
set a break condition, type **bc** _string_. To remove it, type
**bc** without arguments—this installs the null string, which
is ignored.

**_steptrap** evaluates the break condition **$_brcond** only if
it's not null. If the break condition evaluates to 0, then the
**if** clause is true and, once again, **_steptrap** calls the
command loop.


### Execution tracing

The final feature of the debugger is _execution tracing_ ,
available with the **x** command.

The function **_xtrace** "toggles" execution tracing simply
by assigning to the variable **_trace** the logical "not" of its
current value, so that it alternates between 0 (off) and 1
(on). The preamble initializes it to 0.


### Debugger limitations

We have kept _bashdb_ reasonably simple so that you can
see the fundamentals of building a shell script debugger.
Although it contains some useful features and is designed
to be a real tool, not just a scripting example, it has some
important limitations. Some are described in the list that
follows.

1. Debuggers tend to run programs slower than if
    they were executed on their own. _bashdb_ is no
    exception. Depending upon the script you use it
    on, you'll find the debugger runs everything
    anywhere from 8 to 30 times more slowly. This
    isn't so much of a problem if you are stepping
    through a script in small increments, but bear it in
    mind if you have, say, initialization code with
    large looping constructs.
2. The debugger will not "step down" into shell
    scripts that are called from the guinea pig. To do
    this, you'd have to edit your guinea pig script and
    change a call to scriptname to **bashdb**
    scriptname.
3. Similarly, nested subshells are treated as one
    gigantic statement; you cannot step down into
    them at all.


4. The guinea pig itself should not trap on the fake
    signals DEBUG and EXIT; otherwise the
    debugger won't work.
5. Command error handling could be significantly
    improved.

Many of these are not insurmountable and you can
experiment with solving them yourself; see the exercises
at the end of this chapter.

The debugger from an earlier version of this book helped
inspire a more comprehensive _bash_ debugger maintained
by Rocky Bernstein, which you can find at the Bash
Debugger Project, _[http://bashdb.sourceforge.net/](http://bashdb.sourceforge.net/)_.


## A Sample bashdb Session

Now we'll show a transcript of an actual session with
_bashdb_ , in which the guinea pig is the solution to Task
6-1, the script _ndu_. Here is the transcript of the debugging
session:

```
[bash]$ bashdb ndu
bash Debugger version 1.0
Stopped at line 0
bashdb> ds
1: for dir in ${*:-.}; do
2: if [ -e $dir ]; then
3: result=$(du -s $dir | cut -f 1)
4: let total=$result*1024
5:
6: echo -n "Total for $dir = $total bytes"
7:
8: if [ $total -ge 1048576 ]; then
9: echo " ($((total/1048576)) Mb)"
10: elif [ $total -ge 1024 ]; then
11: echo " ($((total/1024)) Kb)"
12: fi
13: fi
14: done
bashdb> s
Stopped at line 2
bashdb> bp 4
Breakpoint set at line 4
bashdb> bp 8
Breakpoint set at line 8
bashdb> bp 11
```

Breakpoint set at line 11
bashdb> **ds**
1: for dir in ${*:-.}; do
2: > if [ -e $dir ]; then
3: result=$(du -s $dir | cut -f 1)
4:* let total=$result*1024
5:
6: echo -n "Total for $dir = $total bytes"
7:
8:* if [ $total -ge 1048576 ]; then
9: echo " ($((total/1048576)) Mb)"
10: elif [ $total -ge 1024 ]; then
11:* echo " ($((total/1024)) Kb)"
12: fi
13: fi
14: done
bashdb> **g**
Reached breakpoint at line 4
bashdb> **!echo $total**
6840032
bashdb> _cb 8_
Breakpoint cleared at line 8
bashdb> **ds**
1: for dir in ${*:-.}; do
2: if [ -e $dir ]; then
3: result=$(du -s $dir | cut -f 1)
4:* > let total=$result*1024
5:
6: echo -n "Total for $dir = $total bytes"
7:
8: if [ $total -ge 1048576 ]; then
9: echo " ($((total/1048576)) Mb)"
10: elif [ $total -ge 1024 ]; then
11:* echo " ($((total/1024)) Kb)"
12: fi
13: fi
14: done


```
bashdb> bp
Breakpoints at lines: 4 11
Break on condition:
```
```
bashdb> !total=5600
bashdb> g
Total for. = 5600 bytes (5 Kb)
Reached breakpoint at line 11
bashdb> cb
All breakpoints have been cleared
bashdb> ds
1: for dir in ${*:-.}; do
2: if [ -e $dir ]; then
3: result=$(du -s $dir | cut -f 1)
4: let total=$result*1024
5:
6: echo -n "Total for $dir = $total bytes"
7:
8: if [ $total -ge 1048576 ]; then
9: echo " ($((total/1048576)) Mb)"
10: elif [ $total -ge 1024 ]; then
11: > echo " ($((total/1024)) Kb)"
12: fi
13: fi
14: done
bashdb> g
[bash]$
```
First, we display the script with **ds** and then perform a
step, taking execution to line 2 of _ndu_. We then set
breakpoints at lines 4, 8, and 11 and display the script
again. This time the breakpoints are clearly marked by
asterisks ( ***** ). The right angle bracket (>) indicates that
line 2 was the most recent line executed.


Next, we continue execution of the script that breaks at
line 4. We print out the value of **total** now and decide to
clear the breakpoint at line 8. Displaying the script
confirms that the breakpoint at line 8 is indeed gone. We
can also use the **bp** command, and it too shows that the
only breakpoints set are at lines 4 and 11.

At this stage we might decide that we want to check the
logic of the **if** branch at line 11. This requires that **$total**
be greater than or equal to 1,024, but less than 1,048,576.
As we saw previously, **$total** is very large, so we set its
value to 5,600 so that it will execute the second part of
the **if** and continue execution. The script enters that
section of the **if** correctly, prints out the value, and stops
at the breakpoint.

To finish off, we clear the breakpoints, display the script
again, and then continue execution, which exits the script.


## Exercises

We'll conclude this chapter with some suggested
enhancements to our simple debugger and a complete
listing of the debugger command source code.

1. Improve command error handling in these ways:
    1. Check that the arguments to **s** are valid
       numbers and print an appropriate error
       message if they aren't.
    2. Check that a breakpoint actually exists
       before clearing it and warn the user if the
       line doesn't have a breakpoint.
    3. Any other error handling that you can
       think of.
2. Add code to remove duplicate breakpoints (more
    than one breakpoint on one line).
3. Enhance the **cb** command so that the user can
    specify more than one breakpoint to be cleared at
    a time.
4. Implement an option that causes a break into the
    debugger whenever a command exits with
    non-zero status:


1. Implement it as the command-line option
    **-e**.
2. Implement it as the debugger command **e**
    to toggle it on and off. (Hint: when you
    enter **_steptrap** , **$?** is still the exit status
    of the last command that ran.)
5. Implement a command that prints out the status
of the debugger: whether execution trace is on/
off, error exit is on/off, and the number of the last
line to be executed. In addition, move the
functionality for displaying the breakpoints from
**bp** to the new option.
6. Add support for multiple break conditions, so
that _bashdb_ stops execution whenever one of
them becomes true and prints a message
indicating which one became true. Do this by
storing the break conditions in an array. Try to
make this as efficient as possible, since the
checking will take place after every statement.
7. Add the ability to watch variables.
1. Add a command **aw** that takes a variable
name as an argument and adds it to a list
of variables to watch. Any watched
variables are printed out when execution
trace is toggled on.


2. Add another command **cw** that, without
    an argument, removes all of the variables
    from the watch list. With an argument, it
    removes the specified variable.
8. Although placing an underscore at the start of the
debugger identifiers will avoid name clashes in
most cases, think of ways to automatically detect
name clashes with the guinea pig script and how
to get around this problem. (Hint: you could
rename the clashing names in the guinea pig
script at the point where it gets combined with
the preamble and placed in the temporary file.)
9. Add any other features you can think of.

Finally, here is a complete source listing of the debugger
function file _bashdb.fns_ :

```
# After each line of the test script is executed the shell traps to
# this function.
```
```
function _steptrap
{
_curline=$1 # the number of the line that just ran
```
```
(( $_trace )) && _msg "$PS4 line $_curline: ${_lines[$_curline]}"
```
```
if (( $_steps >= 0 )); then
let _steps="$_steps - 1"
fi
```
```
# First check to see if a line number breakpoint was reached.
# If it was, then enter the debugger.
```

```
if _at_linenumbp ; then
_msg "Reached breakpoint at line $_curline"
_cmdloop
```
```
# It wasn't, so check whether a break condition exists and is true.
# If it is, then enter the debugger
elif [ -n "$_brcond" ] && eval $_brcond; then
_msg "Break condition $_brcond true at line $_curline"
_cmdloop
```
```
# It wasn't, so check if we are in step mode and the number of
# steps is up. If it is, then enter the debugger.
elif (( $_steps == 0 )); then
_msg "Stopped at line $_curline"
_cmdloop
```
fi
}

# The Debugger Command Loop

function _cmdloop {
local cmd args

```
while read -e -p "bashdb> " cmd args; do
case $cmd in
\? | h ) _menu ;; # print command menu
bc ) _setbc $args ;; # set a break condition
bp ) _setbp $args ;; # set a breakpoint at the given line
cb ) _clearbp $args ;; # clear one or all breakpoints
ds ) _displayscript ;; # list the script and show the
# breakpoints
g ) return ;; # "go": start/resume execution of
# the script
q ) exit ;; # quit
s ) let _steps=${args:-1} # single step N times (default = 1)
return ;;
```

x ) _xtrace ;; # toggle execution trace
* ) eval ${cmd#!} $args ;; # pass to the shell
* ) _msg "Invalid command: '$cmd'" ;;
esac
done
}

# See if this line number has a breakpoint
function _at_linenumbp
{
local i=0

```
# Loop through the breakpoints array and check to see if any of
# them match the current line number. If they do return true (0)
# otherwise return false.
```
if [ "$_linebp" ]; then
while (( $i < ${#_linebp[@]} )); do
if (( ${_linebp[$i]} == $_curline )); then
return 0
fi
let i=$i+1
done
fi
return 1
}

# Set a breakpoint at the given line number or list breakpoints
function _setbp
{
local i

```
# If there are no arguments call the breakpoint list function.
# Otherwise check to see if the argument was a positive number.
# If it wasn't then print an error message. If it was then check
```

```
# to see if the line number contains text. If it doesn't then
# print an error message. If it does then echo the current
# breakpoints and the new addition and pipe them to "sort" and
# assign the result back to the list of breakpoints. This results
# in keeping the breakpoints in numerical sorted order.
```
```
# Note that we can remove duplicate breakpoints here by using
# the -u option to sort which uniquifies the list.
```
if [ -z "$1" ]; then
_listbp
elif [ $(echo $1 | grep '^[0-9]*') ]; then
if [ -n "${_lines[$1]}" ]; then
_linebp=($(echo $( (for i in ${_linebp[*]} $1; do
echo $i; done) | sort -n) ))
_msg "Breakpoint set at line $1"
else
_msg "Breakpoints can only be set on non-blank lines"
fi
else
_msg "Please specify a numeric line number"
fi
}

# List breakpoints and break conditions
function _listbp
{
if [ -n "$_linebp" ]; then
_msg "Breakpoints at lines: ${_linebp[*]}"
else
_msg "No breakpoints have been set"
fi

_msg "Break on condition:"
_msg "$_brcond"
}


# Clear individual or all breakpoints
function _clearbp
{
local i bps

```
# If there are no arguments, then delete all the breakpoints.
# Otherwise, check to see if the argument was a positive number.
# If it wasn't, then print an error message. If it was, then
# echo all of the current breakpoints except the passed one
# and assign them to a local variable. (We need to do this because
# assigning them back to _linebp would keep the array at the same
# size and just move the values "back" one place, resulting in a
# duplicate value). Then destroy the old array and assign the
# elements of the local array, so we effectively recreate it,
# minus the passed breakpoint.
```
if [ -z "$1" ]; then
unset _linebp[*]
_msg "All breakpoints have been cleared"
elif [ $(echo $1 | grep '^[0-9]*') ]; then
bps=($(echo $(for i in ${_linebp[*]}; do
if (( $1 != $i )); then echo $i; fi; done) ))
unset _linebp[*]
_linebp=(${bps[*]})
_msg "Breakpoint cleared at line $1"
else
_msg "Please specify a numeric line number"
fi
}

# Set or clear a break condition
function _setbc
{
if [ -n "$*" ]; then
_brcond=$args


_msg "Break when true: $_brcond"
else
_brcond=
_msg "Break condition cleared"
fi
}

# Print out the shell script and mark the location of breakpoints
# and the current line

function _displayscript
{
local i=1 j=0 bp cl

( while (( $i < ${#_lines[@]} )); do
if [ ${_linebp[$j]} ] && (( ${_linebp[$j]} == $i )); then
bp='*'
let j=$j+1
else
bp=' '
fi
if (( $_curline == $i )); then
cl=">"
else
cl=" "
fi
echo "$i:$bp $cl ${_lines[$i]}"
let i=$i+1
done
) | more
}

# Toggle execution trace on/off
function _xtrace
{


let _trace="! $_trace"
_msg "Execution trace "
if (( $_trace )); then
_msg "on"
else
_msg "off"
fi
}

# Print the passed arguments to Standard Error
function _msg
{
echo -e "$@" >&2
}

# Print command menu
function _menu {
_msg 'bashdb commands:
bp N set breakpoint at line N
bp list breakpoints and break condition
bc string set break condition to string
bc clear break condition
cb N clear breakpoint at line N
cb clear all breakpoints
ds displays the test script and breakpoints
g start/resume execution
s [N] execute N statements (default 1)
x toggle execution trace on/off
h,? print this menu
! string passes string to a shell
q quit'
}

# Erase the temporary file before exiting


```
function _cleanup
{
rm $_debugfile 2>/dev/null
}
```
[10] Unfortunately, the debugger will not work with

versions of _bash_ prior to 2.0, because they do not
implement the DEBUG signal.

[11]All function names and variables (except those local

to functions) in _bashdb_ have names beginning with an
underscore (_), to minimize the possibility of clashes with
names in the guinea pig script.

[12] **exec** can also be used with an I/O redirector only; this

makes the redirector take effect for the remainder of the
script or login session. For example, the line **exec
2>errlog** at the top of a script directs standard error to the
file _errlog_ for the rest of the script.

[13]If you are typing or scanning in the preamble code

from this book, make sure that the last line in the file is
the call to set the trap, i.e., no blank lines should appear
after the call to **trap**.

[14] There is nothing to stop you from changing the

commands to something you find easier to remember.
There is no "official" _bash_ debugger, so feel free to
change the debugger to suit your needs.


[15]This isn't a complete solution. Certain other lines

(e.g., comments) will also be ignored by the DEBUG
trap. See the list of limitations and the exercises at the end
of this chapter.

[16] _bash_ versions 2.01 and earlier have a bug in assigning

arrays to themselves that prevents the code for **setbp** and
**clearbp** from working. In each case, you can get around
this bug by assigning **_linebp** to a local variable first,
**unset** ting it, and then assigning the local variable back to
it. Better yet, update to a more recent version of _bash_.

[17] Bear in mind that if your break condition sends

anything to standard output or standard error, you will see
it after every statement executed. Also, make sure your
break condition doesn't take a long time to run; otherwise
your script will run very, very slowly.


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


