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


CODIGO EN MICROPHYTON;

//CODIGO CON EL TOPICO:


from machine import Pin, PWM
from time import sleep
from hcsr04 import HCSR04
import utime
import network
from umqtt.simple import MQTTClient

MQTT_BROKER = "192.168.1.100"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_CLIENT_ID = ""
MQTT_TOPIC = "oem/componente"
MQTT_PORT = 1883


# Configuración de pines y componentes
TRIGGER_PIN = Pin(13, Pin.OUT)
ECHO_PIN = Pin(35, Pin.IN)
BUZZER_PIN = Pin(25)
SERVO_PIN = Pin(12)
LED_PIN = Pin(27)
STATUS_LED_PIN = Pin(4, Pin.OUT)

# Configuración de componentes
sensor = HCSR04(TRIGGER_PIN, ECHO_PIN, echo_timeout_us=2400)
buzzer = PWM(BUZZER_PIN, freq=440, duty=512)
servo1 = PWM(SERVO_PIN, freq=50)
led1 = Pin(LED_PIN, Pin.OUT)


def wifi_connect():
    print("Conectando...", end="")
    sta_if = network.WLAN(network.STA_IF)
    sta_if.active(True)
    sta_if.connect("linksys ", "")
    while not sta_if.isconnected():
        print(".", end="")
        utime.sleep(0.3)
    print("WiFi conectada")
    
def llegada_mensaje(topic, msg):
    print("Mensaje recibido en el tópico %s: %s" % (topic, msg))
    if msg == b'jingle_bells':
        print("Reproduciendo Jingle Bells...")
        play_melody(jingle_bells)
    elif msg == b'deck_the_halls_us':
        print("Reproduciendo Deck the Halls (US)...")
        play_melody(deck_the_halls_us)
    else:
        print("Melodía no reconocida:", msg)
        
def suscribir():
    client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, port=MQTT_PORT,
                        user=MQTT_USER, password=MQTT_PASSWORD, keepalive=0)
    client.set_callback(llegada_mensaje)
    client.connect()
    client.subscribe(MQTT_TOPIC)
    print("Conectando en %s, al topico %s" % (MQTT_BROKER, MQTT_TOPIC))
    return client

# Diccionario de frecuencias para las notas
NOTAS_FRECUENCIAS = {
    'C': 261,
    'D': 294,
    'E': 329,
    'F': 349,
    'G': 392,
    'A': 440,
    'B': 493,
    'R': 0
}

def tono(nota, tiempo):
    # Obtener la frecuencia correspondiente a la nota del diccionario
    frecuencia = NOTAS_FRECUENCIAS.get(nota, 0)
    buzzer.freq(int(frecuencia))
    buzzer.duty(512)
    sleep(tiempo)
    # Detener el tono
    buzzer.duty(0)


def play_christmas_melody():
    melody = [
    ('E', 0.125), ('E', 0.125), ('E', 0.25),
    ('E', 0.125), ('E', 0.125), ('E', 0.25),
    ('E', 0.25), ('G', 0.25), ('C', 0.5),
    ('D', 0.25), ('E', 0.25), ('F', 0.25), ('F', 0.25),
    ('F', 0.25), ('F', 0.25), ('F', 0.5),
    ('E', 0.25), ('E', 0.25), ('E', 0.25), ('E', 0.25),
    ('E', 0.25), ('D', 0.25), ('D', 0.25), ('E', 0.25), ('D', 0.25), ('G', 0.5),
    ('E', 0.125), ('E', 0.125), ('E', 0.25),
    ('E', 0.125), ('E', 0.125), ('E', 0.25),
    ('E', 0.25), ('G', 0.25), ('C', 0.5),
    ('D', 0.25), ('E', 0.25), ('F', 0.25), ('F', 0.25),
    ('F', 0.25), ('F', 0.25), ('F', 0.5),
    ('E', 0.25), ('E', 0.25), ('E', 0.25), ('E', 0.25),
    ('E', 0.25), ('D', 0.25), ('D', 0.25), ('E', 0.25), ('D', 0.25), ('G', 0.5),
    
    ]
    
    deck_the_halls_us = [
    ('F', 0.25), ('G', 0.25), ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25),
    ('F', 0.25), ('G', 0.25), ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25),
    ('C', 0.25), ('D', 0.25), ('E', 0.25), ('F', 0.25), ('G', 0.25), ('A', 0.25), ('G', 0.25), ('F', 0.25),
    ('F', 0.25), ('G', 0.25), ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25),
    ('G', 0.25), ('A', 0.25), ('B', 0.25), ('C', 0.25), ('D', 0.25), ('E', 0.25), ('F', 0.25), ('G', 0.25),
    ('A', 0.25), ('B', 0.25), ('C', 0.25), ('D', 0.25), ('E', 0.25), ('F', 0.25), ('G', 0.25), ('A', 0.25),
    ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25), ('F', 0.25), ('G', 0.25),
    ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25), ('C', 0.25), ('D', 0.25),
    ('E', 0.25), ('F', 0.25), ('G', 0.25), ('A', 0.25), ('G', 0.25), ('F', 0.25), ('F', 0.25), ('G', 0.25),
    ('A', 0.25), ('G', 0.25), ('F', 0.25), ('E', 0.25), ('D', 0.25), ('C', 0.25),
    ]
    

    for note in melody:
        tono(note[0], note[1])
