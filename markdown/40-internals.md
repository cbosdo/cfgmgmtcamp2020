<!-- .slide: data-state="section-break" id="break-internals" data-timing="10s" -->
# Internals


<!-- .slide: data-state="normal" id="connection" data-timing="120s" data-menu-title="`virt` module" -->
## `libvirtd` Connection

* Global config:

```yaml
virt:
  connection:
    uri: esx://10.1.1.101/?no_verify=1&auto_answer=1
    auth:
      username: user
      password: pass
```

* Per-call settings:

```sh
salt 'hypervisor' virt.list_domains connection=lxc:///
```


<!-- .slide: data-state="normal" id="states-design" data-timing="120s" data-menu-title="States Design" -->
## States Design

* `virt.running`:
  * Calls `virt.defined`
  * Calls `virt.start` function

* `virt.defined`:
    * Calls `virt.init` function to create domain
    * Calls `virt.update` function to test update and update

* `pool_` and `network_` prefixes

Note:
* PR pending review


<!-- .slide: data-state="normal" id="events-engine" data-timing="120s" data-menu-title="`libvirt_events` Engine" -->
## `libvirt_events` Engine

* Activate

```yaml
engines:
  - libvirt_events
```

* Config:
  * libvirt URI
  * filter events

* Implementation
  * Register libvirt event callback
  * Convert libvirt structs into Salt events


<!-- .slide: data-state="normal" id="future" data-timing="180s" data-menu-title="Challenges & Future" -->
## Challenges & Future

* Testing
    * Mock-based unit tests
    * No test with real libvirt
    * Integration tests

* More libvirt API to cover
    * Network
    * `virt.init` to handle more disks and network types
    * Refactor migration
    * Snapshots
    * Salt-cloud libvirt provider needs ❤️
