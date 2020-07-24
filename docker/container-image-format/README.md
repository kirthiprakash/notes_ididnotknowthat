---
description: 'https://github.com/moby/moby/blob/master/image/spec/v1.md'
---

# Container Image format

### Union Filesystems

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

### Constructing Image IDs

Image IDs are formed by taking a hash of the image's configuration object in JSON. It consists of the root os architecture, platform, config, created date, docker version, image history, digests of intermediate layers.

#### Sample manifests file

```text
{
  "architecture": "amd64",
  "config": {
    "Hostname": "",
    "Domainname": "",
    "User": "",
    "AttachStdin": false,
    "AttachStdout": false,
    "AttachStderr": false,
    "ExposedPorts": {
      "80/tcp": {}
    },
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Cmd": null,
    "ArgsEscaped": true,
    "Image": "sha256:273b6cd2c559fd7f4a6e9989c73a6ced44bd3d4787146cb0b02b77d299de0d91",
    "Volumes": null,
    "WorkingDir": "",
    "Entrypoint": [
      "/main.sh"
    ],
    "OnBuild": null,
    "Labels": {
      "maintainer": "opsxcq@strm.sh"
    }
  },
  "container": "f5d00def67f63b78455d80e80d07048d6c4177e9f1e6766e706752890bf9c037",
  "container_config": {...},
  "created": "2018-10-12T17:49:01.240444043Z",
  "docker_version": "18.03.1-ee-3",
  "history": [
    {
      "created": "2017-11-04T05:26:40.421915785Z",
      "created_by": "/bin/sh -c #(nop) ADD file:a71e077a42995a68ffe4834d85cfe26af4ea12aa8ed43decc03cc487124b1f70 in / "
    },
    {
      "created": "2017-11-04T05:26:40.778734274Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"bash\"]",
      "empty_layer": true
    },
    {
      "created": "2018-10-12T17:44:51.568417068Z",
      "created_by": "/bin/sh -c #(nop)  LABEL maintainer=opsxcq@strm.sh",
      "empty_layer": true
    },
    {
      "created": "2018-10-12T17:48:41.572499443Z",
      "created_by": "/bin/sh -c #(nop) COPY file:e1162a50525b284972b663daef5ca505c724673da0dda9a707bc8f67e4ec1220 in /etc/php5/apache2/php.ini "
    },
    {
      "created": "2018-10-12T17:48:43.817849479Z",
      "created_by": "/bin/sh -c #(nop) COPY dir:9c23b23aaae913c12ab3d2659b4d45398ee5652ed113267814c49d9ba501992a in /var/www/html "
    },
    {
      "created": "2018-10-12T17:48:44.34086818Z",
      "created_by": "/bin/sh -c #(nop) COPY file:55e9d94279708ad763c17a1ca775e225829b275ace5716ca8a0aff69c70161a4 in /var/www/html/config/ "
    },
    {
      "created": "2018-10-12T17:48:49.325818591Z",
       "created_by": "/bin/sh -c chown www-data:www-data -R /var/www/html &&     rm /var/www/html/index.html"
    },
    {
      "created": "2018-10-12T17:49:00.560515018Z",
      "created_by": "/bin/sh -c #(nop)  EXPOSE 80",
      "empty_layer": true
    },
    {
      "created": "2018-10-12T17:49:00.941597385Z",
      "created_by": "/bin/sh -c #(nop) COPY file:f24e7713eb6c0568608bea3ff7b52edda86305cfd5bef0ac4e57efdb15792202 in / "
    },
    {
      "created": "2018-10-12T17:49:01.240444043Z",
      "created_by": "/bin/sh -c #(nop)  ENTRYPOINT [\"/main.sh\"]",
      "empty_layer": true
    }
  ],
  "os": "linux",
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:a75caa09eb1f7d732568c5d54de42819973958589702d415202469a550ffd0ea",
      "sha256:80f9a8427b1826f014f873dc471b6a89916ff79550bcd1c94aadd78c3f5bbdc7",
      "sha256:97a1040801c3e87c036ef26da36a8dcbce61c0c8a6b4b3c8d9dda3409e7dfffe",
      "sha256:acf8abb873cedff7e2aad9da561c592a5aac1938cb2bf0f8c4f6c97406c92a17",
      "sha256:9713610e6ec4cbca7ade219f3efc5df93bd74b21d60c4754d6f400dad226f02f",
      "sha256:73e92d5f2a6cf8a95578e6191222aaa8ade9a5cd792e9b3ed8a2b6a023bb8259",
      "sha256:585e40f29c46d8ef2ecbccb870654b76e0910da9123bff93457011eebbc5cf6c",
      "sha256:deeea3c4d56f1bbb10d3bd44879305d2fbef6678aa6c20947bd99576fe85fd45"
    ]
  }
}

```

The configation object contains the history, the commands used to build the objects and the digests of the layer diffs. Each command in the Dockerfile corresponds to a layer. There may be a scenario where the number of history items are more than the number of layer diffs. If a command is a no-op, which doesn't change anything in the file system \(EXPOSE 80, SET ENTRYPOINT etc.\), it will not add to a new layer.

The layer diffs are calculated by performing a digest on the archived layer contents.

`<digest algorithm>:<digest>`

\`\`

