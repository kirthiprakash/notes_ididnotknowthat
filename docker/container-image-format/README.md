---
description: 'https://github.com/moby/moby/blob/master/image/spec/v1.md'
---

# Container Image format

An container image can be made up of mutiple root filesystem and combine each of them to form an abstract union filesystem.

Layer 1 might have

```text
 etc/
        my-app-config
    bin/
        my-app-binary
        my-app-tools-v1
```

Layer 2 might have modified some of the contents

```text
    etc/
        my-app.d/
            default.cfg
    bin/
        my-app-binary
        my-app-tools-v2
```

The layer 2 will have only information about the changeset compared to the layer 1.

```text
Added:      /etc/my-app.d/default.cfg
Modified:   /bin/my-app-tools
Deleted:    /etc/my-app-config
```

A Tar Archive is then created which contains _only_ this changeset. Any given image is likely to be composed of several of these Image Filesystem Changeset tar archives.

Union mounting these layers will result in a single "final" filesystem which is union of all the layers.

AUFS, OverlayFS, GlusterFS are some of the union mount implementations

