

 <img src="https://www.fi.uba.ar/images/logo-fiuba.png" alt="image2" width="50%">

# **Juego de la viborita "SNAKE"**

**Autores:** DELGADO Alex; PAREDES Fabian; LAIME Kevin\
**Padrones:** 112250; 113724; 111621\
**Fecha:** 2.º cuatrimestre 2026\
**Fecha de entrega estimada:** 30/10/2026

------------------------------------------------------------------------

# **1. Selección del proyecto a implementar**

## **1.1 Objetivo del proyecto y resultados esperados**

El objetivo del proyecto es diseñar e implementar un videojuego embebido
basado en el clásico juego *Snake* o "la viborita", utilizando como
plataforma principal una placa **NUCLEO-F103RB**, basada en el
microcontrolador **STM32F103RB (ARM Cortex-M3)**.

El jugador deberá controlar el movimiento de una serpiente dentro de un
área de juego de 8 × 8 posiciones, recolectando frutas para incrementar
su longitud y su puntaje. La partida finalizará cuando la serpiente
choque contra los límites del área de juego o contra su propio cuerpo.

La representación gráfica se realizará mediante una **matriz de LEDs 8 ×
8**, controlada por un integrado MAX7219 a través del bus SPI. Se
incorporará una **pantalla LCD 16 × 2** para mostrar información
complementaria, como el puntaje, la velocidad, los menús y los mensajes
de estado. El control del jugador se realizará mediante un joystick
analógico conectado a entradas ADC.

Además de la mecánica principal, se plantean las siguientes
funcionalidades:

-   **Menú de configuración:** permitirá seleccionar el nivel de
    dificultad, ajustar la velocidad inicial, habilitar o deshabilitar
    los efectos de sonido y consultar o reiniciar el récord histórico.
-   **Almacenamiento persistente:** se guardarán las configuraciones y
    el puntaje máximo en una memoria no volátil. Se contempla utilizar
    una EEPROM externa por I2C o, como alternativa, la memoria Flash
    interna del STM32.
-   **Interfaz sonora:** un buzzer controlado mediante PWM generará
    sonidos para indicar eventos del juego, como la recolección de una
    fruta, el cambio de nivel y el fin de la partida.
-   **Comunicación Bluetooth:** mediante un módulo HM-10 conectado por
    UART, se podrán transmitir estadísticas de la partida y recibir
    comandos básicos, como pausar o reiniciar el juego.
-   **Señalización de estados:** se utilizarán LEDs indicadores para
    distinguir los estados de ejecución normal, configuración y
    finalización de la partida.

El firmware se desarrollará en lenguaje C, sin utilizar un sistema
operativo de tiempo real (RTOS). La arquitectura estará organizada según
el patrón **Escrutar--Procesar--Actuar**, con una máquina de estados
finitos para administrar los modos de funcionamiento. El programa
principal utilizará un *Super-Loop* sincronizado mediante interrupciones
periódicas de `SysTick`, evitando retardos bloqueantes.

Como resultado se espera obtener un prototipo funcional, estable y
ampliable, que integre periféricos del STM32 y permita demostrar
conocimientos de entradas y salidas digitales, conversión
analógico-digital, temporizadores, PWM, interrupciones y buses de
comunicación.

## **1.2 Proyectos similares y alternativas consideradas**

Para seleccionar el alcance de implementación se consideran tres
alternativas posibles:

1.  **Snake avanzado con STM32:** incluye matriz LED 8 × 8, pantalla
    LCD, joystick analógico, buzzer, memoria no volátil, indicadores y
    comunicación Bluetooth.
2.  **Snake básico con STM32:** conserva la placa STM32, el joystick y
    la matriz LED, pero no incorpora pantalla LCD, Bluetooth ni menú
    avanzado. La configuración y el puntaje se muestran mediante
    indicadores simples.
3.  **Snake básico con Arduino UNO:** implementa la mecánica principal
    del juego utilizando una plataforma Arduino, una matriz LED y
    controles sencillos, con una arquitectura de firmware más simple.

