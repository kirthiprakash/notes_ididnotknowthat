# Dockerfile Best Practises

### Docker build image layer caching

While building docker image, at a particular step, if there are no changes in the filesystem, the build tool will use the layer build last time, from its cache.

For scenarios where depedency installation file and source code are together, its advised to add just the dependency file without the source code and trigger the installation step. Once installation is done, add the source code. This helps in taking advantage of the layer cache if there is no changes in the dependency. 

If both dependency file and source code is added before dependency installation, for every change in the source code, but not in the dependency file, the build tool cannot utilize the layer caching and hence will trigger the installation on each build.

In Node application, only package.json \(all its related lock files\) needs to be copied.

In Python applications, the requirement.txt file alone needs to be copied.

The build tool also provides an option to skip cache with the `--no-cache` during docker build command

```text
docker build -t repo/imagename --no-cache
```

The `FROM` command is the only command to ignore the --no-cache, but the force pull newer images we can pass the `--pull` flag

```text
docker build -t repo/imagename --pull
```

