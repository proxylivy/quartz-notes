Tengo que reescribir este proyecto desde 0, eso es planeacion, cierto?
Inicio del proyecto: 14-Nov-2024

Lecturas
- https://www.smarthomebeginner.com/traefik-v3-docker-compose-guide-2024/
- https://github.com/mochman/Bypass_CGNAT
- https://jramtech.gitlab.io/post/getting-over-cgnat-wireguard-gce/
- https://github.com/jbencina/vpn
- https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/private-net/cloudflared/
- https://pandorafms.com/blog/es/docker-run/
- https://techmormo.com/posts/docker-made-easy-1-what-is-docker/
- [Github - Mash Playbook](https://github.com/mother-of-all-self-hosting/mash-playbook)


Investigar sobre
- Advance DNS Encryption | Example: [Adguard Home](https://github.com/AdguardTeam/AdGuardHome/wiki/Encryption)
	- DNSSEC, DNS-over-HTTPS, DNS-over-TLS, DNS-over-QUIC 
- GrimD
- Pfsense
- HolePI
- Tailscale
- https://github.com/PacktPublishing/Learning-DevOps-Second-Edition



## Necesidades Servidor
- OS: [Arch Linux](https://archlinux.org/)
	- Discos con soporte de cifrado | Como muestra [Aqui](https://github.com/maximbaz/arch-secure-boot) y [Aqui](https://gist.github.com/huntrar/e42aee630bee3295b2c671d098c81268)
- Full Suport IPv6 (via Router)
- Containers via Podman
- Salir detras de NAT: [Cloudflare Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/), [Rathole](https://github.com/rapiz1/rathole), [PageKit](https://pagekite.net/), [BoringProxy](https://boringproxy.io/) o [Wireguard](https://www.wireguard.com/) | [Wireguard Blog](https://dev.to/tangramvision/what-they-don-t-tell-you-about-setting-up-a-wireguard-vpn-1h2g), tailscale or zerotier ([Reddit Guide](https://www.reddit.com/r/selfhosted/comments/u8n5hz/how_to_bypass_cgnat_and_expose_your_server_to_the/) or [Reddit Guide](https://www.reddit.com/r/selfhosted/comments/m0n2te/yet_another_cgnat_vps_bypass_setup/), [Github - Awesome Tunneling](https://github.com/anderspitman/awesome-tunneling) or [Other Blog](https://shaleenjain.com/blog/wireguard-cgnat-bypass/) or [Another Blog](https://shaleenjain.com/blog/wireguard-cgnat-bypass/))
- Separar particiones
	- Posiblemente ZFS a discos nuevos + 1 SSD
	- Posiblemente LVM para no segmentar discos
- Usuarios para usuarios y permisos
	- Root (Lo mas inactivo y dificil de acceder posible)
	- Normal (Usado lo menos posible)
	- Contenedores (Menor permiso)
- Implementar acceso con llaves seguras
	- ssh public/private keys
	- no password login anywhere
- Reverse Proxy
	- [x] Traefik | [Docs](https://doc.traefik.io/traefik/) | [Docker Hub](https://hub.docker.com/_/traefik)
- Encriptar el contenido en local
	- [x] SSL/TLS | Lets Encrypt | Adguard

## Pasos a Grandes Rasgos
- Documente todo lo que he hecho en github
- Reinstale el sistema operativo
- Reestructure la teoria del funcionamiento de servicios
- Haga diagramas para entender la teoria
- Hacer una guia de instalacion para apegarme
- Completar la topologia de microservicios
- Ver si tengo el conocimiento para poder hacer matrix | [Blog](https://appelman.se/matrix-on-cloudflare/) or [Blog](https://whitematter.tech/posts/encrypted-matrix-server/)

## Way to: Docker & Kubernetes
Kubernetes Learn
- Gateway: [Official blog](https://gateway-api.sigs.k8s.io/)


## Final Boss: Matrix
- Official [Site](https://matrix.org/)
- Official User [Docs](https://matrix.org/docs/chat_basics/matrix-for-im/)
- Official Synapsic [Docs](https://element-hq.github.io/synapse/latest/welcome_and_overview.html)
- Github [Ansible Docker Deploy](https://github.com/spantaleev/matrix-docker-ansible-deploy)

Dudas
- ¿¿Se pueden separar la casa en diferentes Vlan?? de que forma??

Debo Aprender a configurar
- SSH de forma segura de manera local
- SSH de forma segura de manera remota
- Estructura de red de mi casa
- Wireguard + Pfsense routing, Necesita una Multiport NIC slot PCIe x4 para 2 salidas RJ45, uno para LAN y otro se usa en la WAN
- Reverse Proxy to make fastest non NAT connections to my server
- Certification local for my services
- Docker Compose Yaml
- Database
- SD-WAN
- VNF (Virtual Network Functions)

With tools like:
- Wireguard (and a webui)
- Cloudflare (More than ever)
- PFsense & OPNsense
- NAT
- CG-NAT
- Reverse Proxy
- ONT
- Routers & Switchs
- Encryptions End-to-End and Certs
- GPG and Keys, add SSH
- IPv4 & IPv6



Guides: 
- https://homenetworkguy.com/how-to/use-static-routing-to-second-opnsense-router-with-nat-disabled-for-homelab/
- https://forum.netgate.com/topic/140093/setting-up-a-home-lab-need-nat-to-have-internal-virtual-switch-to-go-into-the-internet/4
- https://forums.spacerex.co/t/exposing-homelab-with-wireguard-vpn-over-vps-and-reverse-proxies/163
- https://forums.lawrencesystems.com/t/reverse-proxy-understanding-the-potential-security-issues/18743/42?page=3
- https://blog.cloudflare.com/masque-building-a-new-protocol-into-cloudflare-warp
- https://noted.lol/say-goodbye-to-reverse-proxy-and-hello-to-cloudflare-tunnels/
- https://www.reddit.com/r/WireGuard/comments/10vq2y9/wireguard_through_cloudflare/?rdt=53941
- https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/private-net/cloudflared/
- https://blog.cloudflare.com/zero-trust-warp-with-a-masque
- https://github.com/jbencina/vpn
- https://medium.com/@jbtechmaven/creating-an-isolated-network-between-kali-linux-and-windows-10-vms-35efa7134f0b
- https://www.reddit.com/r/homelab/comments/155hbvl/how_do_i_add_opnsense_to_my_lab_without_getting/