Para comparar las alternativas se consideran los siguientes criterios:

1.  **Disponibilidad del hardware:** facilidad para conseguir la placa y
    los componentes necesarios en el mercado local.
2.  **Experiencia de juego:** calidad de la interacción, claridad de la
    información presentada y variedad de funcionalidades.
3.  **Escalabilidad:** posibilidad de incorporar nuevos modos,
    estadísticas, comunicación y funcionalidades.
4.  **Tiempo de implementación:** esfuerzo requerido para diseñar,
    programar, integrar y probar el prototipo dentro del cuatrimestre.
5.  **Costo:** costo estimado de la placa, periféricos, componentes y
    montaje.
6.  **Interés académico y personal:** oportunidad de aplicar los
    contenidos de la materia y profundizar el uso de los periféricos del
    STM32.

Se asignan los siguientes pesos, en una escala de 1 a 10:

-   Disponibilidad del hardware: **10**.
-   Experiencia de juego: **8**.
-   Escalabilidad: **6**.
-   Tiempo de implementación: **8**.
-   Costo: **5**.
-   Interés académico y personal: **8**.

La disponibilidad del hardware recibe el mayor peso porque la
implementación depende de conseguir los componentes dentro de los plazos
de cursada. La experiencia de juego y el tiempo de implementación
también tienen una ponderación elevada: se busca un producto atractivo,
pero realizable y verificable durante el cuatrimestre. La escalabilidad
y el interés académico permiten valorar el aprovechamiento de los
periféricos del microcontrolador. El costo se considera importante,
aunque no es el único factor de decisión.

La tabla 1.2.1 presenta una evaluación preliminar. Los puntajes van
de 1 a 10 y el puntaje ponderado se calcula multiplicando el puntaje
asignado por el peso del criterio.

| Criterio | Peso | Snake avanzado STM32: puntaje | Ponderado | Snake básico STM32: puntaje | Ponderado | Snake básico Arduino: puntaje | Ponderado |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Disponibilidad del hardware | 10 | 8 | 80 | 9 | 90 | 10 | 100 |
| Experiencia de juego | 8 | 10 | 80 | 7 | 56 | 6 | 48 |
| Escalabilidad | 6 | 10 | 60 | 7 | 42 | 5 | 30 |
| Tiempo de implementación | 8 | 6 | 48 | 8 | 64 | 9 | 72 |
| Costo | 5 | 6 | 30 | 8 | 40 | 9 | 45 |
| Interés académico y personal | 8 | 10 | 80 | 8 | 64 | 6 | 48 |
| **Puntaje total** | — | — | **378** | — | **356** | — | **343** |

*Tabla 1.2.1: Comparación ponderada de las alternativas.*

Los valores de la tabla 1.2.1 son estimaciones iniciales del equipo y podrán
revisarse en función de la disponibilidad real de los componentes y del
avance de la implementación.

## **1.3 Selección del proyecto**

A partir de la comparación anterior, se selecciona el **Snake avanzado
con STM32**, debido a que ofrece la mejor combinación entre experiencia
de juego, posibilidades de ampliación y aprovechamiento de los
periféricos estudiados en la materia.

La alternativa básica con STM32 permitiría implementar la mecánica
principal en menos tiempo y con menor costo, pero limitaría la
interacción con el usuario y dejaría sin aprovechar periféricos como
UART, I2C y PWM. Por su parte, la alternativa con Arduino UNO presenta
una mayor simplicidad y disponibilidad, aunque no permite trabajar con
la misma profundidad sobre la arquitectura y los periféricos específicos
del STM32.

La propuesta seleccionada permite integrar:

-   Lectura analógica del joystick mediante ADC.
-   Comunicación SPI con la matriz de LEDs.
-   Comunicación I2C con la pantalla LCD y, si se utiliza, con la EEPROM
    externa.
