# Contenido Semana 7

- [Notación CIDR](#notación-cidr)
- [Subnetting](#subnetting)
  - [Analizando solución](#analizando-solución)
  - [Utilizando VLSM](#utilizando-vlsm)
  - [Utilizando FLSM](#utilizando-flsm)
- [Cálculo de Red](#cálculo-de-red)
  - [Máscara de Subred](#máscara-de-subred)
  - [Wildcard](#wildcard)
  - [ID de Red](#id-de-red)
  - [Dirección de Broadcast](#dirección-de-broadcast)
  - [Primer Host Disponible](#primer-host-disponible)
  - [Ultimo Host Disponible](#ultimo-host-disponible)
  - [Número de Hosts](#número-de-hosts)
  - [Número de Hosts Utilizables](#número-de-hosts-utilizables)
  - [Resumen Final](#resumen-final)
  - [Link para cálculos](#link-para-cálculos)


# Notación CIDR

| CIDR | Mascara Subred  | Host             |    Wildcard     |
| ---- | --------------- | ---------------- | --------------- |
| /0   | 0.0.0.0         |  4,294,967,296   | 255.255.255.255 |
| /1   | 128.0.0.0       |  2,147,483,648   | 127.255.255.255 |
| /2   | 192.0.0.0       |  1,073,741,824   | 63.255.255.255  |
| /3   | 224.0.0.0       |    536,870,912   | 31.255.255.255  |
| /4   | 240.0.0.0       |    268,435,456   | 15.255.255.255  |
| /5   | 248.0.0.0       |    134,217,728   | 7.255.255.255   |
| /6   | 252.0.0.0       |     67,108,864   | 3.255.255.255   |
| /7   | 254.0.0.0       |     33,554,432   | 1.255.255.255   |
| /8   | 255.0.0.0       |     16,777,216   | 0.255.255.255   |
| /9   | 255.128.0.0     |      8,388,608   | 0.127.255.255   |
| /10  | 255.192.0.0     |      4,194,304   | 0.63.255.255    |
| /11  | 255.224.0.0     |      2,097,152   | 0.31.255.255    |
| /12  | 255.240.0.0     |      1,048,576   | 0.15.255.255    |
| /13  | 255.248.0.0     |        524,288   | 0.7.255.255     |
| /14  | 255.252.0.0     |        262,144   | 0.3.255.255     |
| /15  | 255.254.0.0     |        131,072   | 0.1.255.255     |
| /16  | 255.255.0.0     |         65,536   | 0.0.255.255     |
| /17  | 255.255.128.0   |         32,768   | 0.0.127.255     |
| /18  | 255.255.192.0   |         16,384   | 0.0.63.255      |
| /19  | 255.255.224.0   |          8,192   | 0.0.31.255      |
| /20  | 255.255.240.0   |          4,096   | 0.0.15.255      |
| /21  | 255.255.248.0   |          2,048   | 0.0.7.255       |
| /22  | 255.255.252.0   |          1,024   | 0.0.3.255       |
| /23  | 255.255.254.0   |            512   | 0.0.1.255       |
| /24  | 255.255.255.0   |            256   | 0.0.0.255       |
| /25  | 255.255.255.128 |            128   | 0.0.0.127       |
| /26  | 255.255.255.192 |             64   | 0.0.0.63        |
| /27  | 255.255.255.224 |             32   | 0.0.0.31        |
| /28  | 255.255.255.240 |             16   | 0.0.0.15        |
| /29  | 255.255.255.248 |              8   | 0.0.0.7         |
| /30  | 255.255.255.252 |              4   | 0.0.0.3         |
| /31  | 255.255.255.254 |              2   | 0.0.0.1         |
| /32  | 255.255.255.255 |              1   | 0.0.0.0         |

# Subnetting

Una empresa adquirió la red 12.0.0.0/20 y se desea realizar subnetting utilizando FLSM y VLSM a las siguientes áreas

| Vlan |    Departamento    | Cantidad de Host |
| ---- | ------------------ | ---------------- |
|  10  | Linea Blanca       |          3       |
|  20  | Equipos de Sonido  |         12       |
|  30  | Televisores        |          7       |
|  40  | Computadoras       |         69       |
|  50  | Videojuegos        |        212       |

## Analizando solución

Primero tenemos que conocer cuáles son los límites de la red de la que se desea realizar el subnetting para establecer si es posible o no realizarlo.
Para este caso la red principal tine los siguientes datos

|  ID Red  | Mascara Subred |  Wildcard  |  Primer Host  | Ultimo Host | Broadcast   |
| -------- | -------------- | ---------- | ------------- | ----------- | ----------- |
| 12.0.0.0 | 255.255.240.0  | 0.0.15.255 | 12.0.0.1      | 12.0.15.254 | 12.0.15.255 |

Con la tabla anterior podemos establecer los límites de la red, desde el ID de Red, hasta la dirección de Broadcast.

## Utilizando VLSM

Para utilizar VLSM se deben de ordenar los datos de las áreas, desde la mayor cantidad de dispositivos hasta la menor cantidad de dispositivos que se necesitan:

| Vlan |    Departamento    | Cantidad de Host |
| ---- | ------------------ | ---------------- |
|  50  | Videojuegos        |        212       |
|  40  | Computadoras       |         69       |
|  20  | Equipos de Sonido  |         12       |
|  30  | Televisores        |          7       |
|  10  | Linea Blanca       |          3       |

Con esta tabla se busca la máscara de subred mínima que pueda albergar la cantidad de host que se necesitan en cada área.

Para la VLAN 50 se necesitan 212 host, por lo que la máscara de subred que puede albergar dicha cantidad es /24 que otorga 254 host usables por lo que la tabla queda de la siguiente manera:

|    Departamento    |  ID Red  | Mascara Subred |  Wildcard  |  Primer Host  | Ultimo Host | Broadcast   |
| ------------------ | -------- | -------------- | ---------- | ------------- | ----------- | ----------- |
| Videojuegos        | 12.0.0.0 | 255.255.255.0  | 0.0.0.255  | 12.0.0.1      | 12.0.0.254  | 12.0.0.255  |
| Computadoras       |          |                |            |               |             |             |
| Equipos de Sonido  |          |                |            |               |             |             |
| Televisores        |          |                |            |               |             |             |
| Linea Blanca       |          |                |            |               |             |             |

Se continúa con la siguiente área que es la VLAN 40 que necesita 69 host, por lo que la máscara de subred que puede albergar dicha cantidad es /25 que otorga 126 host uables por lo que la tabla queda de la siguiente manera:

|    Departamento    |  ID Red  | Mascara Subred  |  Wildcard  |  Primer Host  | Ultimo Host | Broadcast   |
| ------------------ | -------- | --------------- | ---------- | ------------- | ----------- | ----------- |
| Videojuegos        | 12.0.0.0 | 255.255.255.0   | 0.0.0.255  | 12.0.0.1      | 12.0.0.254  | 12.0.0.255  |
| Computadoras       | 12.0.1.0 | 255.255.255.128 | 0.0.0.127  | 12.0.1.1      | 12.0.1.126  | 12.0.1.127  |
| Equipos de Sonido  |          |                 |            |               |             |             |
| Televisores        |          |                 |            |               |             |             |
| Linea Blanca       |          |                 |            |               |             |             |

De esta forma se sigue llenando la tabla por cada área. Por lo que la tabla final queda de la siguiente manera:

|    Departamento    |   ID Red   | Mascara Subred  |  Wildcard  |  Primer Host  | Ultimo Host | Broadcast   |
| ------------------ | ---------- | --------------- | ---------- | ------------- | ----------- | ----------- |
| Videojuegos        | 12.0.0.0   | 255.255.255.0   | 0.0.0.255  | 12.0.0.1      | 12.0.0.254  | 12.0.0.255  |
| Computadoras       | 12.0.1.0   | 255.255.255.128 | 0.0.0.127  | 12.0.1.1      | 12.0.1.126  | 12.0.1.127  |
| Equipos de Sonido  | 12.0.1.128 | 255.255.255.240 | 0.0.0.15   | 12.0.1.129    | 12.0.1.142  | 12.0.1.143  |
| Televisores        | 12.0.1.144 | 255.255.255.240 | 0.0.0.15   | 12.0.1.145    | 12.0.1.158  | 12.0.1.159  |
| Linea Blanca       | 12.0.1.160 | 255.255.255.248 | 0.0.0.7    | 12.0.1.161    | 12.0.1.166  | 12.0.1.167  |

En esta tabla el broadcast de la última VLAN es `12.0.1.167` por lo que queda dentro de la red principal, por lo que se concluye que sí es posible realizar el subnetting por este método.

## Utilizando FLSM

Para utilizar FLSM se debe de buscar el área que necesite la mayor cantidad de host. Al encontrar este dato, se busca la máscara de subred mínima que pueda albertar la cantidad de host que se necesitan en esta área.

| Vlan |    Departamento    | Cantidad de Host |
| ---- | ------------------ | ---------------- |
|  10  | Linea Blanca       |          3       |
|  20  | Equipos de Sonido  |         12       |
|  30  | Televisores        |          7       |
|  40  | Computadoras       |         69       |
|  50  | Videojuegos        |       `212`      |

El departameno que tiene la mayor cantidad de host es la VLAN 50 que necesita de 212 host, por lo que la máscara de subred que puede albergar dicha cantidad es /24 que otorga 254 host usables. Esta máscara de subred se aplica a todas las áreas, por lo que la tabla final queda de la siguiente manera: 

|    Departamento    |  ID Red  | Mascara Subred |  Wildcard |  Primer Host | Ultimo Host | Broadcast   |
| ------------------ | -------- | -------------- | --------- | ------------ | ----------- | ----------- |
| Linea Blanca       | 12.0.0.0 | 255.255.255.0  | 0.0.0.255 | 12.0.0.1     | 12.0.0.254  | 12.0.0.255  |
| Equipos de Sonido  | 12.0.1.0 | 255.255.255.0  | 0.0.0.255 | 12.0.1.1     | 12.0.1.254  | 12.0.1.255  |
| Televisores        | 12.0.2.0 | 255.255.255.0  | 0.0.0.255 | 12.0.2.1     | 12.0.2.254  | 12.0.2.255  |
| Computadoras       | 12.0.3.0 | 255.255.255.0  | 0.0.0.255 | 12.0.3.1     | 12.0.3.254  | 12.0.3.255  |
| Videojuegos        | 12.0.4.0 | 255.255.255.0  | 0.0.0.255 | 12.0.4.1     | 12.0.4.254  | 12.0.4.255  |

En esta tabla el broadcast de la última VLAN es `12.0.4.255` por lo que queda dentro de la red principal, por lo que se concluye que sí es posible realizar el subnetting por este método.


# Cálculo de Red
Esta sección es para que entiendan cómo poder obtener los datos de una red con una IP y su máscara de subred. No es necesario que lo hagan en los ejercicios.

Para el siguiente ejemplo, tenemos una computadora con una ip 10.217.123.7 /20 y queremos determinar:
1. ID de Red
2. Máscara de Subred
3. Wildcard
4. Primer Host Disponible
5. Último Host Disponible
6. Dirección de broadcast
7. Número de hosts
8. Número de host utilizables

Para ello realizamos los siguientes cálculos:

## Máscara de Subred

Esta se determina por la cantidad de Bits significativos que indica la notación CIDR. En esta caso la notación es `/20` por lo que son 20 bits significativos los que se deben de colocar en binario

Máscara de Subred /20 = `11111111.11111111.11110000.00000000`

Luego con este resultado, cada octeto se convierte en decimal y el resultado será la máscara de subred

|           |   |     |
| --------- | - | --- |
| 11111111  | = | 255 |
| 11111111  | = | 255 |
| 11110000  | = | 240 |
| 00000000  | = | 0   |

Máscara de SubRed = `255.255.240.0`

## Wildcard

La wildcard es el valor inverso de la máscara de subred. Esta la obtenemos negando cada bit de la máscara de subred.

Máscara de Subred /20 = `11111111.11111111.11110000.00000000`

Wildcard = `00000000.00000000.00001111.11111111`

Luego con este resultado, cada octeto se convierte en decimal y el resultado será la wildcard

|           |   |     |
| --------- | - | --- |
| 00000000  | = | 0   |
| 00000000  | = | 0   |
| 00001111  | = | 15 |
| 11111111  | = | 255 |

Wildcard = `0.0.15.255`

## ID de Red
Para obtener el ID de la red tenemos que pasar la dirección IP y la máscara de subred a binario

IP: `10.217.123.7` = `00001010.11011001.01111011.00000111`

Máscara de subred = `11111111.11111111.11110000.00000000`

Con estos dos datos se realiza una operación AND de cada bit en cada octeto

|           | Operacion | Binario                             |
| --------- | --------- | ----------------------------------- |
| IP        |           | 00001010.11011001.01111011.00000111 |
| Máscara   | AND       | 11111111.11111111.11110000.00000000 |
| Resultado | =         | 00001010.11011001.01110000.00000000 |

La dirección Resultante se convierte en decimal nuevamente y el resultado será el ID de la Red a la que pertenece esa IP

|           |   |     |
| --------- | - | --- |
| 00001010  | = | 10 |
| 11011001  | = | 217 |
| 01110000  | = | 112 |
| 00000000  | = | 0   |

ID de Red = `10.217.112.0`

## Dirección de Broadcast

Para obtener la dirección de Broadcast se realiza una suma (recordando que en cada octeto el valor máximo es 255) entre la dirección de Red y la Wildcard

|           | Operacion | Dirección      |
| --------- | --------- | ---------------|
| ID de Red |           | 10.217.112.0   |
| Wildcard  | +         |  0.0.15.255    |
| Resultado | =         | 10.217.127.255 |

Broadcast = `10.217.127.255`

## Primer Host Disponible

Para obtener el primer host disponible se suma una dirección al ID de Red

ID de Red = `10.217.112.0`

Primer Host Disponible = `10.217.112.1`

## Ultimo Host Disponible

Para obtener el ultimo host disponible se resta una dirección al Broadcast

Broadcast = `10.217.127.255`

Ultimo Host Disponible = `10.217.127.254`

## Número de Hosts

Para obtener la cantidad de Host en una red, se toma el valor de cada octeto en la wildcard y se le suma 1 a cada octeto y luego se multiplican los valores restantes

Wildcard = `0.0.15.255`

0 + 1 = 1

0 + 1 = 1

15 + 1 = 16

255 + 1 = 256

1 * 1 * 16 * 256 = 4096

Número de Host = `4096`

## Número de Hosts Utilizables

Para obtener la cantidad de Host Utilizables, solo se restan dos direcciones a la cantidad de Host ya que se deben de quitar el ID de la Red y la dirección de Broadcast.

Número de Host Utilizables = Número de Host - 2

Número de Host Utilizables = `4094`

## Resumen Final

| Dato                        | Valor            |
| --------------------------- | ---------------- |
| ID de Red                   | `10.217.112.0`   |
| Máscara de SubRed           | `255.255.240.0`  |
| Wildcard                    | `0.0.15.255`     |
| Primer Host Disponible      | `10.217.112.1`   |
| Último Host Disponible      | `10.217.127.254` |
| Dirección de Broadcast      | `10.217.127.255` |
| Número de Hosts             | `4096`           |
| Número de Hosts Utilizables | `4094`           |

## Link para cálculos
Estos cálculos de red se pueden realizar de forma práctica desde https://www.calculator.net/ip-subnet-calculator.html
