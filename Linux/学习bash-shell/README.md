**Table of Contents**

## 1. bash Basics

- 1.1. What Is a Shell?
- 1.2. Scope of This Book
- 1.3. History of UNIX Shells
  - 1.3.1. The Bourne Again Shell
  - 1.3.2. Features of bash**
- 1.4. Getting bash
- 1.5. Interactive Shell Use
  - 1.5.1. Commands, Arguments, and Options**
- 1.6. Files
  - 1.6.1. Directories
  - 1.6.2. Filenames, Wildcards, and Pathname Expansion
  - 1.6.3. Brace Expansion**
- 1.7. Input and Output
  - 1.7.1. Standard I/O
  - 1.7.2. I/O Redirection
  - 1.7.3. Pipelines
- 1.8. Background Jobs
  - 1.8.1. Background I/O
  - 1.8.2. Background Jobs and Priorities
- 1.9. Special Characters and Quoting
  - 1.9.1. Quoting
  - 1.9.2. Backslash-Escaping
  - 1.9.3. Quoting Quotation Marks
  - 1.9.4. Continuing Lines
  - 1.9.5. Control Keys
- 1.10. Help

## 2. Command-Line Editing

- 2.1. Enabling Command-Line Editing
- 2.2. The History List**
- 2.3. emacs Editing Mode**
  - 2.3.1. Basic Commands**
  - 2.3.2. Word Commands**
  - 2.3.3. Line Commands**
  - 2.3.4. Moving Around in the History List**
  - 2.3.5. Textual Completion**
  - 2.3.6. Miscellaneous Commands**
- 2.4. vi Editing Mode
  - 2.4.1. Simple Control Mode Commands
  - 2.4.2. Entering and Changing Text
  - 2.4.3. Deletion Commands
  - 2.4.4. Moving Around in the History List
  - 2.4.5. Character-Finding Commands
  - 2.4.6. Textual Completion
  - 2.4.7. Miscellaneous Commands
- 2.5. The fc Command
- 2.6. History Expansion
- 2.7. readline
  - 2.7.1. The readline Startup File
  - 2.7.2. Key Bindings Using bind
- 2.8. Keyboard Habits

## 3. Customizing Your Environment
- 3.1. The .bash_profile, .bash_logout, and .bashrc Files
- 3.2. Aliases
- 3.3. Options
  - 3.3.1. shopt
- 3.4. Shell Variables
  - 3.4.1. Variables and Quoting
  - 3.4.2. Built-In Variables
- 3.5. Customization and Subprocesses
  - 3.5.1. Environment Variables
  - 3.5.2. The Environment File
- 3.6. Customization Hints

## 4. Basic Shell Programming**
- 4.1. Shell Scripts and Functions
  - 4.1.1. Functions
- 4.2. Shell Variables
  - 4.2.1. Positional Parameters
  - 4.2.2. Local Variables in Functions
  - 4.2.3. Quoting with $@ and $*
  - 4.2.4. More on Variable Syntax
- 4.3. String Operators
  - 4.3.1. Syntax of String Operators
  - 4.3.2. Patterns and Pattern Matching
  - 4.3.3. Length Operator
  - 4.3.4. Extended Pattern Matching
- 4.4. Command Substitution
- 4.5. Advanced Examples: pushd and popd

## 5. Flow Control** 
- 5.1. if/else**
  - 5.1.1. Exit Status**
  - 5.1.2. Return**
  - 5.1.3. Combinations of Exit Statuses**
  - 5.1.4. Condition Tests**
  - 5.1.5. Integer Conditionals**
- 5.2. for
- 5.3. case
- 5.4. select
- 5.5. while and until
  
# 6. Command-Line Options and Typed Variables**
- 6.1. Command-Line Options
  - 6.1.1. shift
  - 6.1.2. Options with Arguments
  - 6.1.3. getopts
