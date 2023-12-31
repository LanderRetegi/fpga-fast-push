# fpga-fast-push

# CONTEXTUALIZACIÓN FPGA

Una FPGA es un conjunto de múltiples circuitos dispuestos matricialmente, cuyas interconexiones son programables por el usuario para la aplicación requerida. En una FPGA se programa su hardware, a diferencia de los microcontroladores en los que solo existe un hardware fijo y se programa su software.

Tienen un amplio espectro de aplicaciones: como industria automotriz, reconocimiento de voz, controladores de dispositivos, sistemas militares, radiotransmisores/receptores gestionados por software, computación de alto rendimiento, sistemas de emulación de hardware, equipos de investigación en medicina....  Además, tienen un papel notable en el desarrollo de sistemas integrados debido a su capacidad para iniciar el desarrollo de software del sistema simultáneamente con el hardware.

![descarga](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/69469c70-018b-4f5e-814d-8ff38566e210)


# EL RETO

En nuestro caso hemos realizado un pequeño juego gracias al FPGA y sus posibilidades gracias a las puertas lógicas. El juego lo hemos llamado fast push, y consiste en una competición entre dos jugadores para ver quien es capaz de pulsar más rápido el botón de acción hasta llegar a una cifra previamente seleccionada. Antes de que el juego comience los jugadores podrán elegir del 0 al 99 la cantidad de pulsaciones necesarias para ganar y una vez elegido intentarán llegar a dicho número lo más rápido posible. 


# DESARROLLO

Para crear este proyecto hemos tenido que dividirlo en varias partes. Primero realizamos un esquema para plasmar la lógica que iba a seguir nuestro juego y a partir de ahí empleamos nuestro tiempo en diferentes programas para empezar a hacer el mismo. Esos programas son principalmente Multisim (para realizar el circuito) e Icestudio para poder transferir el circuito a nuestra FPGA. Por último montamos dicho circuito físicamente en una protoboard ya con todo preparado para jugar cuando se desee.

![GGGGGG](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/f4ac8c35-e15e-4e3b-905b-0dd331002748) ![806b2700-cc2e-11eb-92f6-f49697fc75e3](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/8ea3bdf8-2551-44cc-a4cc-3df0365ea5c9)


# BOM (lista de componentes) 

-FPGA Alhambra II

-Display SPI de 4 dígitos de 7 segmentos

-3 pulsadores

-3 resistencias de 10k Ω

-4 resistencias de 1k Ω

-Cables hembra-macho

-1 selector

-2 LEDs

-Protoboard


# PASOS SEGUIDOS

Para realizar el juego hemos creado el circuito en Multisim. En dicho circuito hemos implementado los componentes virtuales necesarios que son: 3 pulsadores, 2 interruptores de 4, 4 displays de 7 segmentos, 4 contadores (2 para cada jugador), 2 decodificadores y algunas puertas lógicas( 3 AND y 2 OR).

El primer paso es colocar los dos pulsadores pull down, uno para cada jugador. Para evitar los rebotes usamos condensadores y disparadores Schmitt. Además de estos botones utilizamos otro como iniciador/reset con su respectiva resistencia. El selector de pulsaciones se realiza con los 2 interruptores de 4.

El siguiente paso es implementar los contadores. Cada uno tiene 4 entradas y 4 salidas, los 2 interruptores de 4 se conectan a las entradas de los 4 contadores. El primer interruptor al primer contador de cada jugador y el segundo al segundo contador de cada uno. El interruptor de inicio/reset se une a cada “load” de cada contador para poder resetear o cargar el número que queramos en cualquier momento. 

El “up” de cada contador se une al vcc de 5V mientras que el “down” de los contadores del segundo dígito van al botón de los jugadores. El botón del primer jugador se conecta al contador que maneja el display de la segunda cifra y el segundo botón igual pero con sus respectivos componentes. Por último el down de los contadores que manejan la primera cifra van unidas a la salida BO de los contadores anteriormente mencionados (así logramos que la primera cifra solo baje cuando el segundo dígito llegue a 0).

Las 4 salidas de los contadores se conectan a los decodificadores que a su vez van a los displays. Los decodificadores tienen otras 3 entradas llamadas “LT” “RBI” y “BI”. Dos se unen a los 5V y uno que es el “RBI”a tierra.

Para finalizar el circuito necesitamos las puertas lógicas. Colocamos 2 OR de 8 entradas (las 4 salidas de cada contador) y se unen mediante un AND que se dirige al “down” de cada contador que controla la segunda cifra del display.

Este sería el circuito mencionado al completo:
![jjjjjjjjjjjjjjjjjjjjjjjjjj](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/b9063a0c-a1f9-4e07-99a6-4e4ef95e5e53)

# ICESTUDIO

Para realizar el programa en ICEstudio hemos utilizado 4 contadores, la pantalla SPI, anti rebotes, detectores de flancos para los pulsadores y las salidas y entradas mencionadas anteriormente. 

El juego inicia después de que con el selector indiquemos el número máximo al que deseamos llegar en el juego. Los 2 jugadores pulsaran sus botones lo más rápido que puedan hasta llegar al número seleccionado. Cuando el jugador mas rapido llegue al número, se bloquearan los contadores y se quedara marcado el numero en el que están. Cuando finalice se encenderá una led verde al lado del ganador.

Así quedaría el programa:
![Pulsar rapido](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/2d050330-d5a4-4662-b007-314997af7ddb)

# MONTAJE

Por último completamos el montaje físico del juego en una protoboard gracias a los pines de la FPGA.

![IMG20231124093020](https://github.com/LanderRetegi/fpga-fast-push/assets/151746072/3f6e885d-0b51-4881-8bc8-00af721f329a)

# MEJORAS

Para mejorar el juego se pueden implementar varias ideas como por ejemplo introducir un buzzer que suene cuando un jugador gane. U otro selector para poder elegir los números del 0 al 99, ya que al tener pines limitados solo podemos seleccionar de 10 en 10 los numeros mediante un único selector.  También se podria añadir un display para mostrar la mejor puntuación hasta el momento o cualquier ocurrencia futura que pueda aportar al juego .












 




