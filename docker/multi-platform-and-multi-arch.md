# Multi platform and Multi Arch

Based on the manifests file, docker images can differentiated for mutiple architecture and os.

The docker cli pull the appropriate os version based on where the command is invoked.

Tools like skopeo which are used to pull images, we can override the os and architecture flags to pull that particular manifest file if one exists.

