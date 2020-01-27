<!-- .slide: data-state="section-break" id="break-uyuni" data-timing="10s" -->
# Use Case: Uyuni


<!-- .slide: data-state="normal" class="full-screen" id="salar-de-uyuni" data-menu-title="Salar de Uyuni" data-timing="120" -->
<a title='By Luca Galuzzi [CC BY-SA 2.5](https://creativecommons.org/licenses/by-sa/2.5/)'
  href='https://commons.wikimedia.org/wiki/File:Piles_of_Salt_Salar_de_Uyuni_Bolivia_Luca_Galuzzi_2006_a.jpg'>
  <img alt="Salar de Uyuni" data-src="images/salar_uyuni.jpg"/>
</a>

Note:
* Fork of Spacewalk
* Upstream of SUSE Manager
* Uses Salt hence the name


<!-- .slide: data-state="normal" id="uyuni" data-menu-title="Uyuni" data-timing="60" -->
## Systems Management
<img alt="All Systems" data-src="images/systems_all.png"/>

Note:
* Managing systems
* Content lifecycle management
* See other Uyuni talks


<!-- .slide: data-state="normal" id="uyuni-virt" data-timing="120s" data-menu-title="Uyuni and Virtualization" -->
## Uyuni and Virtualization
<img alt="Virtual guests" data-src="images/uyuni-vms.png"/>

Note:
* actions -> Salt States
* libvirt-events engine


<!-- .slide: data-state="normal" id="virtual-console" data-timing="120s" data-menu-title="Virtual Console" -->
<img alt="Virtual console" data-src="images/display-console.png"/>

Note:
* Almost no use of salt here!


<!-- .slide: data-state="normal" id="uyuni-pools" data-timing="120s" data-menu-title="Uyuni and Virtual Storage" -->
<img alt="Virtual storage" data-src="images/uyuni-pools.png"/>

Note:
* List: directly fetched from Salt virt module
* Actions: Salt States
