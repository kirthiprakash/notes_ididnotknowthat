# whiteout files

In Union mount files systems, the difference between two layers is handled by saving just the changeset.

The deleted files are save as an empty file with ".wh." prefixed to the original name of the deleted file.

From the previous example

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

layer 2 Tar archive contents

```text
/etc/my-app.d/default.cfg
/bin/my-app-tools
/etc/.wh.my-app-config
```

Since \`my-app-config\` was deleted, its have been replaced by an empty file with '.wh.' prefix

