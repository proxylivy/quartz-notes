# Info
**E**asy **V**irtual **N**etwork es una solucion Propietaria de Cisco para la virtualizacion de red basada en IP que simplifica la implementacion de [[999 - Archivado/VRF|VRF-Lite]] y mejora sus capacidades, proporciona separacion de trafico y aislamiento de rutas en una infraestructura de red compartida, ofreciendo simplicidad de capa 2 con controles de capa 3.

Utiliza troncales virtuales (vnet tag) para transportar multiples VRFs a travez de una sola interfaz fisica o logica mediante replicacion de rutas, permitiendo que cada VRF acceda a la tabla de enrutamiento compartido y elimiando la necesidad de configuracion de importacion y exportacion de rutas y la creacion de sub-interfaces, EVN facilita el acceso a servicios comunes (Internet, Mail, [[010 - Protocolos/010.3 - Comunicaciones/DHCP|DHCP]], DNS) desde multiples VRFs

EVN proporciona herramientas para facilitar la administracion y solucion de problemas en entornos virtualizados, lo que permite solucionar problemas sin especificar cada Contexto VRF a revisar
