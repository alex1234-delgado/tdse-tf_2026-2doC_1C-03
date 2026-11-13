# Juego de la viborita "SNAKE"

## Informe de Avances

**Autores:**

DELGADO Alex – Padrón 112250  
PAREDES Fabian – Padrón 113724  
LAIME Kevin – Padrón 111621

**Fecha:** 13/11/26

**2.º cuatrimestre 2026**

A continuación, se presenta el informe de avance del proyecto integrador **Snake**, desarrollado sobre la placa NUCLEO-F103RB con microcontrolador STM32F103RB.

Se detallan los requisitos establecidos en la especificación inicial, indicando el estado actual de implementación de cada uno de ellos.

## Estados

| Estado | Descripción |
|:---:|---|
| 🟢 | Implementado y probado. |
| 🟡 | En proceso de implementación. |
| 🔴 | No implementado |

## 1. Lógica del juego

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF01 | El sistema representará una serpiente y una fruta dentro de un área de juego de 8 × 8 posiciones. | 🟢 |
| RF03 | La serpiente avanzará automáticamente con una velocidad determinada por el nivel de dificultad. | 🟢 |
| RF04 | Cuando la serpiente alcance la fruta, aumentará su longitud y el puntaje se incrementará en una unidad. | 🟢 |
| RF05 | Luego de recolectar una fruta, el sistema generará una nueva posición válida para ella. | 🟢 |
| RF06 | El sistema detectará colisiones contra los límites del área de juego y contra el cuerpo de la serpiente. | 🟢 |
| RF07 | Al producirse una colisión, la partida finalizará. | 🟢 |
| RF08 | El sistema permitirá iniciar una nueva partida y reiniciar el puntaje. | 🟢 |

## 2. Control de la partida

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF02 | El usuario podrá controlar la dirección de movimiento mediante el joystick analógico. | 🟢 |
| RF11 | El sistema interpretará las lecturas del joystick como órdenes de movimiento en cuatro direcciones. | 🟡 |
| RF12 | El sistema rechazará cambios de dirección que produzcan un giro de 180 grados inmediato. | 🟡 |
| RF13 | Los pulsadores permitirán pausar, reanudar o reiniciar la partida. | 🟡 |

## 3. Interfaz visual

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF09 | La matriz LED 8 × 8 mostrará la posición de la serpiente y de la fruta. | 🟢 |
| RF10 | La pantalla LCD 16 × 2 mostrará el puntaje, la velocidad, las opciones del menú y los mensajes de estado. | 🟢 |
| RF22 | Los LEDs indicadores señalarán los estados principales del sistema: juego, configuración y fin de partida. | 🟢 |

## 4. Configuración

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF14 | El usuario podrá seleccionar el nivel de dificultad o la velocidad inicial desde el menú. | 🟡 |
| RF15 | El usuario podrá habilitar o deshabilitar los efectos de sonido. | 🔴 |

## 5. Almacenamiento persistente

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF16 | El sistema almacenará el récord máximo en memoria no volátil, mediante EEPROM externa o Flash interna. | 🟡 |
| RF17 | El sistema permitirá consultar el récord máximo almacenado. | 🟡 |
| RF18 | El sistema actualizará el récord cuando el puntaje de una partida supere el valor histórico. | 🟡 |

## 6. Interfaz sonora

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF19 | El buzzer emitirá señales sonoras para indicar eventos como recolección de fruta, cambio de nivel o fin de partida. | 🟢 |

## 7. Comunicación Bluetooth

| Req ID | Descripción | Estado |
|---|---|:---:|
| RF20 | El módulo Bluetooth permitirá transmitir el puntaje y el estado de la partida hacia un dispositivo externo. | 🟡 |
| RF21 | El sistema podrá recibir comandos de pausa o reinicio mediante Bluetooth. | 🔴 |

## 8. Arquitectura y firmware

| Req ID | Descripción | Estado |
|---|---|:---:|
| RNF01 | El firmware se desarrollará en lenguaje C para el STM32F103RB y no utilizará un RTOS. | 🟢 |
| RNF02 | El código se organizará mediante el patrón Escrutar–Procesar–Actuar. | 🟢 |
| RNF03 | La lógica de funcionamiento se implementará mediante una máquina de estados finitos. | 🟢 |
| RNF04 | El programa principal utilizará un Super-Loop coordinado mediante SysTick. | 🟢 |
| RNF05 | No se utilizarán retardos bloqueantes para temporizar el juego. | 🟢 |
| RNF06 | Las entradas de pulsadores contarán con antirrebote por software. | 🟢 |
| RNF07 | El sistema iniciará en un estado seguro, con las salidas en condiciones conocidas. | 🟢 |
| RNF08 | El montaje final se realizará sobre una placa rígida, con componentes y conexiones correctamente fijados. | 🟡 |
| RNF09 | El firmware mantendrá una estructura modular y nombres claros para variables, funciones y módulos. | 🟢 |
| RNF10 | La comunicación con los periféricos respetará los protocolos eléctricos y temporales correspondientes. | 🟡 |

## 9. Integración de hardware

| Componente | Función dentro del proyecto | Estado |
|---|---|:---:|
| NUCLEO-F103RB | Unidad principal de procesamiento del sistema. | 🟢 |
| Joystick analógico | Control de dirección mediante entradas ADC. | 🟢 |
| Matriz LED 8 × 8 | Representación visual del juego mediante MAX7219. | 🟢 |
| Pantalla LCD 16 × 2 | Visualización de puntaje, menús y mensajes. | 🟡 |
| Pulsadores | Control de inicio, pausa y reinicio. | 🟡 |
| LEDs indicadores | Señalización de estados de funcionamiento. | 🟡 |
| Buzzer | Generación de efectos sonoros mediante PWM. | 🟡 |
| Módulo HM-10 | Comunicación Bluetooth mediante UART. | 🟡 |
| Memoria EEPROM | Almacenamiento de configuraciones y récord. | 🟡 |

## 10. Observaciones generales

El desarrollo del proyecto se plantea de manera incremental, priorizando inicialmente la implementación de la mecánica principal del juego, el control mediante joystick y la representación gráfica en la matriz LED.

Una vez verificado el funcionamiento de estos módulos, se continuará con la integración de las funcionalidades adicionales, como la pantalla LCD, el menú de configuración, el almacenamiento persistente, los efectos sonoros y la comunicación Bluetooth.

Los estados indicados en las tablas reflejan el avance de implementación al momento de elaboración del presente informe y podrán actualizarse a medida que se realicen nuevas pruebas e integraciones.