-   Comunicación UART con el módulo Bluetooth HM-10.
-   Generación de señales sonoras mediante PWM.
-   Gestión temporal mediante `SysTick` y temporizadores.
-   Diseño de firmware modular con máquinas de estados y ejecución no
    bloqueante.

Para mantener el proyecto dentro del tiempo disponible, se establece
como prioridad la implementación de la mecánica principal del juego, el
control mediante joystick, la visualización en la matriz y la
señalización de estados. El menú, la persistencia, el sonido y Bluetooth
se integrarán como funcionalidades previstas en el alcance, priorizando
primero una versión estable del juego.

### **1.3.1 Hardware obligatorio y hardware adicional**

A los fines de organizar el desarrollo, se distingue entre el hardware
necesario para cumplir con la funcionalidad principal y los componentes
que amplían la experiencia del producto.

**Hardware obligatorio para la propuesta:**

-   **Placa de desarrollo:** NUCLEO-F103RB con microcontrolador
    STM32F103RB.
-   **Joystick analógico de dos ejes:** conectado a entradas ADC para
    seleccionar la dirección de movimiento.
-   **Matriz de LEDs 8 × 8 con controlador MAX7219:** permite
    representar el área de juego, la serpiente y la fruta. Se comunica
    mediante SPI.
-   **Placa base rígida:** el montaje final se realizará sobre PCB o
    placa experimental con componentes, conectores y cables
    correctamente fijados y soldados. No se utilizarán protoboards ni
    cables Dupont sueltos en la presentación final.
-   **Pulsadores o entradas digitales auxiliares:** para pausa, reinicio
    y confirmación de opciones, según la distribución definitiva de
    controles.
-   **LEDs indicadores:** para señalizar los estados principales del
    sistema.

**Hardware adicional o de ampliación:**

-   **Pantalla LCD 16 × 2:** mostrará puntaje, velocidad, menú de
    configuración y mensajes de estado. Podrá conectarse mediante un
    adaptador I2C (por ejemplo, PCF8574) o mediante interfaz paralela de
    4 bits.
-   **Buzzer piezoeléctrico:** generará efectos sonoros mediante PWM.
-   **Módulo Bluetooth HM-10:** permitirá transmitir estadísticas y
    recibir comandos mediante UART.
-   **Memoria EEPROM externa I2C:** almacenará configuraciones y
    récords. Si no se dispone de este componente, podrá evaluarse el uso
    de la Flash interna del STM32, considerando sus restricciones de
    escritura y borrado.
-   **Dip switches:** permitirán seleccionar manualmente el modo de
    funcionamiento, si se mantiene esta alternativa de interfaz.

La clasificación anterior define el alcance de implementación y no
modifica los requisitos de la cátedra que deban cumplirse con
periféricos específicos. La asignación definitiva de pines y buses se
documentará durante la integración del hardware.

### **1.3.2 Programación obligatoria y programación adicional**

**Programación obligatoria:**

-   Desarrollo del firmware en lenguaje C para el STM32F103RB, sin RTOS.
-   Inicialización y configuración de GPIO, ADC, SPI y temporizadores
    necesarios para el funcionamiento principal.
-   Lectura periódica del joystick y conversión de sus valores
    analógicos en eventos direccionales.
-   Implementación de la lógica del juego: movimiento, crecimiento de la
    serpiente, generación de fruta, puntaje y detección de colisiones.
-   Actualización de la matriz LED para representar el estado del juego.
-   Organización del programa mediante el patrón
    Escrutar--Procesar--Actuar.
-   Implementación de una máquina de estados finitos para administrar
    los modos de juego.
-   Uso de interrupciones y temporización no bloqueante, evitando
    retardos pasivos.

**Programación adicional:**

-   Control de la pantalla LCD y desarrollo del menú de configuración.
-   Gestión de la EEPROM o Flash para guardar el récord y los parámetros
    de configuración.
-   Generación de efectos sonoros mediante PWM.
-   Comunicación Bluetooth mediante UART, incluyendo un protocolo
    sencillo para estadísticas y comandos.
