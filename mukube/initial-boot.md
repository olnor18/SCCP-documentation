# Initial boot

1. First boot
   1. The Secure Boot keys are generated with `sbctl`
   1. A encrypted partition is created with `systemd-repart`:
      * The following files are copied to the new fs:
        1. A new genereated machine-id
        1. The generated Secure Boot keys
      * A empty keyfile is used as key
      * The operation should be atomic, i.e. the partition table is written after the fs and files have been copied (TBC, see [this upstream ML discussion](https://lists.freedesktop.org/archives/systemd-devel/2020-May/044599.html))
   1. The bootloader is signed
   1. A [Unified Kernel Image](https://systemd.io/BOOT_LOADER_SPECIFICATION/#type-2-efi-unified-kernel-images) is created and signed
   1. The Secure Boot keys are enrolled
   1. Reboot before [`sysinit.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#sysinit.target) or [`multi-user.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#multi-user.target) is reached (not decided yet)
1. Second boot
   1. The encrypted partition is unlocked with a empty keyfile as key
   1. The TPM chip is enrolled for unlocking the encrypted partition
   1. The empty keyfile is removed as key
   1. Reboot before [`sysinit.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#sysinit.target) or [`multi-user.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#multi-user.target) is reached (not decided yet)
1. Third boot
   1. The TPM is used for unlocking the encrypted partition
   1. Normal boot

Notes:
* We need to reboot after enrolling the Secure Boot keys for new PCR7 ("Secure Boot Policy") measurements
* By blocking [`sysinit.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#sysinit.target) or [`multi-user.target`](https://www.freedesktop.org/software/systemd/man/systemd.special.html#multi-user.target) (not decided yet) "normal services" won't be started (ex: `crio.service` and `kubelet.service`)