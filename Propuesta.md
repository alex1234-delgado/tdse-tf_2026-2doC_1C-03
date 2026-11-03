

 <img src="https://www.fi.uba.ar/images/logo-fiuba.png" alt="image2" width="50%">


# **El juego de la viborita SNAKE**

 

**Autores: DELGADO Alex; PAREDES Fabian; LAIME Kevin**

**Padrón: 112250; 1113724O; 111621**

**Fecha: 2do cuatrimestre 2026**

**Fecha de entrega estimada: 30/10/2026**

 

 

 

## 1\. Descripción General del Proyecto

El presente proyecto propone el diseño e implementación de un **Producto Mínimo Viable (MVP)** consistente en una consola recreativa embebida que ejecuta el clásico juego del gusanito (*Snake*). El sistema está construido alrededor de un microcontrolador de 32 bits de la familia **STM32 (ARM Cortex-M)** sobre una placa **NUCLEO-F103RB**.

El prototipo combina una **Matriz de LEDs 8x8 (controlada vía bus SPI)** para la renderización gráfica del área de juego y una **Pantalla LCD 16x2 (vía comunicación I2C o bus de 4 bits)** para la interfaz de usuario, visualización de puntuaciones en tiempo real, velocidad actual y menús de navegación.

El *firmware* se desarrollará en **C Bare Metal** (sin sistema operativo de tiempo real), siguiendo una arquitectura modular basada en el patrón **Escrutar – Procesar – Actuar**. La ejecución estará coordinada por un **Super-Loop** gobernado por un **Ejecutor Cíclico** sincronizado por la interrupción periódica del `SysTick` a **1 ms**, garantizando un comportamiento determinístico y no bloqueante en todas sus tareas.

El sistema contempla tres modos de operación principales:

1. **Modo `NORMAL`:** Ejecución dinámica del juego, captura de movimiento mediante Joystick analógico (ADC), gestión de colisiones, crecimiento del gusanito, actualización de puntaje y reproducción de efectos de sonido vía PWM en un buzzer.
2. **Modo `SET_UP`:** Menú interactivo desplegado en la pantalla LCD que permite ajustar parámetros como el nivel de dificultad (velocidad del juego), límites de sonido y borrado de récords. Los parámetros configurados se almacenan de forma no volátil en una **memoria EEPROM externa (I2C)** o Flash interna.
3. **Modo `FALLA` / `GAME_OVER`:** Gestión de eventos de colisión (contra bordes o cuerpo), activación de alarma sonora, almacenamiento no volátil de la puntuación máxima (*High Score*) y notificación vía **Bluetooth (módulo HM-10)** a una aplicación móvil.

---

## 2\. Especificación de Hardware

El hardware del sistema cumple estrictamente con los lineamientos de la cátedra, combinando los periféricos obligatorios del curso con componentes dedicados para la aplicación del juego.

### 2.1\. Placa Base y Ensamblado

* **Placa Base Soldada:** La totalidad del circuito se montará sobre un PCB diseñado a medida o sobre una placa experimental con componentes, borneras, conectores y cables rígidamente soldados (**queda totalmente excluido el uso de protoboards o cables Dupont sueltos**).

### 2.2\. Componentes de Hardware Obligatorios

* **Placa de Procesamiento:** STMicroelectronics NUCLEO-F103RB (MCU STM32F103RB, ARM Cortex-M3 a 72 MHz).
* **Entradas Analógicas:** Joystick analógico de 2 ejes (ejes X e Y leídos mediante canales del **ADC** de 12 bits) para el control direccional del gusanito.
* **Módulo de Comunicación Inalámbrica:** Transceptor Bluetooth **HM-10 (UART)** para el envío de métricas, estadísticas y comandos desde/hacia una App móvil.
* **Almacenamiento No Volátil:** Memoria **EEPROM externa (vía I2C)** o Flash interna del microcontrolador para guardar configuraciones del modo `SET_UP` y el récord histórico (*High Score*).
* **Salida de Audio:** **Buzzer piezoeléctrico** modulado por **PWM (Timer)** para la generación de tonos y efectos sonoros (comer fruta, cambio de dirección, game over).
* **Entradas Digitales Auxiliares:** *Dip switches* para la selección manual del modo de operación (`NORMAL` / `SET_UP`) y botones para pausa/reinicio.
* **Salidas Indicadoras:** LEDs de estado (Verde: Ejecución `NORMAL`, Amarillo: Modo `SET_UP`, Rojo: `GAME_OVER` / `FALLA`).

