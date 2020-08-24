# Monitoring logs on a different terminal device

Linux terminals allows messages to be passed to each other. The below link talks more about it.

{% page-ref page="../linux/message-an-user-at-terminal-device.md" %}

This functionality can be used to monitor logs of an application running on a specific terminal.

When developing code with hot reload functionality, I often found myself switching to different terminal to see the output. 

Suppose, we are edting the code in a terminal editor say, Vim,  at terminal 1, and we are running a linter on terminal 2 which checks for linting errors on any new file changes. On every file save, we have to switch to the 2nd terminal to check for any new liniting errors. Most of the time, there won't be any errors and we just want to confirm that there were zero errors. It would be nice to just get a glimpse of the last few lines without switching the terminal. If the last few lines says it has errors, then we switch terminals to see more detailed error output. If it prints "zero errors found", no need to swtich.

### "tee"ing terminal 2 output to a tmux split window at terminal 1

We shall split teminal 1, into two window panes using tmux, one for code editing and the other \(probably a small dimension\) for watching the last 3 lines of the linter output on terminal 2.

```text
Terminal one
========= tmux split window 1 ========
|                                    |
|                                    |
|                                    |
|            code editor             |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
|                                    |
---------- tmux split window 2-------
|# tty                               |
|# /dev/pts/1                        |
|# starting jslint with hotreload..  |
======================================
```

```text
Terminal two
# ./jslint --hotreload | tee /dev/pts/1
# starting jslint with hotreload..
```

By using the messaging feature, we have to just `tee` the output of the linter to the terminal device identifier. `tee` makes sure the linter output is printed on the current stdout and also to the other terminal device

