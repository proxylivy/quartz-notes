# Info
**W**ide **A**rea **N**etwork, 

Son criticas para el funcionamiento y fiabilidad de internet, sus fallos son inevitables y se deben minimar sus efectos

Caracteristicas
- Velocidades Terabits/sec
- Enlaces agregados de cientos de enlaces individuales

Problemas
- Alto costo por alta capacidad / uso del enlace: Solo se usa 3-40% y se desperdicia entre 6-70%
- Uso ineficiente de recursos: Todas las aplicaciones y enlaces se tratan de igual forma, lo que es suboptimo para el uso de la capacidad de red
- Falta de control: Capacidad limitada para imponer prioridades relativas variables o segun contexto
- Problemas de escabilidad: Problemas al cambiar la red escalando verticalmente o horizontalmente manteniendo la eficiencia


Los [[020 - Conceptos/020.4 - Dispositivos de Red/Router|Router]] de WAN son equipos high-end especializados con alta disponibilidad.

Se pueden fusionar con tecnologias [[020 - Conceptos/020.3 - Fundamentos/SDN|SDN]] y se convierte en SD-WAN
Si todos los enlaces y aplicaciones usas en mismo enlace, no es ideal a niveles practicos, QoS permite optimizar ese uso segun prioridad y banda de ancha necesaria

Ejemplos en [[020 - Conceptos/020.3 - Fundamentos/SDN#Ejemplos|SDN]]