wifi_connect()
client = suscribir()

try:
    while True:
        distancia = sensor.distance_cm()
        print("Distancia:", distancia, "centímetros")

        if distancia <= 200:
            while True:
                servo1.duty(45)
                sleep(0.9)
                servo1.duty(0)
        else:
            for i in range(60, 0, -2):
                servo1.duty(i)
                sleep(0.02)
            servo1.duty(0)
            led1.value(0)

        sleep(1)

except KeyboardInterrupt:
    # Detener el tono y apagar el servo y el LED cuando se interrumpe con Ctrl+C
    buzzer.duty(0)
    servo1.duty(0)
    led1.value(0)

# Apagar el LED de estado al finalizar el programa
STATUS_LED_PIN.value(0)


CODIGO EN C

//Librería para el Servo
#include <ESP32Servo.h>

Servo servo;
int servoPin = 19;

void setup() {
  Serial.begin(115200);
  //Configurar servo
  servo.attach(servoPin);   
}

//Función que se ejecuta repetidamente
void loop() {
  // Mueve el servo a 0 grados
  servo.write(0);
  delay(1000);

  // Mueve el servo a 90 grados
  servo.write(90);
  delay(1000);

  
}



// constants won't change
const int RELAY_PIN = 4;  // the Arduino pin, which connects to the IN pin of relay




int pinBuzzer = 23;

int C__ =  261/4;
int Cs__=  277/4;
int D__ =  293/4 ;
int Ds__=  311/4;
int E__ =  329/4 ;
int F__ =  349/4 ;
int Fs__=  369/4;
int G__ =  391/4 ;
int Gs__=  415/4;
int A__ =  440/4 ;
int As__=  466/4;
int B__ =  493/4;

int C_ =  261/2;
int Cs_=  277/2;
int D_ =  293/2 ;
int Ds_=  311/2;
int E_ =  329/2 ;
int F_ =  349/2 ;
int Fs_=  369/2;
int G_ =  391/2 ;
int Gs_=  415/2;
int A_ =  440/2 ;
int As_=  466/2;
int B_ =  493/2 ;


int Sil = 5;
int C =  261;
int Cs=  277;
int D =  293 ;
int Ds=  311;
int E =  329 ;
int F =  349 ;
int Fs=  369;
int G =  391 ;
int Gs=  415;
int A =  440 ;
int As=  466;
int B =  493 ;



int C2   =524;
int Cs2  =555;
int D2   =588;
int Ds2  =623;
int E2   =660;
int F2   =699;
int Fs2  =740;
int G2   =784;
int Gs2  =831;
int A_2   = 880;
int As2  =933;
int B2   =988;

int C3  =1047;
int Cs3  =555*2;
int D3   =588*2;
int Ds3  =623*2;
int E3   =660*2;
int F3   =699*2;
int Fs3  =740*2;
int G3   =784*2;
int Gs3  =831*2;
int A_3   = 880*2;
int As3  =933*2;
int B3   =988*2;



int tempo=88*2;
int negra=60000/tempo;
int semi = negra/4;

int fusa =semi/2;
int corch = 2*semi;
int np = corch*3;

int blanca = negra*2;
int redonda = blanca*2;
int rep = 3*negra;
int bnp = 3*negra+3*corch;

int retardo = 100;

void nota(int nota, int duracion){
  tone(pinBuzzer,nota, duracion);
  delay(duracion);
  noTone(pinBuzzer);
  delay(duracion);
}

void setup() {
  // initialize digital pin as an output.
  pinMode(RELAY_PIN, LOW);


  delay(500);

}

void loop() {



  // bemoles
  // A B E
  //
  // sostenidos
  // G A D
  //  // 1
  
 

prePre();
yalosves(); 
ymaullare();
 delay(1000);


}
//C D F G


