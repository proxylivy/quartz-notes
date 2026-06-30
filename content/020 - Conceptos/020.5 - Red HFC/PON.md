# Info
**P**assive **O**ptical **N**etwork (Red Optica Pasiva) es una red de acceso basadas en  arquitecturas FTTx que utiliza [[020 - Conceptos/020.5 - Red HFC/Fibra Optica|Fibra Optica]] y [[020 - Conceptos/020.5 - Red HFC/Elementos Pasivos|Elementos Pasivos]] (como splitters) en su red de distribucion para conectar multiples usuarios sobre una infraestructura compartida

> [!TIP] Sobre FTTx
> la "x" indica hasta donde llega la fibra optica
> - FTTN (Fiber To The Node): Hasta un nodo central del vecindario
> - FTTC (Fiber To The Curb): Hasta la Acera
> - FTTB (Fiber To The Building): Hasta el piso administrativo del edificio
> - FTTH (Fiber To The Home): Directamente al Hogar

Su estructura fisica se compone en orden secuencial
1. OLT: Central del proveedor, punto de origen de la red optica
2. Feeder: Tramo troncal desde el OLT a la distribucion, sin division de señal (1:1)
3. Distribucion: Divide la señal mediante splitters opticos (ej: 1:32, 1:64, 1:128)
4. Drop: Tramo final desde la distribucion hasta el hogar ("Ultima Milla")
5. ONT: Convierte la señal optica en electricas (Ethernet, Wifi, etc.) y viceversa

Tiene 2 grandes variantes, GPON y EPON

- GPON - Gigabit PON (Asimetrico)
	- Encapsulacion: GEM (GPON Encapsulation Method) -> GTC (GPON Transmission Convergence) sobre Fibra Optica
	- Entidad Normativa: ITU-T
	- Working Group: [ITU-T G Series: Transmission systems and media, digital systems and networks](https://www.itu.int/rec/T-REC-G/en)
	- Longitud de Ondas
		- 1G: 1490 nm ↓ /1310 nm ↑
		- 10G: 1577 nm ↓ / 1270 nm ↑
		- 50G: 1340~1344 nm ↓ / 1298~1302 nm ↑ 
	- Normas
		- [G.984.1](https://www.itu.int/rec/T-REC-G.984.1) (2010): GPON estandar (1G-PON)
		- [G.987](https://www.itu.int/rec/T-REC-G.987) (2012): XG-PON1 para 10G-PON (10G↓, 2.5G↑)
		- [G.9807.1](https://www.itu.int/rec/T-REC-G.9807.1/en) (2023): XGS-PON para 10G-PON Simetrico
		- [G.989](https://www.itu.int/rec/T-REC-G.989) (2015): NG-PON2 para 40G-PON
		- [G.9804.1](https://www.itu.int/rec/T-REC-G.9804.1) (2019): HSP para 50G-PON (50G↓, 12.5G↑)
- EPON - Ethernet PON (Simetrico)
	- Encapsulacion: Ethernet sobre Fibra Optica
	- Entidad Normativa: IEEE
	- Working Group: [IEEE 802.3](https://www.ieee802.org/3/)
	- Normas
		- [802.3ah-2004](https://standards.ieee.org/standard/802_3ah-2004.html) | [WG](https://www.ieee802.org/3/ah/) (1GE): GEPON estandar
		- [802.3av-2009](https://standards.ieee.org/ieee/802.3av/4060/) | [WG](https://www.ieee802.org/3/av/) (10GE)
		- [802.3bk-2013](https://web.archive.org/web/20131217221237/http://standards.ieee.org/findstds/standard/802.3bk-2013.html) | [WG](https://www.ieee802.org/3/bk/) (10GE mejor definido)
		- [802.3ca-2020](https://standards.ieee.org/ieee/802.3ca/7440/) | [WG](https://www.ieee802.org/3/ca/) (25GE y 50GE)
		- 802.3dj (En Desarrollo) | [NOTE](https://www.ieee802.org/3/dj/index.html) (200G,400G,800G,1.6T) (2026)