-   Ajuste de velocidad y dificultad desde el menú.
-   Rutinas de bajo consumo durante pausas o períodos de inactividad, si
    el tiempo de implementación lo permite.

### **1.3.3 Diagrama en bloques**

El diagrama en bloques deberá representar al STM32 como unidad central
de procesamiento y mostrar las conexiones con los periféricos: joystick
mediante ADC; matriz LED mediante SPI; LCD y EEPROM mediante I2C, si se
utilizan; Bluetooth mediante UART; buzzer mediante PWM; y pulsadores,
dip switches y LEDs mediante GPIO.

![Diagrama en bloques general del sistema Snake](Arquitectura_del_sistema_Snake_con_STM32.png)

*Figura 1.3.1: Diagrama en bloques general del sistema Snake.*

------------------------------------------------------------------------

# **2. Elicitación de requisitos y casos de uso**

El proyecto se plantea como un dispositivo recreativo embebido que
combina una interfaz física de control con una representación visual
sencilla. A diferencia de una implementación del juego en una
computadora o teléfono, el prototipo utiliza componentes electrónicos
reales y permite interactuar directamente con los periféricos del
microcontrolador.

Existen numerosas implementaciones de *Snake* en computadoras, teléfonos
y plataformas educativas. También pueden encontrarse versiones basadas
en microcontroladores y matrices LED. Sin embargo, muchas de estas
alternativas utilizan bibliotecas de alto nivel o plataformas que
ocultan parte del funcionamiento del hardware. La propuesta busca
diferenciarse por implementar la lógica de control sobre STM32 y
desarrollar explícitamente la gestión de periféricos, temporización,
entradas y estados del sistema.

El producto tendrá como usuario principal a una persona que desee jugar
una partida, modificar algunos parámetros y consultar su récord. Desde
el punto de vista técnico, también estará orientado a demostrar el uso
integrado de ADC, GPIO, SPI, I2C, UART, PWM e interrupciones.

## **2.1 Requisitos del proyecto**

Los requisitos se organizan por grupos funcionales. Se consideran
requisitos funcionales (RF), que describen los servicios que debe
ofrecer el sistema, y requisitos no funcionales (RNF), que establecen
restricciones de implementación, desempeño y robustez. Tales requisitos se muestran en la tabla 2.1:

