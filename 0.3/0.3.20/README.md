# Building 0.3.20

Note that only bitcoind will be built.

1. Clone Bitcoin and checkout the v0.3.20 tag
2. In the parent directory, clone gitian-builder and `cd` into it. Make sure gitian dependencies for Docker are installed and working.
3. Apply the patch:

```
git am ../../build-old-bitcoin/0.3/gitian-builder.patch
```

4. Download "inputs":

```
mkdir -p inputs; cd inputs/
wget 'https://github.com/wxWidgets/wxWidgets/releases/download/v2.9.0/wxWidgets-2.9.0.tar.bz2'
cd ..
```

5. Make base VM

```
bin/make-base-vm -s lucid -a i386 --docker
bin/make-base-vm -s lucid -a amd64 --docker
```

8. Do the build. This build will use a custom gitian descriptor from this repo.

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.3.20 ../build-old-bitcoin/0.3/0.3.20/gitian-linux.yml
```

9. Retrieve the binaries from `build/out/bin/`.
