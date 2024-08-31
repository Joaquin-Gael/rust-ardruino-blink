# Makefile para Arduino con Rust

Este `Makefile` automatiza el proceso de compilación y carga de código Rust en un Arduino Uno en un entorno Windows.

## Requisitos

- **Rust**: Asegúrate de tener Rust instalado.
- **Herramientas AVR**: Instala `avr-gcc`, `avr-objcopy`, y `avrdude`. Puedes obtenerlas a través de [WinAVR](https://sourceforge.net/projects/winavr/) o el [Arduino IDE](https://www.arduino.cc/en/software).

## Contenido del Makefile

Guarda el siguiente contenido en un archivo llamado `Makefile` en la raíz de tu proyecto:

# Nombre del proyecto
PROJECT_NAME = arduino_blink

# Ruta al archivo de configuración del target
TARGET_JSON = avr-atmega328p.json

# Archivos de salida
ELF_FILE = target/avr-atmega328p/debug/$(PROJECT_NAME).elf
HEX_FILE = target/avr-atmega328p/debug/$(PROJECT_NAME).hex

# Puerto COM para el Arduino (ajustar según tu configuración)
COM_PORT = COM3

# Compilar el código
build:
	cargo build --target $(TARGET_JSON)

# Convertir el archivo ELF a HEX
hex: build
	@echo "Converting ELF to HEX..."
	avr-objcopy -O ihex $(ELF_FILE) $(HEX_FILE)

# Cargar el archivo HEX al Arduino
upload: hex
	@echo "Uploading HEX to Arduino..."
	avrdude -c arduino -p atmega328p -P $(COM_PORT) -U flash:w:$(HEX_FILE)

# Limpiar archivos generados
clean:
	@echo "Cleaning up..."
	cargo clean

# Objetivo por defecto
all: build hex upload
