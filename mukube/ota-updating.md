# OTA (over-the-air) updating

Updating mukube manually (ex: by replacing the boot disk) is very labor intensive and not scalable.
A more scalable approach is updating them over-the-air (i.e. they download a update and install the update automatically), which can be implemented in a few different ways.

## Solutions

The Yocto project has a thorough [wiki page](https://wiki.yoctoproject.org/wiki/System_Update) comparing different updating solutions.

The most promising solutions are:
- [SWUpdate](https://swupdate.org/)
- [Mender](https://mender.io/)
- [RAUC](https://rauc.io/)

As they offer a complete end-to-end solution (i.e. the update client and management server) and these features:
- Atomic updates ([Mender](https://mender.io/how-it-works), [RAUC](https://rauc.readthedocs.io/en/latest/updating.html#redundancy-and-atomicity))
- Rollback support
- Signed updates
- Management server ([hawkBit](https://www.eclipse.org/hawkbit/) for SWUpdate and RAUC, [Mender Server](https://mender.io/how-it-works) for Mender)
  - With support for phased rollouts
- Yocto layers

Mender is [open core](https://opensource.com/article/21/11/open-core-vs-open-source) and has a enterprise plan, the remaining two projects are fully open-source and enterprise support isn't offered.

Before deciding on a final solution, the following must be taken into account:
- Is a support agreement required?
- Can it work with Secure Boot?
- Can it worh with our current bootloader (systemd-boot)?
- Do we want a fully open-source solution?

## Linux distributions with built in OTA-updating

Container distributions like [Flatcar Container Linux](https://www.flatcar.org/) and [Fedora CoreOS](https://getfedora.org/en/coreos) have their own updating mechanism with most of the listed properties.
It is unclear if Flatcar supports phased rollouts, but it is [supported](https://github.com/coreos/zincati/blob/d6c6c8bafc07631b0e399279a1a33bb91f490040/docs/usage/auto-updates.md#phased-rollouts-client-wariness-canaries) by Fedora CoreOS.

# References

- https://wiki.yoctoproject.org/wiki/System_Update
- https://elinux.org/images/3/31/Comparison_of_Linux_Software_Update_Technologies.pdf
- https://mkrak.org/wp-content/uploads/2018/04/FOSS-NORTH_2018_Software_Updates.pdf