- 6.2. Typed Variables
- 6.3. Integer Variables and Arithmetic
  - 6.3.1. Arithmetic Conditionals
  - 6.3.2. Arithmetic Variables and Assignment
  - 6.3.3. Arithmetic for Loops
- 6.4. Arrays

# 7. Input/Output and Command-Line Processing**
- 7.1. I/O Redirectors**
  - 7.1.1. Here-documents**
  - 7.1.2. File Descriptors**
- 7.2. String I/O
  - 7.2.1. echo
  - 7.2.2. printf
  - 7.2.3. read
- 7.3. Command-Line Processing
  - 7.3.1. Quoting
  - 7.3.2. command, builtin, and enable
  - 7.3.3. eval

# 8. Process Handling**
- 8.1. Process IDs and Job Numbers
- 8.2. Job Control
  - 8.2.1. Foreground and Background
  - 8.2.2. Suspending a Job
- 8.3. Signals
  - 8.3.1. Control-Key Signals
  - 8.3.2. kill
  - 8.3.3. ps
- 8.4. trap
  - 8.4.1. Traps and Functions
  - 8.4.2. Process ID Variables and Temporary Files
  - 8.4.3. Ignoring Signals
  - 8.4.4. disown
  - 8.4.5. Resetting Traps
- 8.5. Coroutines
  - 8.5.1. wait
  - 8.5.2. Advantages and Disadvantages of Coroutines
  - 8.5.3. Parallelization
- 8.6. Subshells
  - 8.6.1. Subshell Inheritance
  - 8.6.2. Nested Subshells
- 8.7. Process Substitution

## 9. Debugging Shell Programs**
- 9.1. Basic Debugging Aids
  - 9.1.1. Set Options
  - 9.1.2. Fake Signals
  - 9.1.3. Debugging Variables
- 9.2. A bash Debugger
  - 9.2.1. Structure of the Debugger
  - 9.2.2. The Preamble
  - 9.2.3. Debugger Functions
  - 9.2.4. A Sample bashdb Session
  - 9.2.5. Exercises
  

## 10. bash Administration**
- 10.1. Installing bash as the Standard Shell**
  - 10.1.1. POSIX Mode**
  - 10.1.2. Command-Line Options**
- 10.2. Environment Customization
  - 10.2.1. umask
  - 10.2.2. ulimit
  - 10.2.3. Types of Global Customization
- 10.3. System Security Features
  - 10.3.1. Restricted Shell
  - 10.3.2. A System Break-In Scenario
  - 10.3.3. Privileged Mode

## 11. Shell Scripting**
- 11.1. What's That Do?
  - 11.1.1. Comments
  - 11.1.2. Variables and Constants
11.2. Starting Up
11.3. Potential Problems
11.4. Don't Use bash
```
**12. bash for Your System**
    **12.1. Obtaining bash**
    **12.2. Unpacking the Archive**
    **12.3. What's in the Archive**
       **12.3.1. Documentation**
       **12.3.2. Configuring and Building bash**
       **12.3.3. Testing bash**
       **12.3.4. Potential Problems**
       **12.3.5. Installing bash as a Login Shell**
       **12.3.6. Examples**

```
12.4. Who Do I Turn to?
12.4.1. Asking Questions
12.4.2. Reporting Bugs
```
**A. Related Shells**


```
A.1. The Bourne Shell
A.2. The IEEE 1003.2 POSIX Shell Standard
A.3. The Korn Shell
A.4. pdksh
A.5. zsh
A.6. Shell Clones and Unix-like Platforms
A.6.1. Cygwin
A.6.2. DJGPP
A.6.3. MKS Toolkit
A.6.4. AT&T UWIN
```
**B. Reference Lists
B.1. Invocation
B.2. Prompt String Customizations
B.3. Built-In Commands and Reserved Words
B.4. Built-In Shell Variables
B.5. Test Operators
B.6. set Options
B.7. shopt Options
B.8. I/O Redirection
B.9. emacs Mode Commands
B.10. vi Control Mode Commands**

**C. Loadable Built-Ins
D. Programmable Completion**
