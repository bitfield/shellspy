# ShellSpy

This is a Go project template, intended for students at the [Bitfield Institute of Technology](https://bitfieldconsulting.com/golang/bit)—but everyone's welcome to try it if you're interested in learning or practicing Go.

## Description

The aim of this project is to produce a Go library package and accompanying command-line tool that will record a user's shell session to a text file, like a simple version of the Unix [`script`](https://man7.org/linux/man-pages/man1/script.1.html) command.

For example, you might use it like this:

```
./shellspy
Recording session to 'shellspy.txt'

> ls
cmd go.mod README.md shellspy.go shellspy_test.go

> echo hello
hello

> exit
Session saved as 'shellspy.txt'
```

In other words, when you run it, it gives you a prompt. You can type commands at this prompt and they will be run, and you'll see the output. When you exit the session, a complete transcript will be stored in the file `shellspy.txt`:

```
> ls
cmd go.mod README.md shellspy.go shellspy_test.go

> echo hello
hello

> exit
```

## Getting started

The main thing you'll need for this project is the standard [`os/exec`](https://pkg.go.dev/os/exec) package. This will let you run external commands from a Go program and get the results. Play around with this a little until you're happy you know how to run external commands using `exec`.

## Goal 1: creating a command from a string

Suppose you read a line of input from the user. You then need to turn that string into an `*exec.Cmd` object that can be run. That sounds like something testable, so see if you can write a test for some function like:

```go
func CommandFromString(input string) (*exec.Cmd, error)
```

Once you've got the test failing correctly, go ahead and implement the function.

## Goal 2: looping and reading input

You'll also need some function that can loop, reading a line of input at a time, until the user types `exit`. Don't worry about what to do with the input for now; just see if you can write a _test_ for such a function.

How would you call that function from a test? How could you supply simulated "user" input to it? How could you check that it's _looping_, rather than just reading the first line and returning?

Once you have the test working, implement the function.

## Goal 3: executing a command and getting its output

Now you can read lines from the user and turn them into `exec.Cmd` objects, you need some function that actually _runs_ the external command and returns its output. See if you can work out how to test it. Then implement it.

## Goal 4: a basic shell

You now have three useful bits of machinery:

1. Looping and reading
2. Parsing input lines into commands
3. Executing commands and getting output

Can you figure out how to put them together to make a basic shell? Don't worry about the transcript recording for now; just see if you can put these bits together in a `main.go` to make a little shell that you can run and play with.

## Goal 5: transcript recording

Since you have the user's input lines, and the commands' output lines, all you need to do now is send them both somewhere! How could you test that? Suppose you wrote some test that creates a shell session, sends it some simulated input (a couple of commands, let's say), and then gets the transcript.

Since you're supplying the input, you can work out in advance exactly what the transcript should be, can't you? See if you can write this test. Then make it pass.

Once you've done this, and tied it up together into a neat CLI tool, you're basically there.

Bear in mind that if someone wants to _use_ your package in their own program, as opposed to just running your binary, they should have access to all the important behaviour. For example, the remote shell server described below. It won't be able to use anything in your `main` package, so things like the reading loop can't be in there, can they? It all needs to be in the `shellspy` package, and the job of `main` should just be to create the "session" and run it.

## Stretch goals

For extra credit, use your package to implement a _remote_ shell. In other words, a server that listens on some network port. When it receives a connection, it starts a shell session. Everything it reads from the connection will be interpreted as a command line and executed. The output will be sent back to the connection.

You don't need a special client program on the other end: [`nc`](https://linux.die.net/man/1/nc) will do just fine.

It's probably a good idea to add some kind of authentication, so that random internet people can't just run arbitrary commands on your machine! (Hopefully you have a firewall too, of course, but a password would be a good idea too.)
