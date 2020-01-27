<!-- .slide: data-state="section-break" id="section-break-1" data-timing="10s" -->
# Salt states for dummies

Note:
* Software Defined VMs


<!-- .slide: data-state="normal" id="writing-salt-states" data-timing="240s" data-menu-title="Writing Salt States" -->
## Writing Salt States

* Idempotent
* Define the target setup:

```yaml
install_webserver:
  pkg:
    - installed
      - name: apache2
```

https://docs.saltstack.com/en/latest/ref/states/all/index.html

Note:
* Lots of shipped states
* GitOps


<!-- .slide: data-state="normal" id="top.sls" data-timing="240s" data-menu-title="Top.sls" -->
## `top.sls` file

* ``/srv/salt/top.sls``
* Map state files to machines

```yaml
base:
  'web-*':
    - webserver
```

* Matching grains

```yaml
base:
  'role:webserver'
    - match: grain
    - webserver
```


<!-- .slide: data-state="normal" id="applying-states" data-timing="240s" data-menu-title="Applying Salt States" -->
## Salt States

* Bypassing `top.sls` rules

```sh
salt 'test-web-*' state.apply webserver
```

* Applying `top.sls` rules

```sh
salt '*' state.apply
```


<!-- .slide: data-state="normal" id="advanced-states" data-timing="240s" data-menu-title="Advanced States" -->
## Advanced States

* Dependencies

```yaml
/etc/apache2/conf.d/server.conf:
  file:
    - managed
    - source: salt://webserver/server.conf
    - require:
      - pkg: install_webserver
```

* Templating

```yaml
install_webserver:
  pkg:
    - installed
    {% if grains['os'] == 'RedHat' %}
        name: httpd
    {% else %}
      - name: apache2
    {% endif %}
```

Note:
* Require is not the only one
