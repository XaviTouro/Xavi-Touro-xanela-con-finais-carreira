# Xavi-Touro-xanela-con-finais-carreira
/************************************************************************+************************************
*
*  Na seguinte práctica vamos imitar o funcionamento dunha xanela de coche, a cal se
*  activa mediante un único pulsador de activación no pin 13 para activar dous motores de corrente continua (nos pins 7 e 8)
*  (os motores non se activarán ao mesmo tempo,xa que en cada pulso do pulsador activamos o motor A ou B
*   e si damos outro pulso en calquer momento despois da activación forzamos unha parada).
*  Aparte añadiranse dous pulsadores máis simulando finais de carreira pra cada motor.
*  No momento que premas o botón de final carreira de cada motor, este cesa o seu traballo.
*  os pulsadores levan as suas respectivas resistencias para protexer o arduíno.
*
*  Autor: Javier Figueiro Resúa
*  Data: Marzo 2024
*
*************************************************************************************************************/


//Definir pins de entrada e saída
const int pinEntrada = 13;
const int pinSalida1 = 8;
const int pinSalida2 = 7;
const int pinFinal1 = 11;
const int pinFinal2 = 12;

//Variabel para almacenar o estado da entrada
int estadoEntradaAnterior = LOW;
//Variabel para controlar que a saída está activa
int salidaActiva = 0;
int estadoEntrada = 0;
int direccion = 0; // 0 abajo 1 arriba
int valorPinFinal1 = 0;
int valorPinFinal2 = 0;

void setup(){
  //Iniciar os pins
  pinMode(pinEntrada, INPUT);
  pinMode(pinFinal1, INPUT);
  pinMode(pinFinal2, INPUT);
  
  pinMode(pinSalida1, OUTPUT);
  pinMode(pinSalida2, OUTPUT);
}

void loop() {
  //Ler o estado actual da entrada
  // int estadoEntrada = digitalRead(pinEntrada);
  valorPinFinal1 = digitalRead(pinFinal1);
  valorPinFinal2 = digitalRead(pinFinal2);
	
  while(digitalRead(pinEntrada)) {
  	estadoEntrada = 1;
  }
  
  if (estadoEntrada) { // Si se ha detectado una pulsación
    estadoEntrada = 0; // Se resetea el valor del pulsador
    if (salidaActiva) { // Revisa si se está moviendo la ventanaa
      	direccion = !direccion;	// Se cambia la dirección
      
    	digitalWrite(pinSalida1, LOW); // Se apagan los dos motores
      	digitalWrite(pinSalida2, LOW);
      	
      	salidaActiva = 0; // Se indica que se dejo de mover la ventana
    } else {
    	salidaActiva = 1; // Se indica que se mueve la ventana
   		
    	if (direccion) { // Si dirrecion es 1 (arriba)
    		digitalWrite(pinSalida1, HIGH); // Se enciende el motor
   		} else { // Si no (abajo)
    		digitalWrite(pinSalida2, HIGH); // Se enciende el otro motor
    	}
   	  }
  }
  
  if (valorPinFinal1) { // Si se detecta una pulsación del final de carrera de arriba
    direccion = 0; // Se cambia la dirección
    salidaActiva = 0; // Se indica que se deja de mover la ventana
  	digitalWrite(pinSalida1, LOW); // Se apaga el motor
  }
  
  if (valorPinFinal2) { // Si se detecta una pulsación del final de carrera de abajo
    direccion = 1; // Se cambia la dirección
    salidaActiva = 0; // Se indica que se dejó de mover la ventana
  	digitalWrite(pinSalida2, LOW); // Se apaga el motor
  }
  
  delay (10);
  //actualizamos estado anterior da entrada
  estadoEntradaAnterior = estadoEntrada;
}
  
