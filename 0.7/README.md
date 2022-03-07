# Building 0.7.x

1. Clone Bitcoin and checkout a 0.7.x tag
2. In the parent directory, clone gitian-builder and `cd` into it. Make sure gitian dependencies for Docker are installed and working.
3. Apply the patch:

```
git am ../../build-old-bitcoin/0.7/gitian-builder.patch
```

4. Download "inputs":

```
mkdir -p inputs; cd inputs/
wget 'http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.6.tar.gz' -O miniupnpc-1.6.tar.gz
wget 'http://fukuchi.org/works/qrencode/qrencode-3.2.0.tar.bz2'
cd ..
```

5. Make base VM

```
bin/make-base-vm -s lucid -a i386 --docker
bin/make-base-vm -s lucid -a amd64 --docker
```

6. Do the build (replace 0.7.0 with the correct tag)

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.7.0 ../bitcoin/archaeology/contrib/gitian-descriptors/gitian.yml
```

7. Retrieve the binaries from `build/out/bin/`.
