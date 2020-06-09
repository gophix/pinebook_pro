# Streamripper via pkgbuild

## Preparations

Install build env:

 - binutils
 - gcc
 - make
 - guile
 - libmpc
 - pkgconf
 - fakeroot

## build manually

```
$ ./configure --build=unknown-unknown-linux
$ make
$ make install
$ make clean depclean
```

## makepkg build

```
$ export LC_MESSAGES=POSIX
$ cd Documens/streamripper
$ makepkg
$ sudo pacman -U streamripper-1.64.6-1-aarch64.pkg.tar.xz --overwrite /usr/local/share/man    # install
$ sudo pacman -R streamripper                                                                 # remove
```

# run streamripper

```
$ cd streamripper
$ streamripper http://stream.on.se/rver
```
