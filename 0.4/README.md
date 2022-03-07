# Building 0.4.x

1. Clone Bitcoin and checkout a 0.4.x tag
2. In the parent directory, clone gitian-builder and `cd` into it. Make sure gitian dependencies for Docker are installed and working.
3. Apply the patch:

```
git am ../../build-old-bitcoin/0.4/gitian-builder.patch
```

4. Download "inputs":

```
mkdir -p inputs; cd inputs/
wget 'https://github.com/wxWidgets/wxWidgets/releases/download/v2.9.2/wxWidgets-2.9.2.tar.bz2'
wget 'http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.6.tar.gz' -O miniupnpc-1.6.tar.gz
cd ..
```

5. Make base VM

```
bin/make-base-vm -s lucid -a i386 --docker
bin/make-base-vm -s lucid -a amd64 --docker
```

6. Build wxWidgets (Note: This step is needed unless you want to modify the gitian descriptor).

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.4.0 ../bitcoin/contrib/gitian-descriptors/wxwidgets.yml
```

7. Move `build/out/wxWidgets-2.9.2-x32-gitian.zip` and `build/out/wxWidgets-2.9.2-x64-gitian.zip` to `inputs/`

8. Do the build (replace 0.4.0 with the correct tag)

```
env USE_DOCKER=1 bin/gbuild --commit bitcoin=v0.4.0 ../bitcoin/contrib/gitian-descriptors/gitian.yml
```

9. Retrieve the binaries from `build/out/bin/`.
