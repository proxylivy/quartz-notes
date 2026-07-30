Nota 2: Radius tiene la mejor seguridad de contraseña pero lo demas lo envia en texto plano, usa los puertos en UDP 1645 o 1812 para la autenticacion con autorizacion y el 1646 o 1813 para la auditoria.

|                    | Radius                                                                     |
| ------------------ | -------------------------------------------------------------------------- |
| Funcionalidad      | Combina Autenticacion con Autorizacion<br>Separa Auditoria, menos flexible |
| Estandar           | Estandar Abierto/RFC                                                       |
| UDP/TCP?           | UDP                                                                        |
| CHAP               | Desafio Unidireccional                                                     |
| Soporte Protocolo  | No ARA, No NetBEUI                                                         |
| Confidencialidad   | Contraseña Cifrada                                                         |
| Personalizacion    | No tiene opciones para autorizar                                           |
| Registro Auditoria | Extensivo                                                                  |