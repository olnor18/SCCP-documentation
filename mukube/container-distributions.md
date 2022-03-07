# Container distributions

[Flatcar Container Linux](https://www.flatcar.org/) and [Fedora CoreOS](https://getfedora.org/en/coreos/) is two Linux distributions designed for container workloads.

They both offer similar features:
- Minimal OS with only the tools required for running container workloads
- "Immutable" filesystem ([Flatcar](https://www.flatcar.org/docs/latest/reference/developer-guides/sdk-disk-partitions/), [CoreOS](https://docs.fedoraproject.org/en-US/fedora-coreos/storage/#_mounted_filesystems))
- Automated atomic updates (Flatcar uses dual partition system (A/B updating), CoreOS uses OSTree)

Questions:
- How does rolling updates work?
- Do they support Secure Boot?

## Rolling updates

Upgrading all nodes in a cluster at the same time, would break the cluster.
The updating must be coordinated so they aren't updated at the same time.

### Flatcar

Flarcar has created: [Flatcar Linux Update Operator](https://github.com/flatcar-linux/flatcar-linux-update-operator/), which is k8s aware.

> Flatcar Linux Update Operator is a node reboot controller for Kubernetes running Flatcar Container Linux images. When a reboot is needed after updating the system via update_engine, the operator will drain the node before rebooting it.
>
> Flatcar Linux Update Operator fulfills the same purpose as locksmith, but has better integration with Kubernetes by explicitly marking a node as unschedulable and deleting pods on the node before rebooting.

### CoreOS

CoreOS has a not-k8s aware updating mechanism called [airlock](https://github.com/coreos/airlock):

> Airlock is a minimal update/reboot manager for clusters of Linux nodes. It is meant to be simple to run in a container.
>
> Fleet-wide updates and reboots are coordinated via semaphore locking, with configurable groups and simultaneous reboot slots.

This is very disruptive to a k8s cluster as the nodes won't be drained before the node is rebooted.
A k8s aware update operator is tracked upstream [here](https://github.com/coreos/fedora-coreos-tracker/issues/241) with links to a few third party operators: [System Upgrade Controller](https://github.com/rancher/system-upgrade-controller) by Rancher and [fleetlock](https://github.com/poseidon/fleetlock) by Poseidon.

Another project mentioned is: [machine-config-operator](https://github.com/openshift/machine-config-operator), but it unclear if it works outside the OpenShift 4 ecosystem:

> OpenShift 4 is an [operator-focused platform](https://blog.openshift.com/openshift-4-a-noops-platform/), and the Machine Config operator extends that to the operating system itself, managing updates and configuration changes to essentially everything between the kernel and kubelet.
>
> To repeat for emphasis, this operator manages updates to systemd, cri-o/kubelet, kernel, NetworkManager, etc. It also offers a new `MachineConfig` CRD that can write configuration files onto the host.

## Secure Boot

Secure Boot in our case means two things:
- The hardware must only boot software signed by us
- A [chain of trust](https://en.wikipedia.org/wiki/Chain_of_trust) must be ensured in the boot process, so only trusted code can be executed

Flatcar doesn't support Secure Boot in any capacity ([upstream issue](https://github.com/flatcar-linux/Flatcar/issues/501)) as of 2022-03-03.

CoreOS's EFI binaries are signed by Red Hat (AFAIU), but a sufficient [chain of trust](https://en.wikipedia.org/wiki/Chain_of_trust) isn't established. This means ex: a malicious [ignition](https://github.com/coreos/ignition) config, can be used to run untrusted code on the machine.

In both cases it should be possible to sign the bootloader and kernel with our own keys, but establishing a sufficient [chain of trust](https://en.wikipedia.org/wiki/Chain_of_trust) is the tricky part.
Both projects use [ignition](https://github.com/coreos/ignition) which would need to support some kind of verification of the input (checking signature or hash) and changes to the boot process is likely also needed. It is unclear if upstream would accept that kind of changes.