| Grupo | ID | Tipo | Descripción |
| :--- | :---: | :--- | :--- |
| Juego | RF01 | Funcional | El sistema representará una serpiente y una fruta dentro de un área de juego de 8 × 8 posiciones. |
| Juego | RF02 | Funcional | El usuario podrá controlar la dirección de movimiento mediante el joystick analógico. |
| Juego | RF03 | Funcional | La serpiente avanzará automáticamente con una velocidad determinada por el nivel de dificultad. |
| Juego | RF04 | Funcional | Cuando la serpiente alcance la fruta, aumentará su longitud y el puntaje se incrementará en una unidad. |
| Juego | RF05 | Funcional | Luego de recolectar una fruta, el sistema generará una nueva posición para ella que no esté ocupada por la serpiente. |
| Juego | RF06 | Funcional | El sistema detectará colisiones contra los límites del área de juego y contra el cuerpo de la serpiente. |
| Juego | RF07 | Funcional | Al producirse una colisión, la partida finalizará y se mostrará el estado de fin de juego. |
| Juego | RF08 | Funcional | El sistema permitirá iniciar una nueva partida y reiniciar el puntaje de la partida anterior. |
| Interfaz visual | RF09 | Funcional | La matriz LED 8 × 8 mostrará la posición de la serpiente y de la fruta. |
| Interfaz visual | RF10 | Funcional | La pantalla LCD 16 × 2 mostrará el puntaje, la velocidad, las opciones del menú y los mensajes de estado, si se incorpora al prototipo. |
| Control | RF11 | Funcional | El sistema interpretará las lecturas del joystick como órdenes de movimiento hacia arriba, abajo, izquierda o derecha. |
| Control | RF12 | Funcional | El sistema rechazará cambios de dirección que produzcan un giro de 180 grados inmediato, para evitar que la serpiente choque contra sí misma por una inversión directa. |
| Control | RF13 | Funcional | Los pulsadores permitirán pausar, reanudar o reiniciar la partida, de acuerdo con la configuración de controles. |
| Configuración | RF14 | Funcional | El usuario podrá seleccionar el nivel de dificultad o la velocidad inicial desde el menú, si se implementa la interfaz LCD. |
| Configuración | RF15 | Funcional | El usuario podrá habilitar o deshabilitar los efectos de sonido, si se incorpora el buzzer. |
| Persistencia | RF16 | Funcional | El sistema almacenará el récord máximo en memoria no volátil, mediante EEPROM externa o Flash interna. |
| Persistencia | RF17 | Funcional | El sistema permitirá consultar el récord máximo almacenado. |
| Persistencia | RF18 | Funcional | El sistema actualizará el récord cuando el puntaje de una partida supere el valor histórico. |
| Audio | RF19 | Funcional | El buzzer emitirá señales sonoras para indicar eventos como recolección de fruta, cambio de nivel o fin de partida, si se incorpora. |
| Comunicación | RF20 | Funcional | El módulo Bluetooth permitirá transmitir el puntaje y el estado de la partida hacia un dispositivo externo, si se incorpora. |
| Comunicación | RF21 | Funcional | El sistema podrá recibir comandos de pausa o reinicio mediante Bluetooth, si se incorpora. |
| Señalización | RF22 | Funcional | Los LEDs indicadores señalarán los estados principales: juego, configuración y fin de partida. |
| Firmware | RNF01 | No funcional | El firmware se desarrollará en lenguaje C para el microcontrolador STM32F103RB y no utilizará un RTOS. |
| Firmware | RNF02 | No funcional | El código se organizará mediante el patrón Escrutar–Procesar–Actuar, separando lectura de entradas, lógica del juego y actualización de salidas. |
| Firmware | RNF03 | No funcional | La lógica de funcionamiento se implementará mediante una máquina de estados finitos. |
| Temporización | RNF04 | No funcional | El programa principal utilizará un Super-Loop coordinado por una base de tiempo generada mediante SysTick, con período de 1 ms cuando la planificación del firmware lo requiera. |
| Temporización | RNF05 | No funcional | No se utilizarán retardos bloqueantes como `HAL_Delay` ni ciclos de espera activa para temporizar el juego. |
| Robustez | RNF06 | No funcional | Las entradas de pulsadores deberán contar con antirrebote por software. |
| Robustez | RNF07 | No funcional | El sistema deberá iniciar en un estado seguro, con salidas visuales y sonoras en condiciones conocidas. |
| Hardware | RNF08 | No funcional | El montaje final deberá realizarse sobre una placa rígida, con componentes y conexiones correctamente fijados; no se admitirán protoboards ni cables sueltos. |
| Código | RNF09 | No funcional | El firmware deberá mantener una estructura modular, con responsabilidades diferenciadas y nombres claros para variables, funciones y módulos. |
| Comunicación | RNF10 | No funcional | La comunicación con los periféricos deberá respetar los protocolos eléctricos y temporales correspondientes (SPI, I2C y UART). |

*Tabla 2.1: Requisitos funcionales y no funcionales del proyecto.*

Los requisitos identificados como "si se incorpora" corresponden a
funcionalidades de ampliación. La prioridad de implementación será
asegurar primero el funcionamiento de la partida y la visualización en
la matriz LED. La disponibilidad de tiempo y componentes determinará la
integración de las funcionalidades adicionales.

## **2.2 Caso de uso 1: El usuario inicia y juega una partida**

