# Virtualization management platform

Requirements:
* Easy to maintain
* Software TPM support
* Secure Boot support
* Terraform provider

## Comparison

Compared platforms:
* [VMware vSphere](https://www.vmware.com/products/vsphere.html)
* [oVirt](https://www.ovirt.org/)
* [OpenStack](https://www.openstack.org/)
* [libvirt](https://libvirt.org/)

### VMware vSphere

<details>
  <summary>Click here for deployment instructions!</summary>
  
1. Download vSphere Hypervisor 7.0 from [here](https://customerconnect.vmware.com/en/web/vmware/evalcenter?p=free-esxi7) and install it
1. Download `VMware vCenter Server 7.0U2d` from [here](https://customerconnect.vmware.com/downloads/info/slug/datacenter_cloud_infrastructure/vmware_vcloud_suite/2019)
1. Mount the ISO file to `/mnt`: `mount $(losetup --find --show VMware-VCSA-all-7.0.2-18455184.iso) /mnt`
1. Follow [these instructions](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vcenter.install.doc/GUID-C17AFF44-22DE-41F4-B85D-19B7A995E144.html) for deploying vCenter
</details>

Pros:
* Web interface
* [Software TPM support](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-3D39CBA6-E5B2-43E2-A596-B9A69B094558.html)
* Secure Boot is somehow supported (see cons)
* Well maintained [terraform provider](https://github.com/hashicorp/terraform-provider-vsphere)

Cons:
* Unclear if Secure Boot can be configured in setup mode (the documentation is a [forum post](https://communities.vmware.com/t5/ESXi-Discussions/How-to-replace-default-certificate-for-Secure-Boot-Virtual/td-p/1753161))
* Proprietary/Requires a license
* Semi-complex (hypervisor with vCenter as guest)

### oVirt

<details>
  <summary>Click here for deployment instructions!</summary>

1. Download oVirt Node from [here](https://www.ovirt.org/download/node.html) and install it
1. Access Cockpit at `http://<ip>:9090`
1. See these links for how to configure a valid FQDN:
   https://stackoverflow.com/questions/58212221/how-to-configure-fqdn-in-ovirt-engine  
   https://www.techbeatly.com/2020/03/installing-ovirt-4-with-self-hosted-engine-on-enterprise-linux.html
1. Follow [these instructions](https://www.ovirt.org/documentation/gluster-hyperconverged/chap-Single_node_hyperconverged.html) for installing GlusterFS
   1. Be aware of [this bug](https://bugzilla.redhat.com/show_bug.cgi?id=2002178)  
      Solvable by installing `glusterfs-selinux` from [here](https://cbs.centos.org/koji/buildinfo?buildID=33358):
      ```sh
      $ curl -JLO 'https://cbs.centos.org/kojifiles/packages/glusterfs-selinux/2.0.1/1.el8/noarch/glusterfs-selinux-2.0.1-1.el8.noarch.rpm'
      $ dnf install glusterfs-selinux-2.0.1-1.el8.noarch.rpm
      ```
   1. For fixing `Device /dev/xx excluded by a filter` please run:
      ```sh
      $ sed -E 's/(filter =).*/\1 ["a|.*|"]/' -i /etc/lvm/lvm.conf
      ```
1. Install the `Hosted Engine` and use `<hostname>:/engine` as connection path for the GlusterFS storage configuration
1. Access the `Hosted Engine` at `https://<FQDN>`
</details>

Pros:
* Open-source
* Web interface
* [Software TPM support](https://www.ovirt.org/documentation/virtual_machine_management_guide/#Adding_TPM_devices)
* Secure Boot support (can be configured in "setup mode")
* Semi-maintained [terraform provider](https://github.com/oVirt/terraform-provider-ovirt)

Cons:
* Only community support  
  We can switch to [Red Hat Virtualization](https://www.redhat.com/en/technologies/virtualization/enterprise-virtualization) for paid support
* Semi-complex (Cockpit, Hosted Engine, GlusterFS)

### OpenStack

<details>
  <summary>Click here for deployment instructions!</summary>

1. Follow [these instructions](https://microstack.run/docs/single-node) for installing [MicroStack](https://microstack.run/) on Ubuntu. For a production deployment [Packstack](https://wiki.openstack.org/wiki/Packstack) is worth investigating

</details>

Pros:
* Open-source
* Web interface
* Basically a private cloud (block storage, floating ips, object storage etc.)
* [Software TPM support](https://docs.openstack.org/nova/latest/admin/emulated-tpm.html) (not tested)
* [Secure Boot support](https://docs.openstack.org/nova/latest/admin/secure-boot.html) (not tested)
* Well maintained [terraform provider](https://github.com/terraform-provider-openstack/terraform-provider-openstack)

Cons:
* [Very complex](https://docs.openstack.org/install-guide/get-started-logical-architecture.html)

### libvirt

<details>
  <summary>Click here for deployment instructions!</summary>

`libvirt` hasn't been installed as part of the research, but the writer has used it in the past.
</details>

Pros:
* Open-source
* Easy to run/maintain (Debian + libvirt = profit🎉)
* [Software TPM support](https://libvirt.org/formatdomain.html#tpm-device)
* Secure Boot support
* Maintained [terraform provider](https://github.com/dmacvicar/terraform-provider-libvirt)

Cons:
* No web interface
* Can be more cumbersome to use (depending on our requirements)
