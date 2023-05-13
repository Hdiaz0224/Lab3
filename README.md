# Laboratorio 3
Ivan Gutierrez, Hector Diaz, Juan Deutsch
El objetivo de este laboratorio es diseñar e implementar una red empresarial escalable y segura basada en IPv6 e IPv4, utilizando técnicas de subnetting y configurando servicios de red como DHCPv6, SLAAC y ACL. Además, se busca reconocer y configurar protocolos de enrutamiento dinámico como OSPFv2, OSPFv3, EIGRP y EIGRPv6, y configurar servicios informáticos como HTTP, DNS y SNMP para ofrecer contenidos web y gestionar la red. Finalmente, se busca analizar el flujo de datos de los servicios HTTP, DNS y SNMP a través de la red empresarial para optimizar su rendimiento y asegurar su seguridad.

## Procedimiento:
En esta parte del procedimiento vamos a explicarlo en 4 diferentes partes y es el como organizamos la topología: Internet, DMZ, intranet BOG , Intranet MAD.

### Internet:
En este caso, internet se configuraba con IPV4 y necesitabamos configurar los protocolos EIGRP, OSPF asi como una redistribucion en 2 routers para que conectaran ambos protocolos y finalmente tunneling para que conectara de forma conecta IPV4 con IPV6

### DMZ e INTRANET BOG:
Frente a la configuración del DMZ contestamos unas preguntas para guiarnos entre estas estaban: ¿Cuántos subnets necesitamos?, ¿Cuántos hosts se necesitan?, ¿ Que dispositivos/interfaces hacen parte de la subred?, ¿ Que partes de la red son públicas o privadas?. Después de esto, se realizan las configuraciones básicas a los diferentes switches que se encuentran entre el DMZ y la intranet BOG. Asimismo realizamos una tabla donde se evidencia el dispositivo, interfaz, Direccion IP, WildCard, Mascara de subred y puerta de enlace.

La configuración se resume en qué hay dos servidores los cuales uno es para el servidor web, dónde se está alojando la página y la otra el DNS dónde se está alojando la direccion IP de la misma página web, el ISP_BOG se está encargando de ser parte de la comunicación de routers de la OSPF por el lado izquierdo de la topología. Asimismo, El servidor DNS es el encargado de almacenar la dirección IP del servidor web y proporcionar esta información a los clientes que desean acceder al sitio web. Los clientes envían una solicitud de DNS al servidor DNS, que responde con la dirección IP del servidor web correspondiente.

### Intranet MAD:
En esta parte en teoría era la misma que el Intranet BOG ya que nos encontrábamos con una red compuesta por switches y pc’s. Por lo que la configuración era la de configurar los switches de manera normal sin embargo aquí los pc se configuraron para que tuvieran direcciones estáticas. Todo esto con el fin de que se pudiera conectar sin problema a los servidores que habíamos configurado antes y que se observaba que se ejecutaban de manera correcta.

### Analisis:
Segun lo visto en imagenes presentadas se configuro la red con dos procesos de enrutamiento que fueron EIGRP y OSPF en sus dos versiones de IPv6 e IPv4, iniciando con EIGRP (Enhanced Interior Gateway Routing Protocol) de IPv6 se tiene en cuenta que este, segun los requerimientos se configuro por el lado ESP, que son el ISP_ESP y el R2_ESP de manera que se comunicara el internet de IPv4 con la intranet de Madrid teniendo en cuenta que este tiene una rápida convergencia de rutas, eficiencia de ancho de banda y escalabilidad para una configuracion en caso de que la red sea mas grande ademas que ofrece una autoconfiguración que hace que los dispositivos se descubran automaticamente, siguiendo con la configuracion del OSPF en IPv6 que se realizo por el lado BOG, mas especificamente en ISP_BOG y R1_BOG que de igual manera comunica la red de BOG configurada en IPv6 con el internet que esta configurado con IPv4, con relacion al OSPF que tiene casi las mismas ventajas con relación a EIGRP, con la diferencia de en OSPF la red se segmenta de mejor manera siendo asi que soporta areas de multiple enrutamiento y autenticación de mensajes, siguiendo con la configuracion de enrutamiento en IPv4, se tiene encuenta que en la zona de Internet se tiene que comunicar los protocolos de IPv6 con los de IPv4, para esto se configuro un tunneling para comunicar estas dos redes, en los routers de frontera de BOG y ESP, ya mas puntualmente dentro de IPv4 se configuro tambien los protocolos de EIGRP y OSPF redistribuyendo estos mismos dentro de la red

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/f2f6d9e4-beb3-4b6e-98b1-85a1413437b8)
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/d62492aa-9988-42fe-90c9-cc6ec80eb078)
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/eb26d0d9-90d7-4911-989f-673a63c58500)
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/6f5ce5a2-d3a0-452d-84b5-80282d9513ab)
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/8439670f-df8e-428e-8094-bfe5b170851c)