void prePre(){
  
  // [1]
  nota(E,corch); 
  // [2]
  nota(B,blanca);
  
  nota(Sil,negra+corch);
 
  nota(E,corch); 
  // [3]
  nota(B,semi+corch);
  nota(A,semi+corch);
  nota(B,semi); 
  nota(A,semi);
  
  nota(B,semi+corch);
  nota(A,semi+corch);
  nota(E,semi); 
  nota(Cs,semi);
  // [4]
  nota(Gs,blanca);
  nota(Sil,negra+corch);
 
  nota(Cs,corch); 
  
  // [5]
  nota(Gs,semi+corch);
  nota(Fs,semi+corch);
  nota(Gs,semi); 
  nota(Fs,semi);
  
  nota(Gs,semi+corch);
  nota(Fs,semi+corch);
  nota(Cs,semi); 
  nota(A_,semi);

  // [6]
  nota(E,blanca);
  nota(Sil,negra+corch);
 
  nota(E,semi);
  // [7] ...
  nota(E,semi+corch);
  nota(D,corch);    
  
  nota(Cs,corch);    
  nota(D,semi);    
  nota(Cs,corch+semi);    
  nota(D,corch);  

  nota(Fs,corch);
  nota(A,semi);
  
  // 8..
  nota(Cs2,corch+semi);
  nota(B,corch);

  nota(Cs2,corch);    
  nota(B,semi);    
  nota(Cs2,corch+semi);    
  nota(B,corch);  
  nota(E,corch); 
  // 9
  nota(B,blanca);
  nota(Sil,blanca);

  nota(E,corch); 
  // [11]
  nota(B,semi+corch);
  nota(A,semi+corch);
  nota(B,semi); 
  nota(A,semi);
  
  nota(B,semi+corch);
  nota(A,semi+corch);
  nota(E,semi); 
  nota(Cs,semi);
  // [12]
  nota(Gs,blanca);
  nota(Sil,negra+corch);
 
  nota(Cs,corch); 
  
  // [13]
  nota(Gs,semi+corch);
  nota(Fs,semi+corch);
  nota(Gs,semi); 
  nota(Fs,semi);
  
  nota(Gs,semi+corch);
  nota(Fs,semi+corch);
  nota(Cs,semi); 
  nota(A_,semi);

  // [14]
  nota(E,blanca);
  nota(Sil,negra+corch);
 
  nota(E,semi);
  // [15] ...
  nota(E,semi+corch);
  nota(D,corch);    
  
  nota(Cs,corch);    
  nota(D,semi);    
  nota(Cs,corch+semi);    
  nota(D,corch);  

  nota(Fs,corch);
  nota(A,semi);
  
  // 16..
  nota(Cs2,corch+semi);
  nota(B,corch);

  nota(Cs2,corch);    
  nota(B,semi);    
  nota(Cs2,corch+semi);    
  nota(B,corch+semi);  

  nota(Sil,semi); 
  

};




void yalosves(){
  nota(Cs2,semi);
  nota(B,semi);
  
  // [16]
  nota(Cs2,negra+corch);
  
  
  nota(B,corch);
  nota(Fs,corch);
  nota(A,negra);
  
  nota(Cs2,semi); 
  nota(E2,semi);
  
  // 17
  nota(Cs2,blanca);
  nota(Sil,negra+corch);
  
  nota(B,semi);
  nota(A,semi);
  
  // [18]
  nota(B,negra+corch);
  
  
  nota(A,corch);
  nota(E,corch);
  nota(Gs,negra);
  
  nota(B,semi); 
  nota(E2,semi);

  // [19]
  nota(B,blanca);
  nota(Sil,negra+corch);
  nota(A,semi);
  nota(Gs,semi);
  
  
  // [20]
  nota(A,negra+corch);
  
  
  nota(Fs,corch);
  nota(Gs,corch);
  nota(A,negra);
  
  nota(Gs,semi); 
  nota(Fs,semi);

  // 21
  nota(A,corch+semi);
  nota(Gs,negra);
  nota(Sil,negra);
  nota(Sil,semi);
  
  
  nota(Gs,negra/3);
  nota(A,negra/3);
  nota(B,negra/3);

  // 22
  nota(B,corch+semi);
  nota(Gs,negra);
  nota(Sil,negra);
  nota(Sil,semi);
  
  
  nota(Gs,negra/3);
  nota(A,negra/3);
  nota(B,negra/3);


}

void ymaullare(){
  
  // 23
  nota(D2,corch+semi);
  nota(Cs2,negra);
  nota(Sil,blanca);
  nota(Sil,semi);

  // 24
  nota(Fs,corch);
  nota(Gs,corch);
  nota(A, corch);

  
  nota(Cs2,blanca);
  
  nota(B,corch);
  
  //25
  
  nota(A,blanca);
}



