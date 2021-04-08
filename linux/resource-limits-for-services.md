# Resource limits for Services

The ability to write core dumps is dictated by resource limits set on that process. If the resource limits are not passed while starting a process, it inherits from its parent's process. Linux systems provide a tool \`ulimit\` to set or report the limits of the current user. A system administrator may also set these limits in the /etc/security/limits.conf.

The man page for limits.conf \([https://linux.die.net/man/5/limits.conf](https://linux.die.net/man/5/limits.conf)\) states

```text
Please note that all limit settings are set per login. 
They are not global, nor are they permanent; existing only for the duration of the session.
```

 The \`ulimit\` and \`limits.conf\` applies only to users and the processes which are initiated by users but doesn't apply to services. Services are started and managed by init daemon [Upstart](https://manpages.debian.org/wheezy/upstart/init.5.en.html) which is the 1st process to start on boot with the pid 1. The init daemon doesn't honour the limits \(even the hard limits\) set at limits.conf but it lets the service define its own service limits or provides its own settings if not defined by the service.

From [https://serverfault.com/a/632018](https://serverfault.com/a/632018)

```text
The Upstart behaviour is as specified and is not a bug. 
Services are bound by their own configuration and not by the security limits (which bind user processes for which there is no other definition. 
Hence why it is security limits)
```

Upstart honours the security limits only for user job managed by it. There is a note on that here [http://upstart.ubuntu.com/cookbook/\#limit](http://upstart.ubuntu.com/cookbook/#limit)

```text
If a user job specifies this stanza (limit), it may fail to start should it specify a value greater than the users privilege level allows.
```

In our case, Docker is running as a service \(and not initiated by a user\) and the docker init scripts sets its own resource limits during its start up and the core file limit is set as unlimited. If resource limits are not specified while starting a Docker container using the --ulimit flag, the containers inherits the limits from Docker daemon.

Docker provides a configuration \(/etc/docker/daemon.json\) to set the default resource limits for the containers 

```text
{
  "default-ulimits": {
    "core": {
      "Hard": 0,
      "Name": "core",
      "Soft": 0 
    }
  }
} 
```

With this configuration,  containers will have these resource limits by default and can be overridden by --ulimit flag during container creation.

These settings applies regardless of containers being started/managed by users or any other orchestration framework like Docker Swarm or Kubernetes. 

This solves the resources limit settings for Docker containers. If we need to override the resource limits for the Docker daemon it self, Upstart \(the init process\) provides an option to place an override file in the /etc/init directory for that particular service. \( this has not been tested though as the ticket talks only about containers\)

From [https://manpages.debian.org/wheezy/upstart/init.5.en.html\#DESCRIPTION](https://manpages.debian.org/wheezy/upstart/init.5.en.html#DESCRIPTION)

```text
Files ending in .override are called override files. If an override file is present, the stanzas it contains take precedence over those equivalently named stanzas in the corresponding configuration file contents for a particular job
```

