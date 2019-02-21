# Homework 3 - A Simple Shell Pt. 2 #
## Due: 6 March at 11:00 PM ##
### Introduction ###
In this assignment we will build on the shell you submitted for Homework 2. The learning objectives for this assignment are. 
* Demonstraing knowledge of Linux signal handling best practices including those outlined in Bryant & O'Hallaron ch 8.5.5
* Gaining more familiarity with the system calls in unistd.h by implementing file redirection. 
* Combining the above to run background jobs and bring them to the foreground.

### Signal Handling ###
Recommended Reading [Signal Man Page](http://man7.org/linux/man-pages/man7/signal.7.html), GNU Manual on Signal Handling ftp://ftp.gnu.org/old-gnu/Manuals/glibc-2.2.3/html_chapter/libc_24.html  
Your job is to handle the following signals as described:
* SIGINT
  * This signal is usually sent when the user enters Ctrl-c. When your shell receives SIGINT it should print the following:
  ```BASH
  Caught: Interrupt
  ```
  * exit normally. 
* SIGCHLD
  * This signal is sent to the parent when one of its child processes terminates or stops. Move your implementation of waitpid() into the SIGCHLD handler to print:
  ```BASH
  pid:XXXXXX status:X
  ```
* SIGTERM
  * SIGTERM is generated when kill pid is sent from the command line. 
  * Send SIGTERM to all children and exit this process normally. 
  * print to stdout
  ```BASH
  Caught: Terminated
  ```
* SIGTSTP
  * This is the signal sent by pressing Ctrl-z on the keyboard. 
  * Pause program execution.
  
* SIGCONT
  * Resume process execution if stopped, otherwise ignore. 
  
### IO Redirection ###
Recommended reading: [BASH man page](https://linux.die.net/man/1/bash), [A dup2() tutorial](https://www.cs.rutgers.edu/~pxk/416/notes/c-tutorials/dup2.html)  
A pipe is a special kind of file type where the first data written is the first data read. Pipes are great because they allow us to set up pipelines like this:
```BASH
CS361 > command1 | command2 | command3
```
Where the output of command1 is the input to command2 and the output of command2 is the input to command3 and command3 outputs to stdout. You may have even used pipes before. For instance, you can sort all the output of ls using sort
```Bash
CS361 > ls -al | sort
```
Which takes the output of ps and writes it to a pipe. Grep reads from the pipe and then prints its output to stdout. 
We don't always want to print to stdout or read from stdin. In this case we can Redirect Input and Output. This is done with the >, >>, and < commands. 
* Redirect output - output that would have printed to stdout is printed to the file instead. 
  * command > output.txt saves the output of command into a file called output.txt
  * command >> output.txt appends the output of command onto the file called output.txt
* Redirect input - Input that would have read from stdin is read from a file
  * command < input.txt command gets its input from input.txt. 
  
Your job is to implement |, <, >, and >>. 

### Background Jobs ###
Recommended reading [Job control in BASH](https://www.digitalocean.com/community/tutorials/how-to-use-bash-s-job-control-to-manage-foreground-and-background-processes)  
Another shell feature is the ability to run jobs in the background. A job is initialized in the background by appending an ampersand, &, to the end of a command. For example: 
```BASH
CS361 > emacs hw3.c &
```
In this command the text editor emacs is started with hw3.c ready to edit, but its in the background. To bring it to the foreground type: 
```BASH
CS361 > fg
```
To send the editor back to the background type Ctrl-z. 
A background job generating output to stdout will typically interleave output with the foreground program. A background job attempting to read from the terminal will be sent SIGTTIN. The job should wait until it is brought into the foreground with SIGCONT before attempting the read. 

Your job is to implement &, fg, and properly handle signals SIGTTOUT, SIGTTIN

### Administration ###
Submission will be on gradescope. If there is an autograder it will have limited submissions to discourage guess-and-check programming. Submit a makefile with target hw3. Your shell executes with the command ./hw3. 

You will get one point each for implementing the 5 signal handlers, the pipe, the 3 redirects, &, fg, and SIGGTTIN for a total of 12 points. 

There is no code stub to begin
