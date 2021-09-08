# SWTPM
The following explains how to launch and use `swtpm`. 

The standard software implementation of a TPM is [swtpm](https://github.com/stefanberger/swtpm). A recipe for compiling it is included in the [meta-security/meta-tpm layer](https://git.yoctoproject.org/cgit/cgit.cgi/meta-security/tree/meta-tpm/recipes-tpm/swtpm) of the OpenEmbedded project. 

For testing the features which rely on a `TPM` we can compile it as a native tool on the host and start it before launching the image with `runqemu`. I.e. we compile it with:
```
bitbake swtpm-native
```
NOTE: We are currently in the process of updating the version of swtpm: 0.5.2 --> 0.6.0. This will fix some issues with dependencies on python.

After swtpm is compiled the binaries are located in `${WORKDIR}/swtpm-native`.

The [QEMU documentation](https://qemu-project.gitlab.io/qemu/specs/tpm.html#the-qemu-tpm-emulator-device) suggests starting `swtpm` with 
```
$ mkdir /tmp/mytpm1
$ swtpm socket --tpmstate dir=/tmp/mytpm1 --ctrl type=unixio,path=/tmp/mytpm1/swtpm-sock --log level=20 --tpm2
```
To have the image connect to it when lauched via `runqemu` we pass extra parameters via the `qemuparams` option. This becomes
```
runqemu wic ovmf mukube nographic qemuparams="-chardev socket,id=chrtpm,path=/tmp/mytpm1/swtpm-sock -tpmdev emulator,id=tpm0,chardev=chrtpm -device tpm-tis,tpmdev=tpm0" 
```