Con relación a las tablas de enrutamiento de IPv6 e IPv4, como se ve en la imagen, inciando con BOG, usando el comando show ipv6 route se puede ver que tipo de enrutamiento hay presente y si esta conectado o si esta local, en cuando al enrutamiento que nos da noscomunica que esta usando el OSPF para comunicarse en cuando a IPv6 mientras que si usamos el comando de show ip route nos muestra la tabla de enrutamiento en cuanto IPv4, por lo que este nos manda que se esta comunicando principalmente con EIGRP pese que se espera que se comunique con OSPF, aun asi se comunica entre los routers de IPv4 por medio de EIGRP, siguiendo con ESP en cuanto a IPv6 se nos muestra que en principio no tiene un enrutamiento que se pueda ver pero al ingresar el comando show ipv6 eigrp neighbours se nos muestra que se esta comunicando por medio de EIGRP dentro de la red de IPv6, mientras que en IPv4 simplemente se nos muestra que esta efectivamente conectado.

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/f3b22e74-7d41-432b-9b6b-f327773d4af1)


Para dar soporte de gestión de red en ambas Intranet utilizando el estándar SNMP, es necesario configurar el servicio SNMP en los dispositivos de red y en los PCs del Departamento de Tecnología y Vicepresidencia. Para denegar el uso de SNMP a las demás VLANs, es necesario configurar un ACL (Access Control List) en el router que permita el acceso SNMP. Se considero implementación de ACL pero ofrecen una seguridad muy básica, o la implementación de no se qué protocolo pero no se cuantos, por lo que se decidió imolmenete una vpn.


Para proporcionar comunicación segura entre Intranets, se deben configurar servicios de VPN (Virtual Private Network) y encriptación. Los protocolos de VPN que se pueden utilizar incluyen IPSec (Internet Protocol Security) y SSL (Secure Sockets Layer). Además, se puede utilizar la encriptación para proteger los datos transmitidos entre las redes

 Valide las configuraciones y funcionamiento del ítem 1b. Anexe evidencias de la validación realizada. Si en este punto encuentra problema(s) de funcionamiento, descríbalo(s) y proponga una solución al problema(s) identificado(s).
 
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/ab953e0b-b036-467f-9b0e-9fec4c77f139)
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/08820fa6-68bf-474b-a613-b78fdd5fd91d)
Como se evidencia en las anteriores imagenes, se adjunta evidencia de como se validaron las configuraciones con respecto al item 1b cabe recalcar que la validacion se realiza usando el comando show version para demostrar que si esta configurado de la manera correcta

Explique el flujo bidireccional de mensajes SNMP intercambiados entre los dispositivos gestionados desde las diferentes Intranets al utilizar capacidades “get” y “set” sobre una variable MIB de su elección. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/b1c2f2aa-fae4-401f-9d33-90d90f5ba3ac)

GET

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/5d8d21c5-1d68-419c-89c9-d457132078bc)

SET

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/ea31e095-0914-4ba1-bfeb-6201f0baeb22)

En las imagenes anteriores se observa como se intercambian mensajes entre los diferentes dispositivos y que se realiza de manera exitosa.
Se adjunta evidencia del flujo de paquetes y de como se observa que si se capturan y se hace el get y set

![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/6df89391-b818-4d89-a7a9-4b9d29b8d5f9)

Valide la conectividad entre los equipos PCs de cada Intranet, y los ACLs configurados en ambas Intranet para permitir y denegar el acceso al contenido de la página web alojada en el servidor Web del DMZ (punto 4.4 del LA#2). Anexe evidencias de la verificación realizada.
![image](https://github.com/Hdiaz0224/Lab3/assets/93561095/2f1fc50c-7b5b-4b25-aa33-11512b9ea9e1)
Al ser con http bloquea el acceso

### Retos:
En este laboratorio los retos que nos encontramos fueron muy dificiles pero a la vez interesantes de resolver, ya que, teniamos que modificar el laboratorio # 2 en el cual se nos presentaron muchos retos. Asimismo, otro reto mientras desarrollabamos el laboratorio fue el manejo del tiempo ya que teniamos que realizar este laboratorio mientras desarrollabamos el proyecto.

### Conclusiones:
Para concluir aprendimos la implementacion de diferentes servicios asi como tambien terminamos de aprender otros. Esta experiencia fue grata ya que pudimos observar nuevos servicios que no conociamos que se podian implementar en Cisco Packet Tracer
### Video:
https://www.youtube.com/watch?v=bkD4Yxtf-8g

### Configuraciones:
[Configuraciones LAB 3.txt](https://github.com/Hdiaz0224/Lab3/files/11470385/Configuraciones.LAB.3.txt)

### Bitacora
[bitacora de reuniones grupo 6.pdf](https://github.com/Hdiaz0224/Lab3/files/11470387/bitacora.de.reuniones.grupo.6.pdf)

### Referencias:
https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst3650/software/release/3se/ipv6/configuration_guide/b_ipv6_3se_3650_cg/b_ipv6_3se_3650_cg_chapter_0111.html
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/snmp/configuration/xe-16/snmp-xe-16-book/nm-snmp-cfg-snmp-support.html
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/snmp/configuration/xe-16/snmp-xe-16-book/nm-snmp-cfg-snmp-support.html
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_conn_vpnips/configuration/xe-16/sec-sec-for-vpns-w-ipsec-xe-16-book/sec-ipv6-ipv4-gre.html


