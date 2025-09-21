# Anycubic-Kobra-2-Klipper
Instalación de Klipper en la Anycubic Kobra 2 usando una Raspberry Pi 4 como host. Desbloquea el rendimiento de la impresora (Input Shaping, Pressure Advance) con hardware 100% de fábrica, sin modificaciones físicas. Incluye el archivo printer.cfg base para empezar.

Klipper para Anycubic Kobra 2 (Hardware Stock)
Repositorio para instalar Klipper en la Anycubic Kobra 2 usando una Raspberry Pi 4. Desbloquea el rendimiento de la impresora (Input Shaping, Pressure Advance) con hardware 100% de fábrica, sin modificaciones físicas.
📖 Introducción
Este repositorio contiene una guía completa y los archivos de configuración necesarios para instalar y configurar el firmware Klipper en una impresora 3D Anycubic Kobra 2, utilizando una Raspberry Pi 4 como host.

El objetivo principal es desbloquear todo el potencial de la impresora, superando las limitaciones del firmware de fábrica. Al instalar Klipper, obtendrás control total sobre la máquina, lo que se traduce en una mejora sustancial tanto en la calidad de impresión como en la velocidad.

✨ Características Principales
printer.cfg Optimizado: Un archivo printer.cfg de punto de partida, ajustado a las especificaciones y límites de seguridad del hardware original de la Kobra 2.

Macros Avanzadas: Incluye macros personalizadas como ADAPTIVE_BED_MESH para una nivelación de cama precisa y rápida solo en el área de impresión.

Guía de Calibración: Un flujo de trabajo recomendado para las calibraciones esenciales que aseguran la máxima calidad y precisión dimensional.

Enfoque 100% Stock: Todo el proceso está diseñado para funcionar con la impresora tal como viene de fábrica, haciendo la instalación limpia y reversible.

⚠️ Descargo de Responsabilidad
Este proceso implica flashear un nuevo firmware en tu impresora. Procede con precaución. No me hago responsable de ningún daño que pueda ocurrir en tu equipo. Realiza este procedimiento bajo tu propio riesgo.

🛠️ Requisitos
Hardware
Impresora 3D Anycubic Kobra 2.

Raspberry Pi 4 (se recomienda 2GB o más).

Tarjeta microSD de buena calidad (16GB o más) para la Raspberry Pi.

Tarjeta microSD para la impresora.

Fuente de alimentación adecuada para la Raspberry Pi.

Cable USB para conectar la Pi a la impresora.

Software
Raspberry Pi Imager: Para instalar el sistema operativo en la Pi.

Cliente SSH: Como PuTTY (Windows) o el terminal nativo (macOS/Linux).

Slicer: OrcaSlicer, PrusaSlicer o Cura.

⚙️ Guía de Instalación
1. Preparar la Raspberry Pi
Usa Raspberry Pi Imager para instalar "Raspberry Pi OS Lite (64-bit)" en tu tarjeta microSD.

Antes de escribir la imagen, configura las opciones avanzadas (Ctrl+Shift+X) para establecer un nombre de host, activar SSH y configurar tu red Wi-Fi.

2. Instalar Klipper, Moonraker y Mainsail/Fluidd
La forma más sencilla es usar KIAUH (Klipper Installation And Update Helper).

Conéctate a tu Raspberry Pi por SSH.

Clona el repositorio de KIAUH: git clone https://github.com/th33xitus/kiauh.git

Ejecuta KIAUH: ./kiauh/kiauh.sh

Usa el menú para instalar, en orden: 1) Klipper, 2) Moonraker, y luego 3) Mainsail o Fluidd (tu interfaz web preferida).

3. Compilar el Firmware para la Kobra 2
Desde el menú principal de KIAUH, ve a Avanzado -> Construir solo.

Se abrirá el menú de configuración de Klipper (make menuconfig). Configúralo con estos parámetros exactos:

Micro-controller: Huada Semiconductor HC32F460

Communication interface: Serial (on PA3/PA2)

Application Address: 0x008000

Guarda y sal. El firmware se compilará.

4. Flashear la Impresora
Conéctate a la Pi con un cliente SFTP (como FileZilla) o desde la terminal.

Navega a la carpeta ~/klipper/out y encontrarás un archivo klipper.bin.

Renombra ese archivo a firmware.bin.

Copia firmware.bin a la raíz de la tarjeta microSD de la impresora.

Con la impresora apagada, inserta la microSD y enciéndela. La impresora emitirá unos pitidos mientras se actualiza. El proceso tarda aproximadamente un minuto.

5. Configuración Final
Sube el archivo printer.cfg de este repositorio a tu interfaz web (Mainsail/Fluidd) en la sección "Configuración".

Encuentra el ID de serie de tu impresora. Conéctate por SSH y ejecuta: ls /dev/serial/by-id/*

Copia la dirección que aparezca y pégala en la sección [mcu] de tu printer.cfg.

Guarda y reinicia. Si todo está correcto, Klipper se conectará a la impresora.

🔬 Calibración Esencial (Orden Recomendado)
Una vez instalado, debes calibrar tu impresora para obtener la mejor calidad.

PID Tuning: Calibra la estabilidad de temperatura del extrusor y la cama.

Z-Offset: El paso más crítico para una primera capa perfecta. Usa PROBE_CALIBRATE.

Bed Mesh: Crea un mapa de la cama con BED_MESH_CALIBRATE y guárdalo con SAVE_CONFIG.

Flow Rate (Ratio de Flujo): Usa la herramienta de calibración de tu Slicer para evitar sobre/sub-extrusión.

Tolerancias (Expansión Horizontal): Imprime un test de tolerancias y ajusta la "Expansión Horizontal" en tu Slicer para un encaje perfecto de las piezas.

Retracciones: Imprime una torre de retracción para minimizar los hilos (stringing).

🪛 Troubleshooting: Problemas Comunes
Heat Creep (El Filamento se Expande y Atasca)
Este modelo de impresora es propenso a un problema donde el calor sube por el hotend y ablanda el filamento antes de tiempo, causando un atasco masivo desde los engranajes. Si experimentas esto:

Causa #1: Ventilador del Hotend Deficiente: La causa más probable es que el ventilador no esté refrigerando al 100%. Esto puede deberse a que la placa base no está entregando los 24V completos al puerto del ventilador. La única forma de confirmarlo es con un multímetro.

Causa #2: Filamento de Mala Calidad/Húmedo: Prueba con un rollo de filamento nuevo y de alta calidad para descartar que el material se esté ablandando a muy baja temperatura.

Causa #3: "Gap" en el Tubo de PTFE: Asegúrate de que el tubo de PTFE esté cortado perfectamente plano y haciendo tope contra la boquilla dentro del hotend.

📜 Licencia
Este proyecto se distribuye bajo la licencia MIT. Ver el archivo LICENSE para más detalles.
