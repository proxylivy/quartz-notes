# Info
> [!TIP] RFC Recomendados
> 0. Fuentes
> 	- [ICANN - DNS-Related RFC](https://rfc-annotations.research.icann.org/)
> 	- [TTKZW DNS RFC (jp)](https://emaillab.jp/dns/dns-rfc/)
> 	- [IETF - DNS as Tech](https://www.ietf.org/technologies/dns/)
> 	- [AS112 Project](https://www.as112.net/)
> 	- [DNS Privacy Project - DNS Privacy Reference Material](https://dnsprivacy.org/dns_privacy_reference_material/)
> 	- [IETF Datatracker](https://datatracker.ietf.org/)
> 1. Comprension
> 	- [RFC 9499 - DNS Terminology](https://datatracker.ietf.org/doc/rfc9499/)
> 	- [RFC 1034 - DNS - Concepts and Facilities](https://datatracker.ietf.org/doc/rfc1034/)
> 	- [RFC 1035 - DNS - Implementation and Specification](https://datatracker.ietf.org/doc/rfc1035/)
> 	- [RFC 2181 - Clarifications to the DNS Specification](https://datatracker.ietf.org/doc/rfc2181/)
> 2. Transporte
> 	- [RFC 7766 - DNS over TCP](https://datatracker.ietf.org/doc/rfc7766/)
> 	- [RFC 7858 - DNS over TLS (DoT)](https://datatracker.ietf.org/doc/rfc7858/) | [RFC 9325 - Recommendation for Secure Use of TLS and DTLS](https://datatracker.ietf.org/doc/rfc9325/)
> 	- [RFC 8484 - DNS over HTTPS (DoH)](https://datatracker.ietf.org/doc/rfc8484/)
> 	- [RFC 9250 - DNS over QUIC (DoQ)](https://datatracker.ietf.org/doc/rfc9250/)
> 3. Operacion Basica
> 	- [RFC 1995 - IXFR](https://datatracker.ietf.org/doc/rfc1995/)
> 	- [RFC 1996 - DNS Notify](https://datatracker.ietf.org/doc/rfc1996/)
> 	- [RFC 2136 - DNS Update](https://datatracker.ietf.org/doc/rfc2136/)
> 	- [RFC 2308 - DNS NCACHE](https://datatracker.ietf.org/doc/rfc2308/) | [RFC 9520 - Negative Caching of DNS Resolution Failures](https://datatracker.ietf.org/doc/rfc9520/)
> 	- [RFC 8020 - NXDOMAIN](https://datatracker.ietf.org/doc/html/rfc8020)
> 	- [RFC 6891 - EDNS](https://datatracker.ietf.org/doc/rfc6891/)
> 4. Seguridad
> 	- [RFC 9364 - DNSSEC](https://datatracker.ietf.org/doc/rfc9364/)
> 	- [RFC 4033 - DNS Security Introduction and Requirements](https://datatracker.ietf.org/doc/rfc4033/)
> 	- [RFC 4034 - DNS Security Extensions](https://datatracker.ietf.org/doc/rfc4034/)
> 	- [RFC 4035 - DNS Security Extension - Protocol Modification](https://datatracker.ietf.org/doc/rfc4035/)
> 	- [RFC 5011 - Automated Updates of DNSSEC Trust Anchors](https://datatracker.ietf.org/doc/rfc5011/)
> 	- [RFC 5452 - Make DNS More Resilient against Forged Answers](https://datatracker.ietf.org/doc/rfc5452/)
> 	- [RFC 9077 - NSEC and NSEC3: TTLs and Aggresive Use](https://datatracker.ietf.org/doc/rfc9077/)
> 5. Privacidad | [WG - dprive (Concluded)](https://datatracker.ietf.org/wg/dprive/about/)
> 	- [RFC 7830 - EDNS(0) Padding Option](https://datatracker.ietf.org/doc/rfc7830/) | [RFC 8467 - Padding Policies for EDNS](https://datatracker.ietf.org/doc/rfc8467/)
> 	- [RFC 8932 - DNS Privacy Service Operators Recommendations](https://datatracker.ietf.org/doc/rfc8932/)
> 	- [RFC 9076 - DNS Privacy Considerations](https://datatracker.ietf.org/doc/rfc9076/)
> 	- [RFC 9156 - QNAME Minimization](https://datatracker.ietf.org/doc/rfc9156/)
> 	- [RFC 9539 - Unilateral Opportunistic Deployment of Encrypted Recursive-to-Authoritative DNS](https://datatracker.ietf.org/doc/rfc9539/)
> 6. Operacion y gestion de DNS | [WG - dnsop](https://datatracker.ietf.org/wg/dnsop/about/)
> 	- [RFC 4697 - Observed DNS Resolution Misbehavior](https://datatracker.ietf.org/doc/rfc4697/)
> 	- [RFC 6303 - Locally Served DNS Zones](https://datatracker.ietf.org/doc/rfc6303/)
> 	- [RFC 6781 - DNSSEC Operation Practices, Version 2](https://datatracker.ietf.org/doc/rfc6781/)
> 	- [RFC 7720 - DNS Root Name Service Protocol and Deployment Requirements](https://datatracker.ietf.org/doc/rfc7720/)
> 	- [RFC 7534 - AS112 Nameserver Operations](https://datatracker.ietf.org/doc/rfc7534/)
> 	- [RFC 8806 - Running a Root Server Local to a Resolver](https://datatracker.ietf.org/doc/rfc8806/)
> 	- [RFC 8906 - A Common Operational Problem in DNS Server: Failure to Communicate](https://datatracker.ietf.org/doc/rfc8906/)
> 	- [RFC 9210 - DNS Transport over TCP - Operational Requirements](https://datatracker.ietf.org/doc/rfc9210/)

> [!TIP] Lecturas Recomendadas
> - [Toolkit - How it works - DNS](https://toolkit.whysonil.dev/how-it-works/dns/)
> - [Cloudflare - What is DNS](https://www.cloudflare.com/learning/dns/what-is-dns/)

**D**omain **N**ame **S**ystem (Sistema de Nombre de Dominio) fue definido por Paul Mockapetris en 1983 mediante los [RFC 882](https://datatracker.ietf.org/doc/rfc882/) y [RFC 883](https://datatracker.ietf.org/doc/rfc883/), que luego con los comentarios del [RFC 973](https://datatracker.ietf.org/doc/rfc973/) dieron lugar a los RFC 1034 y RFC 1035 que definen el funcionamiento base de DNS hasta hoy. Este protocolo ha sido actualizado por muchos RFC posteriores.

Es un sistema distribuido y jerarquico que permite la resolucion de nombres y la obtencion de informacion asociada a dominios, mediante consultas a una infraestructura global de servidores autoritativos y resolutores, con uso intensivo de cache para la eficiencia de resolucion

La resolucion de una consulta involucra dos actores principales, el resolver, que actua en nombre del cliente iniciando y gestionando el proceso de consulta y los servidores autoritativos que mantienen la informacion definitiva sobre cada zona. Entre ambos existe una infraestructura de servidores raiz y TLD que permite navegar la jerarquia distribuida. Este diseño concentra la complejidad de la resolucion, el manejo de fallos y el acceso distribuido en el resolver, mientras que la consistencia y actualizacion de los datos queda responsabilidad de los servidores autoritativos