### 2.3\. Componentes Adicionales Específicos del Proyecto

* **Matriz de LEDs 8x8 con Controlador MAX7219:** Pantalla principal de renderizado del mapa de juego, gestionada mediante el bus de comunicación serie de alta velocidad **SPI**.
* **Pantalla LCD 16x2:** Interfaz de texto para el despliegue del menú `SET_UP`, puntaje actual, nivel de velocidad y mensajes de estado, conectada vía módulo adaptador **I2C (PCF8574)** o interfaz paralela de 4 bits.

---

## 3\. Arquitectura de Firmware y Software

El desarrollo del código seguirá el estándar de codificación ***Embedded C Coding Standard* de Michael Barr** y los principios de modularidad del taller.

### 3.1\. Patrón Escrutar – Procesar – Actuar

El programa se estructurará en tres capas independientes:

* **Escrutar (`task_sensor`):** Lee periódicamente los canales ADC del joystick, el estado de los botones, los dip switches y los datos recibidos vía Bluetooth HM-10\. Aplica algoritmos de filtrado y *debouncing* (eliminación de rebotes) antes de generar eventos.
* **Procesar (`task_system`):** Contiene la lógica central del juego modelada mediante **Máquinas de Estado Finito (Statecharts)**. Procesa las transiciones de movimiento, lógica de crecimiento, generación aleatoria de comida, detección de colisiones y gestión de menús.
* **Actuar (`task_actuator`):** Recibe comandos y actualiza el buffer de la matriz de LEDs (SPI), el contenido del LCD (I2C), las frecuencias del PWM en el buzzer y el estado de los LEDs indicadoras.

### 3.2\. Ejecutor Cíclico y Comportamiento No Bloqueante

* **Sincronización:** Un bucle principal (*Super-Loop*) es sincronizado cada **1 ms** por la interrupción del temporizador del sistema `SysTick`.
* **Cero Bloqueo:** Queda estrictamente prohibido el uso de funciones de retardo pasivo (`HAL_Delay` o bucles `while` de espera). Toda temporización se gestiona mediante contadores de *ticks* dentro de los estados.
* **Gestión de Energía:** En períodos de inactividad o pausa prolongada, el sistema invoca rutinas de bajo consumo (`Sleep` / `Stop Mode`) del STM32.

---

## 4\. Tabla de Requisitos del Sistema

### 4.1\. Requisitos Funcionales (RF)

| ID       | Tipo      | Descripción                                                                                                                                |
| -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **RF01** | Funcional | El sistema renderizará la posición del gusanito y la fruta en la Matriz de LEDs 8x8 mediante el bus **SPI**.                               |
| **RF02** | Funcional | El usuario controlará la dirección del gusanito mediante las lecturas analógicas del Joystick leídas a través del **ADC**.                 |
| **RF03** | Funcional | La pantalla **LCD 16x2** mostrará continuamente la puntuación actual, el nivel de dificultad y las opciones del menú.                      |
| **RF04** | Funcional | En el modo `SET_UP`, el usuario podrá ajustar la velocidad inicial y guardar el récord máximo (*High Score*) en la **EEPROM (I2C)**.       |
| **RF05** | Funcional | El sistema transmitirá las estadísticas y recibirá comandos de pausa o reinicio vía el módulo **Bluetooth HM-10 (UART)**.                  |
| **RF06** | Funcional | Se generarán tonos audibles mediante un **Buzzer (PWM)** al comer frutas, cambiar de nivel o al colisionar.                                |
| **RF07** | Funcional | La conmutación de los *dip switches* alternará de forma inmediata entre el modo de juego (`NORMAL`) y el menú de configuración (`SET_UP`). |
| **RF08** | Funcional | Al ocurrir una colisión, el sistema pasará al modo `GAME_OVER`, encenderá el LED rojo y actualizará la EEPROM si se superó el récord.      |

### 4.2\. Requisitos No Funcionales (RNF)

