# freeswitch-debian-package

This repo contains the debian files needed for building deb packages for UBUNTU, (Tested on ubuntu 12.04 and 14.04). There is a separate debian packaging files for music and sound. Below are the steps to build the packages.

Default Prefix Directory is `/usr/local/freeswitch`

####Building Freeswitch packages

  Clone your Freeswitch repo

	$ git clone https://freeswitch.org/stash/scm/fs/freeswitch.git

  Install the dependencies

	$ apt-get -y install autoconf automake devscripts gawk g++ git-core libjpeg-dev libncurses5-dev libtool make python-dev gawk pkg-config libtiff4-dev libperl-dev libgdbm-dev libdb-dev gettext sudo equivs mlocate git dpkg-dev devscripts sudo wget sox flac

  Building packages

	$ DISTRO=`lsb_release -cs`

	$ DCH_DISTRO=UNRELEASED

	$ FS_VERSION="$(cat freeswitch/build/next-release.txt | sed -e 's/-/~/g')~n$(date +%Y%m%dT%H%M%SZ)-1~${DISTRO}+1"

	$ cd freeswitch && build/set-fs-version.sh "$FS_VERSION"

	$ dch -b -m -v "$FS_VERSION" --force-distribution -D "$DCH_DISTRO" "Custom build."

	$ cd debian && ./bootstrap.sh -c ${DISTRO}

	$ mk-build-deps -i control 

	$ cd ../ && dpkg-buildpackage -b -uc

  
####Building music and sound packages

  Choose the appropriate sampling rate by updating the `srate` value in `rules` file. Defaul is 8000. Go to the corresponding folders and run the below commands to build the packages

	$ ./debian/rules get-orig-source

	$ tar -xv --strip-components=1 -f *_*.orig.tar.xz && mv *_*.orig.tar.xz ../

	$ dpkg-buildpackage -uc -us -Zxz -z9