| Elemento | Definición |
| :--- | :--- |
| **Actor principal** | Usuario / Jugador. |
| **Disparador** | El usuario selecciona la opción "Nuevo juego" o presiona el botón de inicio/reinicio. |
| **Precondiciones** | El sistema está encendido, los periféricos fueron inicializados y no existe otra partida en ejecución. La matriz LED se encuentra disponible para mostrar el área de juego. |
| **Flujo principal** | 1. El usuario inicia una nueva partida. 2. El sistema inicializa el puntaje, la posición y longitud inicial de la serpiente, y genera una posición válida para la primera fruta. 3. La matriz LED muestra la serpiente y la fruta. 4. El usuario mueve el joystick para indicar una dirección. 5. El sistema lee el joystick mediante el ADC, interpreta la dirección y actualiza el movimiento de la serpiente en el siguiente instante de avance. 6. Si la serpiente alcanza la fruta, el sistema incrementa su longitud y el puntaje, y genera una nueva fruta en una posición libre. 7. El sistema actualiza la matriz LED y, si está disponible, el LCD con el puntaje actualizado. 8. El ciclo continúa mientras no se produzca una colisión ni se pause la partida. |
| **Flujos alternativos** | **a) Colisión:** si la cabeza de la serpiente alcanza un borde o una parte de su propio cuerpo, el sistema finaliza la partida y pasa al caso de uso 3. **b) Pausa:** si el usuario presiona el botón de pausa, el sistema detiene el avance de la serpiente y muestra el estado de pausa; al recibir una orden de reanudación continúa la partida. **c) Dirección inválida:** si el usuario intenta invertir inmediatamente el sentido de movimiento, el sistema ignora esa orden y conserva la dirección actual. |


*Tabla 2.2: Caso de uso 1: El usuario inicia y juega una partida.*

## **2.3 Caso de uso 2: El usuario configura la dificultad y las opciones**

| Elemento | Definición |
| :--- | :--- |
| **Actor principal** | Usuario. |
| **Disparador** | El usuario selecciona la opción "Configuración" desde el menú principal o conmuta la entrada asignada al modo SET_UP. |
| **Precondiciones** | El sistema está encendido y no hay una partida en curso. La interfaz de configuración está disponible. |
| **Flujo principal** | 1. El usuario ingresa al modo de configuración. 2. El sistema pausa o impide el inicio de una partida y muestra las opciones disponibles en el LCD, si está instalado. 3. El usuario navega por las opciones mediante el joystick o los botones auxiliares. 4. El usuario selecciona un nivel de dificultad o una velocidad inicial. 5. El sistema actualiza el parámetro correspondiente. 6. Si está implementado el almacenamiento no volátil, el sistema guarda la configuración en la EEPROM o Flash. 7. El sistema informa que la configuración fue aplicada y vuelve al menú principal. |
| **Flujos alternativos** | **a) Cancelación:** si el usuario sale del menú sin confirmar, el sistema conserva la configuración anterior. **b) Valor fuera de rango:** si se detecta una selección inválida, el sistema la descarta y mantiene el último valor válido. **c) Error de almacenamiento:** si falla la escritura en la memoria no volátil, el sistema informa el error y conserva la configuración en memoria RAM durante la sesión, siempre que sea posible. |


*Tabla 2.3: Caso de uso 2: El usuario configura la dificultad y las
opciones.*

## **2.4 Caso de uso 3: El sistema finaliza una partida y actualiza el récord**

