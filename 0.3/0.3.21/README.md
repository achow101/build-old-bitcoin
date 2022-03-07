# Building 0.3.21

1. Clone Bitcoin and checkout the v0.3.21 tag
2. In the parent directory, clone gitian-builder and `cd` into it. Make sure gitian dependencies for Docker are installed and working.
3. Apply the patch:

```
git am ../../build-old-bitcoin/0.3/gitian-builder.patch
```

4. Download "inputs":

```
mkdir -p inputs; cd inputs/
wget 'https://github.com/wxWidgets/wxWidgets/releases/download/v2.9.1/wxWidgets-2.9.1.tar.bz2'
cd ..
```

5. Make base VM

```
bin/make-base-vm -s lucid -a i386 --docker
bin/make-base-vm -s lucid -a amd64 --docker
```

8. Do the build

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.3.21 ../bitcoin/contrib/gitian.yml
```

9. Retrieve the binaries from `build/out/bin/`.
