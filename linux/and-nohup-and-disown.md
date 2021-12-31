---
description: https://unix.stackexchange.com/a/148698
---

# & nohup and disown



Let's first look at what happens if a program is started from an interactive shell (connected to a terminal) without `&` (and without any redirection). So let's assume you've just typed `foo`:

* The process running `foo` is created.
* The process inherits stdin, stdout, and stderr from the shell. Therefore it is also connected to the same terminal.
* If the shell receives a `SIGHUP`, it also sends a `SIGHUP` to the process (which normally causes the process to terminate).
* Otherwise the shell waits (is blocked) until the process terminates or gets stopped.

Now, let's look what happens if you put the process in the background, that is, type `foo &`:

* The process running `foo` is created.
* The process inherits stdout/stderr from the shell (so it still writes to the terminal).
* The process in principle also inherits stdin, but as soon as it tries to read from stdin, it is halted.
* It is put into the list of background jobs the shell manages, which means especially:
  * It is listed with `jobs` and can be accessed using `%n` (where `n` is the job number).
  * It can be turned into a foreground job using `fg`, in which case it continues as if you would not have used `&` on it (and if it was stopped due to trying to read from standard input, it now can proceed to read from the terminal).
  * If the shell received a `SIGHUP`, it also sends a `SIGHUP` to the process. Depending on the shell and possibly on options set for the shell, when terminating the shell it will also send a `SIGHUP` to the process.

Now `disown` removes the job from the shell's job list, so all the subpoints above don't apply any more (including the process being sent a `SIGHUP` by the shell). However note that it _still_ is connected to the terminal, so if the terminal is destroyed (which can happen if it was a pty, like those created by `xterm` or `ssh`, and the controlling program is terminated, by closing the xterm or terminating the [SSH](http://en.wikipedia.org/wiki/Secure\_Shell) connection), the program will fail as soon as it tries to read from standard input or write to standard output.

What `nohup` does, on the other hand, is to effectively separate the process from the terminal:

* It closes standard input (the program will _not_ be able to read any input, even if it is run in the foreground. it is not halted, but will receive an error code or `EOF`).
* It redirects standard output and standard error to the file `nohup.out`, so the program won't fail for writing to standard output if the terminal fails, so whatever the process writes is not lost.
* It prevents the process from receiving a `SIGHUP` (thus the name).

Note that `nohup` does _not_ remove the process from the shell's job control and also doesn't put it in the background (but since a foreground `nohup` job is more or less useless, you'd generally put it into the background using `&`). For example, unlike with `disown`, the shell will still tell you when the nohup job has completed (unless the shell is terminated before, of course).

So to summarize:

* `&` puts the job in the background, that is, makes it block on attempting to read input, and makes the shell not wait for its completion.
* `disown` removes the process from the shell's job control, but it still leaves it connected to the terminal. One of the results is that the shell won't send it a `SIGHUP`. Obviously, it can only be applied to background jobs, because you cannot enter it when a foreground job is running.
* `nohup` disconnects the process from the terminal, redirects its output to `nohup.out` and shields it from `SIGHUP`. One of the effects (the naming one) is that the process won't receive any sent `SIGHUP`. It is completely independent from job control and could in principle be used also for foreground jobs (although that's not very useful).
