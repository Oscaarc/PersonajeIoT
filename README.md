# PersonajeIoT
Se realizará un proyecto donde se diseñara un personaje alusivo a un nacimiento, de donde elegimos a un posadero. Dicho personaje cumplirá con los requisitos de realizar ciertas actividades, como movimientos, reproducir sonidos, y algunas otras que se irán planificando a medida que se avance en el proyecto.

## Nombre del Personaje
Posadero

## Materiales utilizados
| Nombre | Descripción | Cantidad | Precio |
|--|--|--|--|
| ESP32 | MicroControlador con acceso WiFi y Bluetooth | 1 | $140.00 |
| Cables Dupont | Cables para conectar circuito | 50 | $60.00 |
| ServoMotor | Encargado de controlar el brazo del posadero como seña dando el paso a su hogar | 1 | $100.00 |
| Leds RGB | Cumplirán la función de un foco que será a su vez un indicador de entrada | 5 | $10.00 |
| Sensor ultrasonico HC - sr04 | Ayudará a saber si es que el posadero tiene un objeto cerca y así hacer un gesto para permitir la entrada | 1 | $70.00 |
| Buzzer Ard - 356 | Pequeña bocinita que reproducirá sonidos dependiendo de la situación en la que se encuentre el posadero | 1 | $50.00 | 

## Software utilizado
| Nombre | Versión | Tipo |
|--|--|--|
| Thonny | 4.1.2 | Software Libre |
| MySQL | 8.0 | Software Libre |
| Servidor | 20.04.4 | Software Libre |

## Prototipo en dibujo
![Imagen de WhatsApp 2023-11-08 a las 23 00 06_4c973103](https://github.com/Oscaarc/PersonajeIoT/assets/146128902/76be5ce5-2d68-4db4-8316-8f9d492ea95b)

## Comunicación
El usuario podrá interactuar con el personaje con ayuda de un sensor, que al recibir señal de tener algo en frente, enviará un mensaje de bienvenida y hara un gesto para dar paso.

## Arquitectura
![image](https://github.com/Oscaarc/PersonajeIoT/assets/146128902/f494c5b0-2307-4b9d-b6d6-5888bb59b1f9)

## Base de datos
Create DataBase Posadero
Go

Use Posadero

Create Table RegistroPos
(
	id int not null Constraint pk_id Primary Key,
	fecha date not null
)


