# Que es?
Una opcion mas ligera que los [[020 - Conceptos/020.4 - Dispositivos de Red/IPS|IPS]] son los Instrusion Detection Systems (IDS) o Sistema de Deteccion de Intrusos es un dispositivo para analisis trafico copiado, solo analiza informacion que le mandan a el, solo manda alertas, funciona comparando las firmas offline de virus, no tiene peso sobre la red ya que no analiza en tiempo real.

## Comparacion
Nota: a [[020 - Conceptos/020.4 - Dispositivos de Red/IPS#Comparacion|IPS]]
Ventajas
- No genera Latencia o Jitter
- La sobrecarga no afecta la red

Desventajas
- La respuesta no detiene los paquetes malignos
- Se requiere un ajuste exacto para que responda bien
- Se debe tener una politica de seguridad bien definida
- Mas Vulnerable a Tecnicas de Evacion (Fragmentacion, Tunelizado y Cifrado)

## Tipos
HIDS (Host Instrusion Detections System)
- [OSSEC](https://www.ossec.net/)

NIDS (Network Instrusion Detections System)
- [Snort](https://www.snort.org/)
- [Suricata](https://suricata.io/)
- [Zeek](https://zeek.org/)
- [Kismet](https://www.kismetwireless.net/)