| Elemento | Definición |
| :--- | :--- |
| **Actor principal** | Sistema. |
| **Actor secundario** | Usuario. |
| **Disparador** | La cabeza de la serpiente colisiona contra un borde del área de juego o contra su propio cuerpo. |
| **Precondiciones** | Existe una partida en curso y el sistema está actualizando periódicamente la posición de la serpiente. |
| **Flujo principal** | 1. El sistema detecta la colisión durante el procesamiento de la lógica del juego. 2. La máquina de estados cambia al estado `GAME_OVER`. 3. El sistema detiene el movimiento de la serpiente y conserva el puntaje final. 4. El sistema compara el puntaje obtenido con el récord histórico almacenado. 5. Si el puntaje actual supera el récord, actualiza el valor en la memoria no volátil, si está disponible. 6. El sistema muestra el mensaje "GAME OVER" y el puntaje final en el LCD, si está instalado. 7. La matriz LED presenta una señal visual de fin de partida y el LED indicador correspondiente se enciende. 8. Si está instalado el módulo Bluetooth, el sistema transmite el resultado de la partida y la indicación de nuevo récord cuando corresponda. 9. El usuario puede iniciar una nueva partida o regresar al menú principal. |
| **Flujos alternativos** | **a) No se supera el récord:** el sistema conserva el valor histórico y muestra únicamente el puntaje de la partida finalizada. **b) Error de EEPROM o Flash:** el sistema informa que no pudo guardar el nuevo récord, pero mantiene el puntaje final en pantalla durante la sesión. **c) Bluetooth no disponible:** la partida finaliza normalmente; se omite el envío de estadísticas y el resto de las funciones continúa operativo. |


*Tabla 2.4: Caso de uso 3: El sistema finaliza una partida y actualiza
el récord.*

------------------------------------------------------------------------

# **3. Organización preliminar del desarrollo**

El desarrollo se realizará de manera incremental, priorizando la
integración y validación de los módulos principales antes de incorporar
las funcionalidades de ampliación.

Una posible división del firmware se muestra en la tabla 3.1:

| Módulo | Responsabilidad principal |
| :--- | :--- |
| `task_sensor` | Leer joystick, pulsadores, dip switches y datos recibidos por UART. Aplicar filtrado y antirrebote cuando corresponda. |
| `task_system` | Ejecutar la máquina de estados, actualizar la lógica de Snake, controlar puntaje, dificultad y colisiones. |
| `task_actuator` | Actualizar la matriz LED, el LCD, los LEDs indicadores y el buzzer. |
| `task_storage` | Leer y escribir configuraciones y récords en EEPROM o Flash, si se implementa. |
| `task_communication` | Gestionar el protocolo de intercambio de datos con el módulo Bluetooth, si se implementa. |
| `scheduler` | Mantener la base de tiempo y ejecutar las tareas periódicas sin bloqueos. |

La interrupción de `SysTick` se utilizará para mantener una referencia
temporal del sistema. Las rutinas de interrupción deberán ser breves:
actualizarán contadores o banderas y dejarán el procesamiento de la
lógica para el bucle principal. La frecuencia de actualización de la
matriz y la velocidad del juego serán independientes de la base de
tiempo de 1 ms.

Se evitarán las operaciones prolongadas dentro de las interrupciones y
los retardos bloqueantes. Las comunicaciones y escrituras en memoria se
realizarán de forma controlada, procurando que no interrumpan la
actualización del juego.

## **3.1 Criterios de validación**

Se considerará que el prototipo cumple con el alcance principal cuando
se verifique que:

-   La matriz LED representa correctamente la serpiente y la fruta.
-   El joystick permite cambiar la dirección de movimiento sin generar
    inversiones inmediatas inválidas.
-   La serpiente avanza a intervalos regulares y el usuario puede
    modificar su dirección durante la partida.
-   La recolección de una fruta incrementa el puntaje y la longitud de
    la serpiente.
-   Las colisiones contra los bordes y contra el propio cuerpo finalizan
    la partida.
-   El usuario puede iniciar una nueva partida sin reiniciar físicamente
    el microcontrolador.
-   El montaje final cumple con las condiciones de fijación y conexión
    establecidas para la entrega.

Como validaciones adicionales, se comprobará el funcionamiento del menú,
la persistencia del récord, la generación de sonidos y el intercambio de
información por Bluetooth.

------------------------------------------------------------------------

**Nota de planificación:** esta propuesta describe el alcance previsto
del proyecto. La disponibilidad de componentes, las pruebas de
integración y el tiempo restante de cursada podrán llevar a priorizar o
postergar funcionalidades identificadas como adicionales, sin
comprometer los requisitos centrales del juego.
