---
description: 'ref: https://unix.stackexchange.com/a/21294'
---

# Message an user at terminal device

Linux provides terminals to interact with the operating systems. It could be a physical terminal \(tty\) or a psuedo terminal \(pty\) which is emulated by programs like xterm, ssh etc. Each terminal is identified by character based device file located at `/dev/***`. A physical terminal can be addressed by `/dev/tty1` and a pseudo terminal can be addressed by `/dev/pts/12.`

> pts is a slave part of pty which interacts with the shell

The command `tty` outputs the current terminal device identifier

```text
On a physical terminal
> tty
> /dev/tty1

On a pseudo terminal
> tty
> /dev/pts/2
```

Terminal devices have messaging capabilities among themselves. Anything written to the the device file will be visible to the user at the corresponding terminal device.

```text
Terminal 1
user1@devbox:$ echo "hello cat" > /dev/pts/2
```

```text
Terminal 2
user2@devbox> tty
user2@devbox> /dev/pts/2
user2@devbox> hello cat
```

### The _write_ command

`write` command is interactive utility which makes it easier to send messages to different users on different terminal devices.

```text
user1@devbox> write user2 /dev/pts2
Good evening user2!
<EOF> or <Interupt signal> to quit write command
```

```text
user2@devbox> 
Message from user1@tdevbox on pts/13 at 13:14
Good evening user2!
EOF
```



