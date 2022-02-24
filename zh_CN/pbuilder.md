---
title: Pbuilder
---

### Install

sudo apt install pbuilder qemu-user-static fakeroot debian-archive-keyring

### Setup

- Add following contents to /etc/pbuilderrc:

```bash
# Codenames for Debian suites according to their alias. Update these when needed.

UNSTABLE_CODENAME="sid"
TESTING_CODENAME="bullseye"
STABLE_CODENAME="buster"
STABLE_BACKPORTS_SUITE="$STABLE_CODENAME-backports"

# List of Debian suites.
DEBIAN_SUITES=($UNSTABLE_CODENAME $TESTING_CODENAME $STABLE_CODENAME
    "unstable" "testing" "stable")

# List of Ubuntu suites. Update these when needed.
UBUNTU_SUITES=("focal" "bionic" "xenial")

# Mirrors to use. Update these to your preferred mirror.
DEBIAN_MIRROR="ftp.cn.debian.org"
UBUNTU_MIRROR="ftp.ubuntu.com"

# Optionally use the changelog of a package to determine the suite to use if none set.

if [ -z "${DIST}" ] && [ -r "debian/changelog" ]; then
    DIST=$(dpkg-parsechangelog | awk '/^Distribution: / {print $2}')
    DIST="${DIST%%-*}"
    # Use the unstable suite for certain suite values.
    if $(echo "experimental UNRELEASED" | grep -q $DIST); then
        DIST="$UNSTABLE_CODENAME"
    fi
fi

# Optionally set a default distribution if none is used. Note that you can set your own default (i.e. ${DIST:="unstable"}).

: ${DIST:="$(lsb_release --short --codename)"}

# Optionally change Debian release states in $DIST to their names.

case "$DIST" in
    unstable)
        DIST="$UNSTABLE_CODENAME"
        ;;
    testing)
        DIST="$TESTING_CODENAME"
        ;;
    stable)
        DIST="$STABLE_CODENAME"
        ;;
esac

# Optionally set the architecture to the host architecture if none set. Note that you can set your own default (i.e. ${ARCH:="i386"}).

: ${ARCH:="$(dpkg --print-architecture)"}

NAME="$DIST"
if [ -n "${ARCH}" ]; then
    NAME="$NAME-$ARCH"
    DEBOOTSTRAPOPTS=("--arch" "$ARCH" "${DEBOOTSTRAPOPTS[@]}")
fi
BASETGZ="/var/cache/pbuilder/$NAME-base.tgz"

# Optionally, set BASEPATH (and not BASETGZ) if using cowbuilder
# BASEPATH="/var/cache/pbuilder/$NAME/base.cow/"

DISTRIBUTION="$DIST"
BUILDRESULT="/var/cache/pbuilder/$NAME/result/"
APTCACHE="/var/cache/pbuilder/$NAME/aptcache/"
BUILDPLACE="/var/cache/pbuilder/build/"

if $(echo ${DEBIAN_SUITES[@]} | grep -q $DIST); then
    # Debian configuration
    MIRRORSITE="http://$DEBIAN_MIRROR/debian/"
    COMPONENTS="main contrib non-free"
    DEBOOTSTRAPOPTS=("${DEBOOTSTRAPOPTS[@]}" "--keyring=/usr/share/keyrings/debian-archive-keyring.gpg")

elif $(echo ${UBUNTU_SUITES[@]} | grep -q $DIST); then
    # Ubuntu configuration
    MIRRORSITE="http://$UBUNTU_MIRROR/ubuntu/"
    COMPONENTS="main restricted universe multiverse"
    DEBOOTSTRAPOPTS=("${DEBOOTSTRAPOPTS[@]}" "--keyring=/usr/share/keyrings/ubuntu-archive-keyring.gpg")
else
    echo "Unknown distribution: $DIST"
    exit 1
fi
```



### Hook

- Copy the hook you want from /usr/share/doc/pbuilder/examples/ to a directory:

```bash
mkdir ~/pbuilderhooks
cp /usr/share/doc/pbuilder/examples/B90lintian ~/pbuilderhooks
```

- Modify the hooks to satisfied our demands:

Change the lintian segments from:

`su -C "lintian -I --show-overrides "$BUILDDIR"/.change; - pbuilder`

to

`su -c "lintian -i -EvIL +pedantic --verbose "$BUILDDIR"/*.changes" - pbuilder`

- Then tell pbuilder to user the hooks in that directory:

```bash
echo "HOOKDIR=$HOME/pbuilderhooks/"  | sudo tee -a /etc/pbuilderrc
```



### Customize the chroot environment

```
sudo pbuilder --login --save-after-login --basetgz <path to your base.tgz file>
```

 

Note down the temporary build directory root which you can use to copy files to and from. For example,  

```
$ sudo pbuilder --login
I: Building the build Environment
I: extracting base tarball [/media/forge/debian/pbuilder/sid-base.tgz]
I: creating local configuration
I: copying local configuration
I: mounting /proc filesystem
I: mounting /run/shm filesystem
I: mounting /dev/pts filesystem
I: Mounting /media/forge/debian/pbuilder/ccache
I: policy-rc.d already exists
I: Obtaining the cached apt archive contents
I: entering the shell
File extracted to: /media/forge/debian/pbuilder/build/26975

root@savannah:/# 
```

Your  debian environment is at /media/forge/debian/pbuilder/build/26975. Any  file you copy there will be available to debian chroot.  



### Multi-arch

If use pbuilder-dist:

```shell
pbuilder-dist unstable armhf create
pbuilder-dist unstable armhf bb_1.3rc1-8.3.dsc
```

Manually doing what pbuilder-dist can do:

```shell
#Create a base environment for Debian sid
# if it warning that the dir do not exist, just `sudo mkdir -p xxxx`
sudo DIST=sid pbuilder create

#Create a base environment for Ubuntu eoan under the i386 architecture
sudo DIST=eoan ARCH=i386 pbuilder create

#Create a base environment for Ubuntu eoan
sudo DIST=eoan pbuilder create

#Update a base environment for Ubuntu eoan
sudo DIST=eoan pbuilder update

#Build a package using Ubuntu eoan as the base environment
DIST=eoan pdebuild

#Build a package using Ubuntu eoan as the base environment under the i386 architecture
DIST=eoan ARCH=i386 pdebuild
```



### Reference

https://wiki.ubuntu.com/PbuilderHowto

https://wiki.debian.org/PbuilderTricks

https://pbuilder-docs.readthedocs.io/en/latest/index.html
