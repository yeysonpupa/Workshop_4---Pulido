# Workshop 4 - Yeyson Esteban Pulido Parra
### Automatización y Control de Procesos

## Introducción 

Este caso de estudio se centra en la aplicación de la automatización industrial para monitorear los niveles de tanques de líquidos químicos, guiado específicamente por la pregunta ¿Cómo se puede utilizar la automatización industrial para automatizar el monitoreo de los niveles de tanques de líquidos químicos, reduciendo así tanto el consumo de energía como los residuos líquidos?

Para abordar dicha problemática, se propone el diseño de un circuito lógico que servirá como base para la creación de un sistema de control automatizado. Este circuito se traducirá posteriormente a lógica de escalera (Ladder Logic), un lenguaje de programación comúnmente utilizado en entornos industriales para controlar sistemas de automatización PLC [1]. Para validar la eficacia de este enfoque, se llevará a cabo la implementación de un prototipo físico, permitiendo así una evaluación práctica del funcionamiento del sistema propuesto.

## Definición del Problema

La representación visual de la pregunta problema se ve descrita en la Figura 1, donde se evidencia que el tanque de químicos tiene 3 niveles, definidos por B1 para cuando el tanque está vacío, B2 cuando el tanque tiene el mínimo nivel de llenado y B3 cuando el tanque está desbordado.

De igual manera, se tienen 5 alertas visuales, cada una representando algún estado del tanque. Primero, H1 alerta cuando el nivel de llenado es correcto, H2 cuando el nivel es muy bajo, H3 cuando el nivel es muy alto, H4 cuando el tanque está vacío y H5 se activa en caso de un estado de señal que no tenga sentido debido a un sensor defectuoso, indicando un error.

<img width="855" alt="Captura de pantalla 2024-03-27 120606" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/c8143e92-9a20-4934-8aef-39a26729645a">

Figura 1: Representación visual del planteamiento del problema [2].

## Circuito Lógico

En los aprendizajes de este curso, se vió como era posible llevar cualquier solución desde una solución basada en compuertas lógicas a una programación PLC, por eso el primer paso para diseñar la implementación es generar una lógica funcional basada en compuertas lógicas. Es evidente que cada variable del sistema (B1, B2, B3, H1, H2, H3, H4 y H5) tiene un comportamiento booleano, es decir, que únicamente toma valores de verdadero o falso, traducidos a su equivalente numérico 1 y 0, gracias a esto, es posible crear una tabla de verdad que ayude a definir los diferentes estados lógicos que puede tomar cada combinación de variables en el sistema, tal como se observa en la Figura 2.

<img width="560" alt="Captura de pantalla 2024-03-27 123758" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/abf5f15b-a7e9-4b32-b3ca-36ce08b019e9">

Figura 2: Tabla de verdad.

Con base a la tabla de verdad, se decide cuales son las compuertas lógicas que ayudan a generar las salidas deseadas según el estado de las entradas B1, B2 y B3. A continuación, se desgloza para cada salida la configuración ideal, además, se utiliza el software Proteus para validar las propuestas.

* Para H1 - Nivel de llenado correcto:
Para que H1 esté activo, B1 y B2 deben estar con un valor de 1, lo que consecuentemente indica que B3 debe permanecer negado, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B1 y B2 llegarán directamente pero B3 llegará después de pasar por una compuerta NOT, lo que garantiza que el LED correspondiente a H1 se encienda unicamente cuando B1 y B2 estén activas, tal como se observa en la Figura 3.

<img width="567" alt="Captura de pantalla 2024-03-27 135551" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/f38ee6c8-83f7-4e9e-9745-04c501bd1aa5">

Figura 3: Configuración para H1 utilizando compuertas AND y NOT.

* Para H2 - Nivel de llenado bajo:
Para que H2 esté activo, únicamente B1 debe estar con un valor de 1, lo que consecuentemente indica que B2 y B3 deben permanecer negadas, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B1 llegará directamente pero B2 y B3 llegarán después de pasar cada una por una compuerta NOT, lo que garantiza que el LED correspondiente a H2 se encienda unicamente cuando B1 está activa, tal como se observa en la Figura 4.

<img width="523" alt="Captura de pantalla 2024-03-27 135600" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/b3b49ac0-cbca-4d68-8b96-8a9df122ef78">

Figura 4: Configuración para H2 utilizando compuertas AND y NOT.

* Para H3 - Nivel de llenado alto:
Para que H3 esté activo, todas las entradas deben estar activas, así que el procedimiento es sencillo, pues sólo se deben capturar las señales directamente en una compuerta AND, lo que garantiza que el LED correspondiente a H3 se encienda unicamente cuando B1, B2 Y B3 tengan un valor de 1, tal como se observa en la Figura 5.

<img width="525" alt="Captura de pantalla 2024-03-27 135610" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/7cf65a52-498f-4af5-9f45-7e04528e08b3">

Figura 5: Configuración para H3 utilizando compuertas AND y NOT.

* Para H4 - Tanque vacío:
Se sabe que para que H4 esté activo, todas las entradas deben tener un valor booleano de 0. Para lograr esto, se puede tener una primera opción, que sería utilizar una compuerta OR que reciba las entradas y negar la salida con una compuerta NOT, lo cual efectivamente garantiza que el LED correspondiente a H1 se encienda unicamente cuando todas las entradas están apagadas, tal como se observa en la Figura 6.

<img width="461" alt="Captura de pantalla 2024-03-27 130905" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/22b41ae8-1169-40bf-9442-26579077e859">

Figura 6: Configuración para H4 utilizando compuerta OR y NOT.

