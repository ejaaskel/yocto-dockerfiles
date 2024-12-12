# yocto-docker

macOS compatible version of crops/yocto-dockerfiles. Currently only Ubuntu-22.04 image has been tested. To build the ubuntu-22.04-base and ubuntu-22.04-builder images, run the following:
```
# Export REPO name. This will be the name of the built image.
# You can use crops/yocto if you want to override it
export REPO=ejaaskel/yocto
# Export the disto to be built. This should be one of the
# versioned distros in dockerfiles-folder. Only ubuntu-22.04
# has been tested by me.
export DISTRO_TO_BUILD=ubuntu-22.04
# Run the build script for a single image
./build_container.sh
```

More information on how to use these Docker images and how to build Yocto with Apple Silicon can be found from my blog:
https://ejaaskel.dev/how-to-build-yocto-with-apple-silicon/


### Note About Upgrading to Ubuntu 24.04

After the latest pull from upstream `master` there is a Dockerfile available for Ubuntu 24.04.
However, the official Ubuntu 24.04 Docker image has a user named `ubuntu` with UID 1000.
This clashes with the user management actions performed by [crops/poky-container](https://github.com/crops/poky-container), and the `pokyuser` cannot be created.
Following error can be seen when launching the poky-container:

```
sudo: unknown user pokyuser
sudo: error initializing audit plugin sudoers_audit
```

There are few options to work around this:

1) Keep using Ubuntu 22.04 (my current choice)
1) Use some other distro than Ubuntu (my future choice)
1) Use other ID than the default 1000 in poky-container `docker run` command. This can be achieved with `--id` option
1) Remove the `ubuntu` user. This can be achieved by adding `RUN userdel -r ubuntu` to ubuntu-24.04-base Dockerfile
