<!-- .slide: data-state="section-break" id="vm-basics" data-timing="10s" -->
# `libvirt` for starters

Note:
* API over hypervisors


<!-- .slide: data-state="normal" id="using-libvirt" data-timing="60s" data-menu-title="Using libvirt" -->
## Using `libvirt`

* One daemon: `libvirt`
* Clients: `virsh`, virt-manager
* Language bindings: C, Python, Go...

```sh
virsh --connect qemu:///system list --all
```

Note:
* Manual process, scriptable


<!-- .slide: data-state="normal" id="libvirt-objects" data-timing="60s" data-menu-title="libvirt objects" -->
## `libvirt` Objects

* **domain**: Virtual Machine
* **pool**: where VM storage is located
* **volume**: actual VM disk
* **virtual network**: networks used by the VM


<!-- .slide: data-state="normal" id="libvirt-xml" data-timing="60s" data-menu-title="libvirt XML" -->
## `libvirt` XML Definition

```xml
<domain type='kvm'>
  <name>srv</name>
  <memory unit='KiB'>8388608</memory>
  <currentMemory unit='KiB'>8388608</currentMemory>
  <vcpu placement='static'>2</vcpu>
  <os>
    <type arch='x86_64' machine='pc-i440fx-4.1'>hvm</type>
    <boot dev='hd'/>
  </os>
  <features>
    <acpi/>
    <apic/>
    <pae/>
  </features>
  <clock offset='utc'/>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <devices>
    <disk type='volume' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source pool='default' volume='srv-main-disk'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='network'>
      <mac address='52:54:00:a6:04:01'/>
      <source network='net0'/>
      <model type='virtio'/>
    </interface>
    <console type='pty'>
      <target type='serial' port='0'/>
    </console>
    <channel type='pty'>
      <target type='virtio' name='org.qemu.guest_agent.0'/>
    </channel>
    <input type='mouse' bus='ps2'/>
    <input type='keyboard' bus='ps2'/>
    <graphics type='spice' autoport='yes' listen='127.0.0.1'>
      <listen type='address' address='127.0.0.1'/>
    </graphics>
    <video>
      <model type='cirrus' vram='16384' heads='1' primary='yes'/>
    </video>
    <memballoon model='virtio' />
    <rng model='virtio'>
      <backend model='random'>/dev/urandom</backend>
    </rng>
  </devices>
</domain>
```


<!-- .slide: data-state="normal" id="libvirt-workflow" data-timing="120s" data-menu-title="libvirt Workflow" -->
## `libvirt` Workflow

```sh
vim myvm.xml
virsh define myvm.xml
virsh start myvm
virsh shutdown myvm
virsh destroy myvm
virsh undefine myvm
```

* Problems:
    * Complex
    * Weird terminology


<!-- .slide: data-state="normal" id="libguestfs" data-timing="60s" data-menu-title="libvirt friends" -->
## `libvirt` Friends

`libguestfs`:

* `guestmount`
* `virt-resize`
* `virt-customize`, `virt-builder`

`virt-install`, `virt-clone`