Sin embargo, con el objetivo de simplificar la cantidad de compuertas lógicas utilizadas, se sabe que la compuerta NOR realiza la negación de las entradas sin necesidad de utilizar una compuerta NOT adicional como en la Figura 6, por lo que, para esta configuración se concluye con el uso de la compuerta NOR tal como se observa en la Figura 7.

<img width="411" alt="Captura de pantalla 2024-03-27 135623" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/4bb48f4d-18b1-4332-9aa8-f0e40569c9c9">

Figura 7: Configuración para H4 utilizando compuerta NOR.

* Para H5 - Error:
A diferencia de las otras salidas, H5 no tiene un único caso de activación, sino 4, como se analizó en la tabla de verdad, pero el procedimiento no cambia mucho, pues la lógica sigue basándose en compuertas AND y NOT.

En el primer caso, para que H5 esté activo, B2 y B3 deben estar con un valor de 1, lo que consecuentemente indica que B1 debe permanecer negado, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B2 y B3 llegarán directamente pero B1 llegará después de pasar por una compuerta NOT, lo que garantiza que el LED correspondiente a H5 se encienda unicamente cuando B2 y B3 estén activas, tal como se observa en la Figura 8.

<img width="517" alt="Captura de pantalla 2024-03-27 140116" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/2f78bb14-67b6-4079-b6e9-7e35eb5580e5">

Figura 8: Configuración para el primer caso de H5 utilizando compuertas AND y NOT.

En el segundo caso, para que H5 esté activo, únicamente B3 debe estar con un valor de 1, lo que consecuentemente indica que B1 y B2 deben permanecer negadas, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B3 llegará directamente pero B1 y B2 llegarán después de pasar cada una por una compuerta NOT, lo que garantiza que el LED correspondiente a H5 se encienda unicamente cuando B3 está activa, tal como se observa en la Figura 9.

<img width="510" alt="Captura de pantalla 2024-03-27 140125" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/f6c5a623-97e7-484d-abab-e7329680162b">

Figura 9: Configuración para el segundo caso de H5 utilizando compuertas AND y NOT.

En el tercer caso, para que H5 esté activo, B1 y B3 deben estar con un valor de 1, lo que consecuentemente indica que B2 debe permanecer negado, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B1 y B3 llegarán directamente pero B2 llegará después de pasar por una compuerta NOT, lo que garantiza que el LED correspondiente a H5 se encienda unicamente cuando B1 y B3 estén activas, tal como se observa en la Figura 10.

<img width="759" alt="Captura de pantalla 2024-03-27 140135" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/02c81175-95ed-486f-8df8-0ebfeb6d6490">

Figura 10: Configuración para el tercer caso de H5 utilizando compuertas AND y NOT.

En el cuarto y último caso, para que H5 esté activo, únicamente B2 debe estar con un valor de 1, lo que consecuentemente indica que B1 y B3 deben permanecer negadas, para esto se utiliza una compuerta AND que recibirá las 3 entradas, donde B2 llegará directamente pero B1 y B3 llegarán después de pasar cada una por una compuerta NOT, lo que garantiza que el LED correspondiente a H5 se encienda unicamente cuando B2 está activa, tal como se observa en la Figura 11.

<img width="732" alt="Captura de pantalla 2024-03-27 140149" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/bfd0f8dc-1ecc-4789-901e-b332337b5daa">

Figura 11: Configuración para el cuarto caso de H5 utilizando compuertas AND y NOT.

Todos estos casos por el momento están configurados de manera individual, pero es evidente que muchos hacen uso de las mismas señales una y otra vez, por lo que, para poder simplificar la cantidad de compuertas lógicas necesarias, se realiza una nueva composición que reuna los 4 casos en un sólo circuito, para esto es indispensable usar una compuerta OR que reciba todas las salidas de cada caso y arroje un único caso que garantice que el LED correspondiente a H5 se encienda. La solución se ve representada en la Figura 12, donde se observa que cada caso funciona correctamente.

![Imagen1](https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/599feb40-c074-476f-ad47-7040de7091b5)

Figura 12: Configuración definitiva para H5 utilizando compuertas AND, NOT y OR.

Ahora que se tienen las configuraciones para cada LED, es necesario unirlos en un único circuito, con tal de una vez más reducir la cantidad de compuertas aprovechando las señales entre los circuitos. Por lo que, finalmente la solución del circuito lógico para el tanque queda como se muestra en la Figura 13 y la respectiva demostración para cada caso de uso en la Figura 14.

<img width="548" alt="Captura de pantalla 2024-03-27 141139" src="https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/7b237616-36f4-40d0-af12-2929b68ad940">

Figura 13: Circuito Lógico del tanque.

![Imagen1](https://github.com/yeysonpupa/Workshop_4---Pulido/assets/101272542/61244953-9d1b-4566-83a6-79a3c2e4a0bb)

Figura 14: Demmostración de cada posible caso en el circuito Lógico del tanque.

## Ladder Logic

Una vez realizado el circuito lógico, se puede traducir el comportamiento generado por cada compuerta lógica a la Lógica Ladder, para esto

### Referencias 

[1] Ladder Logic World. (2023). "Ladder Logic Basics". [En línea]. Disponible en: [http://instrumentasi.lecture.ub.ac.id/full-scale-input-and-output/](https://ladderlogicworld.com/ladder-logic-basics/) (Accedido Marzo 27, 2024).

[2] Aranda, J. (2024). "Automatización & Control de Procesos - PLC – Programming – Ladder Logic (LD)". (Accedido Marzo 27, 2024). 