| ID        | Tipo         | Descripción                                                                                                                               |
| --------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **RNF01** | No Funcional | El firmware debe ser desarrollado en **C Bare Metal** sin uso de RTOS, aplicando la arquitectura **Escrutar–Procesar–Actuar**.            |
| **RNF02** | No Funcional | El bucle principal estará sincronizado por un **Ejecutor Cíclico a 1 ms**, con un tiempo de ejecución por ciclo menor a 1 ms.             |
| **RNF03** | No Funcional | La totalidad de los componentes deberá estar montada y soldada sobre una **placa base rígida**, sin uso de protoboards ni cables sueltos. |
| **RNF04** | No Funcional | La lógica de control debe estar implementada con **Máquinas de Estado Finito (Statecharts)** no bloqueantes.                              |
| **RNF05** | No Funcional | El código debe respetar las normas de estilo de codificación de **Michael Barr**.                                                         |
| **RNF06** | No Funcional | El microcontrolador debe implementar modos de **bajo consumo** durante los estados de reposo o pausa.                                     |

---

## 5\. Casos de Uso

### 5.1\. Caso de Uso 01: Ejecución de Partida de Juego (Modo `NORMAL`)

* **Actor Principal:** Usuario / Jugador.
* **Precondición:** El sistema se encuentra encendido en modo `NORMAL` y la partida ha iniciado.
* **Flujo Principal:**  
  1. El usuario desplaza el Joystick en una dirección (Arriba, Abajo, Izquierda, Derecha).
  2. La tarea `task_sensor` convierte la señal analógica del ADC, filtra el ruido y genera el evento de dirección correspondiente.
  3. La tarea `task_system` actualiza la posición de la cabeza del gusanito en la cuadrícula 8x8.
  4. Si la cabeza coincide con la coordenada de la fruta, el gusanito incrementa su longitud, aumenta el puntaje en +1, genera una nueva fruta en coordenadas aleatorias no ocupadas y emite un evento de sonido.
  5. La tarea `task_actuator` refresca la Matriz de LEDs vía SPI, actualiza el valor del puntaje en la pantalla LCD vía I2C y dispara un tono PWM en el buzzer.
* **Flujo Alternativo (Pausa):**  
  * Si el usuario presiona el botón de pausa o envía el comando vía Bluetooth, el juego congela el estado del gusanito y coloca el MCU en modo de bajo consumo relativo hasta recibir la reactivación.

### 5.2\. Caso de Uso 02: Configuración del Sistema y Gestión de Récord (Modo `SET_UP`)

* **Actor Principal:** Usuario.
* **Precondición:** El usuario conmuta el *dip switch* asignado al modo `SET_UP`.
* **Flujo Principal:**  
  1. El sistema pausa el juego y despliega en la pantalla LCD el menú principal de configuración.
  2. El usuario navega por las opciones utilizando el Joystick o los botones auxiliares:  
    * *Opción 1:* Ajuste de velocidad inicial (Nivel 1 al 5).
    * *Opción 2:* Habilitar / Deshabilitar efectos de sonido.
    * *Opción 3:* Reiniciar récord histórico de puntuación.
  3. Al seleccionar y confirmar un parámetro, la tarea `task_system` envía los datos formateados a la tarea `task_actuator`.
  4. Se ejecutan las operaciones de escritura serie I2C para almacenar la nueva configuración en la memoria EEPROM no volátil.
  5. El LCD muestra el mensaje de confirmación "Configuracion Guardada".

### 5.3\. Caso de Uso 03: Colisión, Game Over y Notificación Bluetooth

* **Actor Principal:** Sistema / Usuario.
* **Precondición:** El gusanito en movimiento colisiona contra los bordes de la matriz 8x8 o contra su propio cuerpo.
* **Flujo Principal:**  
  1. La máquina de estados en `task_system` detecta la superposición de coordenadas o fuera de límite.
  2. Transiciona inmediatamente al estado `GAME_OVER`.
  3. Desconecta el procesamiento del Joystick y evalúa si la puntuación obtenida supera la marca almacenada en la EEPROM.
  4. Si el puntaje es un nuevo récord, escribe la nueva puntuación en la EEPROM y envía una trama con el mensaje `"NUEVO RECORD: XX"` vía UART al módulo Bluetooth HM-10.
  5. La tarea `task_actuator` apaga el LED verde de ejecución, enciende el LED rojo de falla/game over, ejecuta una secuencia de tonos sonoros de derrota en el buzzer y hace parpadear el mensaje "GAME OVER" en la pantalla LCD.
