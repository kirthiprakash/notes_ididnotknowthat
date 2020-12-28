# $HISTCONTROL

If a command in bash shell starts with a space, and `$HISTCONTROL` is set to `ignorespace` bash will not store this command as part of the history. This is very useful if some commands include sensitive data.

`ignoredups` ignores duplicate commands

`ignoreboth` ignores both duplicate commands and commands starting with space

