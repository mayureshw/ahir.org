# Installing AHIR

There are two options to install AHIR on your system:

1. **Using AHIR Docker Image**

    Docker images is the quickest way to get started with AHIR on your system. You just need docker installed on your system.

    AHIR docker images are available in `ghcr.io` and `docker.com` repositories. The `ghcr.io` image is built by a CI flow on AHIR's github repository. Hence this image gets updated regularly. The `docker.com` image is uploaded manually. It may lag behind the ghcr.io image but is likely to be more stable.

    Commands to get and run these images are as follows:

    1. From `ghcr.io`

        ```
        docker pull ghcr.io/mayureshw/vctools:latest
        docker run -it ghcr.io/mayureshw/vctools:latest
        ```

    1. From `docker.com`

        ```
        docker pull mayureshw/ahir:latest
        docker run -it mayureshw/ahir:latest
        ```


2. **Builing AHIR on your system**

    AHIR is available in the [pkgsrc](https://pkgsrc.org/) repository which makes it easy to build on any Unix-like system along with all its dependencies. Following steps are involved in building AHIR on a Unix-like system. All commands are to be run as a root user.

    1. Install the essential build tools for your platform. On Ubuntu-like systems the following would suffice. The package names and installation command may vary from platform to platform.

        ```
        apt-get update && apt-get install -y --no-install-recommends \
            curl wget bzip2 gzip tar xz-utils git ca-certificates build-essential \
            pkg-config libssl-dev libbz2-dev libz-dev libncurses5-dev
        ```

    1. Clone the pkgsrc and pkgsrc-wip repositories

        ```
        cd /usr && git clone --depth 1 https://github.com/NetBSD/pkgsrc
        cd /usr/pkgsrc && git clone --depth 1 https://github.com/NetBSD/pkgsrc-wip wip
        ```

    1. Bootstrap pkgsrc

        ```
        cd /usr/pkgsrc/bootstrap && \
            ./bootstrap --prefer-pkgsrc yes --make-jobs `/usr/bin/nproc` \
            --workdir /usr/pkgsrc/work
        ```

    1. Set the PATH variable. You may want to add this to your shell's startup file such as `~/.profile`

        ```
        export PATH=/usr/pkg/bin:/usr/pkg/sbin:$PATH
        ```

    1. Build and install ahir

        ```
        cd /usr/pkgsrc/wip/ahir && bmake MAKE_JOBS=`/usr/bin/nproc` update
        ```

        Depending on your CPU and internet speed, the build may take 30 to 45 minutes.

    1. Reclaiming the disk space

        You may wish to reclaim some of the space used for the build such as:

        1. `/usr/pkgsrc` : If this was a one off build for you, you may want to delete the the entire pkgsrc source tree. Otherwise, you may wish to delete the contents of the following subdirectories, retaining them as empty directories for future use.
        2. `/usr/pkgsrc/distfiles/*` : Delete the source code tarballs downloaded during the build
        3. `/usr/pkgsrc/work/*` : Delete the temporary area used by pkgsrc for the builds.

# Testing the installation

1. If you are using a docker image the PATH environment is already set. If you installed using pkgsrc set the PATH environment variable and add it to your shell's startup file such as `~/.profile`

    ```
    export PATH=/usr/pkg/bin:/usr/pkg/sbin:$PATH
    ```

2. AHIR is packaged with several examples to test and learn various tools. This is an example of testing the virtual circuit simulator tool in the AHIR framework. The examples for this tool are packaged in the folder `/usr/pkg/share/examples/ahir/vcsim`

    You may copy this folder to a suitable work area and use the `Makefile` in it to build all the examples in it, which contains its documentation. All the examples in the folder, `*.aa`, should  get compiled and should produce executables `*.out` corresponding to them. Explanation of these test cases is beyond the scope of this note and can be found under the [Documentation](../documentation/) tab.
