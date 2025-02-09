# Nsniffer – Herramienta de Análisis y Seguridad de Redes

## Descripción

[Nsniffer](https://github.com/vorosdev/Nsniffer) es una herramienta de red multifuncional 
diseñada para capturar y analizar tráfico de red, realizar ataques Man-in-the-Middle (MITM), 
escanear dispositivos en la red y llevar a cabo otras tareas relacionadas con la seguridad y
auditoría de redes. Está especialmente orientada a propósitos educativos y pruebas de 
penetración en entornos controlados.

**Importante:** Este software está destinado a un uso ético y educativo. Asegúrate de contar 
con los permisos necesarios antes de utilizarlo.

---

## Características

- **Captura de tráfico en tiempo real** utilizando TShark.
- **Guardado de capturas** en formato `.pcap` o `.pcapng`.
- **Extracción de datos en formato hexadecimal** y conversión a texto legible.
- **Ataques de spoofing ARP** y **Man-in-the-Middle**.
- **Escaneo de dispositivos en la red local**.
- **Análisis de archivos de captura**.
- **Listado de interfaces de red** disponibles.
- **Modo de captura en intervalos y registro** de la data en log.
- **Instalación automatizada** de dependencias (en sistemas soportados).

---

## Uso General

En esta actualizacion `nsniffer` utiliza **subcomandos** en lugar de un único comando con múltiples flags.  
La forma general de uso es:

```bash
nsniffer <subcommand> [opciones]
```

Donde `<subcommand>` puede ser uno de los siguientes:

- **capture**: Captura tráfico con TShark.  
- **analyze**: Analiza un archivo `.pcap`.  
- **mitm**: Realiza un ataque MITM entre dos objetivos.  
- **arp**: Lleva a cabo ARP spoofing simple entre un objetivo y una víctima.  
- **scan**: Escanea dispositivos en la red local.  
- **list**: Lista las interfaces de red disponibles.  
- **extract**: Extrae datos hexadecimales y los convierte en texto legible desde un pcap.  
- **log**: Captura tráfico en intervalos definidos y guarda la extracción en un archivo de log.  
- **setup**: Instala o verifica las dependencias necesarias.  
- **help**: Muestra la ayuda de la herramienta.

Para ver la descripción y opciones de cada subcomando, puedes ejecutar:
```bash
nsniffer help
```

---

## Requisitos

- Sistema operativo **Linux**.
- Privilegios de **superusuario (root)** para poder realizar la mayoría de las acciones de sniffing y spoofing.
- Dependencias necesarias:
  - **TShark**
  - **arpspoof** (incluido en la suite **dsniff**)
  - **arp-scan**

Para instalar o verificar dependencias en distribuciones soportadas (Debian, RedHat, Arch), puedes ejecutar:
```bash
nsniffer setup
```
Esto intentará instalar cualquier dependencia faltante.

---

## Instalación

1. **Clona este repositorio**:
   ```bash
   git clone https://github.com/vorosdev/Nsniffer.git
   cd Nsniffer
   ```
2. **Dale permisos de ejecución**:
   ```bash
   chmod +x nsniffer
   ```
3. **(Opcional) Añádelo a tu PATH** para poder usarlo desde cualquier ubicación:
   ```bash
   sudo cp nsniffer /usr/local/bin/nsniffer
   ```

---

## Subcomandos Principales y Ejemplos

### 1. Captura de tráfico (capture)

```bash
nsniffer capture -i <interfaz> [-o <archivo.pcap>] [-f <filtro>] [--live]
```
- **-i, --interface**: Interfaz de red, por ejemplo `eth0`.
- **-o, --output**: Archivo de salida. Por defecto, `output.pcap`.
- **-f, --filter**: Filtro TShark (p.ej. `"host 172.10.23.111 and port 2222 and tcp"`).
- **--live**: Muestra los paquetes capturados en tiempo real (`-x` en TShark).

**Ejemplo**:
```bash
nsniffer capture -i eth0 -o capture.pcap -f "port 443 and tcp" --live
```

### 2. Análisis de un archivo pcap (analyze)

```bash
nsniffer analyze <archivo.pcap>
```
Muestra un análisis de texto del archivo especificado.

**Ejemplo**:
```bash
nsniffer analyze capture.pcap
```

### 3. Man-in-the-Middle (mitm)

```bash
nsniffer mitm <interfaz> <IP_objetivo1> <IP_objetivo2>
```
Habilita IP forwarding y ejecuta `arpspoof` para ambas direcciones.

**Ejemplo**:
```bash
nsniffer mitm eth0 192.168.0.10 192.168.0.20
```

### 4. ARP Spoofing simple (arp)

```bash
nsniffer arp <interfaz> <IP_objetivo> <IP_victima>
```
Inicia `arpspoof` de un objetivo a una víctima específica, sin MITM completo.

**Ejemplo**:
```bash
nsniffer arp eth0 192.168.0.1 192.168.0.50
```

### 5. Escaneo de red (scan)

```bash
nsniffer scan <interfaz>
```
Realiza un escaneo ARP con `arp-scan`.

**Ejemplo**:
```bash
nsniffer scan eth0
```

### 6. Listar interfaces (list)

```bash
nsniffer list
```
Muestra las interfaces de red disponibles en el sistema.

### 7. Extracción de datos hexadecimales (extract)

```bash
nsniffer extract <archivo.pcap>
```
Busca tramas con longitud mayor a 100 bytes y convierte datos hexadecimales a texto legible.

**Ejemplo**:
```bash
nsniffer extract capture.pcap
```

### 8. Captura en intervalos y registro (log)

```bash
nsniffer log <interfaz> <archivo_log> [intervalo_segundos=5]
```
Inicia un bucle de capturas: cada cierto intervalo (por defecto, 5s) detiene la captura, extrae datos hex y los escribe en el `archivo_log`.

**Ejemplo**:
```bash
nsniffer log eth0 mycapture.log 10
```
Cada 10 segundos se detiene la captura temporal, se extraen datos y se guardan en `mycapture.log`, luego se reinicia.

### 9. Configuración (setup)

```bash
nsniffer setup
```
Verifica e instala dependencias requeridas (TShark, arpspoof, arp-scan) si el sistema operativo es soportado.

### 10. Ayuda (help)

```bash
nsniffer help
```
Muestra la descripción general de todos los subcomandos y su uso.

---

## Ejemplos Adicionales

#### Escanear la red local

```bash
nsniffer scan eth0
```
Encuentra hosts activos en la subred asignada a `eth0` usando `arp-scan`.

#### Realizar un MITM

```bash
nsniffer mitm eth0 192.168.1.10 192.168.1.20
```
Ataca la comunicación entre la IP 192.168.1.10 y 192.168.1.20 a través de la interfaz `eth0`.

#### Captura y conversión de datos en modo "log"

```bash
nsniffer log eth0 capture.log
```
Cada 5 segundos (por defecto), detiene la captura, extrae datos hexadecimales y los agrega a `capture.log`.  
Presiona **Enter** para reiniciar el ciclo o **Ctrl+C** para salir.

---

> **Advertencia**  
> - Utiliza este software **únicamente** en redes bajo tu control o con autorización explícita.  
> - El uso malintencionado de esta herramienta puede ser **ilegal** y derivar en sanciones graves.

---

## Licencia

Este proyecto está bajo la licencia **MIT**. Consulta el archivo [LICENSE](LICENSE) para más información.

---

## Disclaimer

Este proyecto está destinado únicamente a fines educativos y éticos. El autor no se hace responsable del 
uso indebido de esta herramienta. Para más información, consulta el archivo [DISCLAIMER.md](DISCLAIMER.md).
