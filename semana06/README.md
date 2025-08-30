# Contenido Semana 6

- [EtherChannel](#etherchannel)
  - [Estático (Manual)](#estático-manual)
  - [LACP](#lacp-link-aggregation-control-protocol)
  - [PAGP](#pagp-port-aggregation-protocol)
  - [Verificación de la configuración](#verificación-de-la-configuración)

# EtherChannel

También conocido como Agregación de Enlaces o como Port Channel, es una técnica para combinar múltiples enlaces físicos en uno lógico con el objetivo de aumentar el ancho de banda, mejorar la redundancia y equilibrar la carga de tráfico entre los enlaces.

EtherChannel puede configurarse utilizando distintos modos de configuración:

## Estático (Manual)

Se configuran manualmente los enlaces sin ningún protocolo de negociación. Es sencilla, pero no verifica si los enlaces están correctamente conectados en ambos extremos.

La configuración admite las siguientes configuraciones:


| Modo en el primer switch | Modo en el segundo switch | Estado del EtherChannel  |
|:------------------------:|:-------------------------:|:------------------------:|
| **ON**                   | **ON**                    | Se forma EtherChannel    |
| **ON**                   | Cualquier otro modo       | No se forma EtherChannel |


Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# En ambos switches
enable
configure terminal
interface Port-Channel 1 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
# Se puede configurar modo troncal, VLAN Nativa y las VLANs con las que trabajará la interfaz lógica
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 1 mode on # Se coloca el mismo número que el creado (en este ejemplo 1)
exit
do write # Guarda la configuracion
```

## LACP (Link Aggregation Control Protocol)

Protocolo estándar que permite la negociación automática de enlaces entre dispositivos compatibles. Se utiliza en entornos mixtos (no solo Cisco).

La configuración admite las siguientes configuraciones:


| Modo en el primer switch | Modo en el segundo switch | Estado del EtherChannel  |
|:------------------------:|:-------------------------:|:------------------------:|
| **active**               | **active**                | Se forma EtherChannel    |
| **active**               | **passive**               | Se forma EtherChannel    |
| **passive**              | **passive**               | No se forma EtherChannel |

Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# Switch 1
enable
configure terminal
interface Port-Channel 2 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
# Se puede configurar modo troncal, VLAN Nativa y las VLANs con las que trabajará la interfaz lógica
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 2 mode active # Se coloca el mismo número que el creado (en este ejemplo 2)
exit
do write # Guarda la configuracion

# Switch 2
enable
configure terminal
interface Port-Channel 2 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
# Se puede configurar modo troncal, VLAN Nativa y las VLANs con las que trabajará la interfaz lógica
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 2 mode [active/passive] # Se coloca el mismo número que el creado (en este ejemplo 2)
exit
do write # Guarda la configuracion
```

## PAGP (Port Aggregation Protocol)

Protocolo propietario de Cisco similar a LACP, diseñado para facilitar la configuración automática de EtherChannel entre dispositivos Cisco.

La configuración admite las siguientes configuraciones:


| Modo en el primer switch | Modo en el segundo switch | Estado del EtherChannel  |
|:------------------------:|:-------------------------:|:------------------------:|
| **desirable**            | **desirable**             | Se forma EtherChannel    |
| **desirable**            | **auto**                  | Se forma EtherChannel    |
| **auto**                 | **auto**                  | No se forma EtherChannel |

Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# Switch 1
enable
configure terminal
interface Port-Channel 3 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 3 mode desirable # Se coloca el mismo número que el creado (en este ejemplo 3)
exit
do write # Guarda la configuracion

# Switch 2
enable
configure terminal
interface Port-Channel 3 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 3 mode [auto/desirable] # Se coloca el mismo número que el creado (en este ejemplo 3)
exit
do write # Guarda la configuracion
```
## Verificación de la configuración

Para mostrar el tipo de configuración que se utiliza, se puede utilizar el siguiente comando en el modo EXEC Privilegiado:

```bash
show etherchannel summary
```
