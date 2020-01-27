<!-- .slide: data-state="section-break" id="section-break-1" data-timing="10s" -->
# Salting Your VMs

Note:
* Software Defined VMs


<!-- .slide: data-state="normal" id="vm-definition-state" data-timing="120s" data-menu-title="VM Definition" -->
## VM definition

```yaml
srv01:
  virt.running:
    - name: srv01
    - cpu: 1
    - mem: 1024
    - disk: my-disk-profile
    - disks:
      - name: system
        model: virtio
        format: qcow2
        image: https://url.to/a/template.qcow2
        pool: pool0
        size: 122880
    - nic: my-nics-profile
    - interfaces:
      - name: eth0
        type: network
        source: net0
    - graphics:
        type: vnc
    - seed: True
```

Note:
* image === template
* size: uses ``virt-resize``
* disks: need to use pools
* Disk size and mem: in MiB
* 122880 MiB = 120 GiB
* seed: injecting Salt minion config.
* PR pending review!


<!-- .slide: data-state="normal" id="pool-definition" data-timing="120s" data-menu-title="Pool Definition" -->
## Storage pool definition

```yaml
pool0:
  virt.pool_running:
    - name: pool0
    - ptype: netfs
    - target: /vms
    - source:
        dir: /srv/vms
        hosts:
          - mirror.tf.local
        format: nfs
    - autostart: True
```

* All libvirt types handled!

Note:
* support for all libvirt types
* libvirt XML data as YAML!


<!-- .slide: data-state="normal" id="abstracting-secrets" data-timing="60s" data-menu-title="Abstracting Secrets" -->
## Abstracting Secrets

```yaml
rbd_pool:
  virt.pool_running:
    - name: ses-pool
    - ptype: rbd
    - source:
        name: {{ pillar['ceph_store'] }}
        hosts:
          - ses2.tf.local
          - ses3.tf.local
        auth:
          username: {{ pillar['ceph_user'] }}
          password: {{ pillar['ceph_key'] }}
```



<!-- .slide: data-state="normal" id="net-definition" data-timing="120s" data-menu-title="Net Definition" -->
## Network definition
```yaml
net0:
  virt.network_running:
    - name: net0
    - bridge: net0-br
    - forward: nat
    - ipv4_config:
        cidr: 192.168.44.0/24
        dhcp_ranges:
          - start: 192.168.44.10
            end: 192.168.44.25
    - autostart: True
```

* Handled network types: NAT, Bridged
* Missing IP/DNS fine tuning


<!-- .slide: data-state="normal" id="chaining" data-timing="60s" data-menu-title="Putting it together" -->
## Putting it together

```yaml
srv01:
  virt.running:
    ...
    - require:
      - virt: pool0
      - virt: net0
```


<!-- .slide: data-state="normal" id="templating" data-timing="120s" data-menu-title="To The Next Level" -->
## To The Next Level

```yaml
{{pillar['vm_name']}}:
  virt.running:
    ...
    - require:
      - virt: pool0
    {% if "vm_net" in pillar %}
      - virt: {{pillar['vm_net']}}
    {% endif %}
```
Note:
* Jinja, renderers
* Pillar data
* Store it all in git with hooks / PRs
* Combine with Salt inside the VM!


<!-- .slide: data-state="normal" id="libvirt-events" data-timing="60s" data-menu-title="`libvirt` Events" -->
## `libvirt` Events
```json
salt/engines/libvirt_events/qemu/system/domain/lifecycle	{
    "_stamp": "2020-02-03T13:36:09.172789",
    "cmd": "_minion_event",
    "data": {
        "detail": "booted",
        "domain": {
            "id": 1,
            "name": "srv01",
            "uuid": "e7a466fd-45ab-45e8-bb78-07845eab8398"
        },
        "event": "started",
        "uri": "qemu:///system"
    },
    "id": "dev-min-kvm.tf.local",
    "pretag": null,
    "tag": "salt/engines/libvirt_events/qemu/system/domain/lifecycle"
}
```
