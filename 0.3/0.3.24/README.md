i# Building 0.3.24

1. Clone Bitcoin and checkout the v0.3.24 tag
2. In the parent directory, clone gitian-builder and `cd` into it. Make sure gitian dependencies for Docker are installed and working.
3. Apply the patch (Note this is the same patch as 0.4 and 0.7):

```
git am ../../build-old-bitcoin/0.3/gitian-builder.patch
```

4. Download "inputs":

```
mkdir -p inputs; cd inputs/
wget 'https://github.com/wxWidgets/wxWidgets/releases/download/v2.9.1/wxWidgets-2.9.1.tar.bz2'
wget 'http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.5.tar.gz' -O miniupnpc-1.5.tar.gz
cd ..
```

5. Copy wxWidgets patches:

```
cp ../bitcoin/contrib/wx-patches/toplevel.cpp.diff inputs/
cp ../bitcoin/contrib/wx-patches/toplevel.h.diff inputs/
```

6. Make base VM

```
bin/make-base-vm -s lucid -a i386 --docker
bin/make-base-vm -s lucid -a amd64 --docker
```

7. Do the build

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.3.24 ../bitcoin/contrib/gitian.yml
```

8. Retrieve the binaries from `build/out/bin/`.
