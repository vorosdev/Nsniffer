# Network Sniffer Tool

## Descripción

Este script Bash es una herramienta de red multifuncional diseñada para capturar y analizar tráfico de red, 
realizar ataques Man-in-the-Middle (MITM), escanear dispositivos en la red y realizar otras tareas 
relacionadas con el análisis y seguridad de la red. Es útil para propósitos educativos y de auditoría ética.

**Nota:** Este script está destinado exclusivamente para propósitos éticos y educativos. Por favor, asegúrate 
de contar con los permisos necesarios antes de realizar cualquier actividad.

---

## Características

- Captura tráfico de red en tiempo real utilizando TShark.
- Guarda tráfico capturado en archivos `.pcap` o `.pcapng`.
- Extrae datos en formato hexadecimal y los convierte a texto legible.
- Realiza ataques de spoofing ARP y MITM.
- Escanea dispositivos en la red local.
- Analiza archivos de captura.
- Lista interfaces de red disponibles.
- Modo interactivo para capturas controladas.
- Visualiza paquetes capturados en tiempo real durante la captura.

---

## Uso

```python
Usage: nsniffer [OPTIONS]

Options:
  -i, --interface   Network interface to capture traffic (required). Example: -i eth0
  -o, --output      Output file to save captured packets (default: output.pcap). Example: -o capture.pcap
  -f, --filter      Capture filter for TShark (optional). Example: -f "tcp.port == 80"
  -a, --analyze     Analyze the data after capture.
  -m, --mitm        Perform a Man-in-the-Middle attack. Example: -m 192.168.0.1 192.168.0.2
  -s, --scan        Scan for ARP devices on the local network.
  -l, --list        List available network interfaces.
  -x, --extract     Extract hexadecimal data in human-readable text.
  -I, --interactive Enter interactive mode for controlled captures. Example: -I -g capture.log
  -g, --log         Capture real-time traffic and save extracted data in a log. Example: -g capture.log
  -h, --help        Display this help message.
  -arp, --arp       Perform ARP spoofing for a single target. Example: -arp 192.168.0.1 192.168.0.2
  -live, --live     Display captured packets in real-time during capture. Example: -i eth0 -f "tcp" -o capture.pcap --live
  -setup, --setup   Install necessary packages for the tool.
  -debug, --debug   Enable debug mode to show additional logs.
```

---

## Requisitos

- Sistema operativo basado en Linux.
- Privilegios de superusuario (root).
- Dependencias necesarias:
  - TShark
  - dsniff
  - arp-scan

Para instalar dependencias, ejecuta:

```bash
./nsniffer --setup
```

---

## Instalación

1. Clona este repositorio:
   ```csharp
   git clone https://github.com/vorosdev/Nsniffer.git
   cd Nsniffer
   ```
2. Dale permisos de ejecucion
   ```csharp
   chmod +x nsniffer
   ```
3. Añadirlo en el path (opcional) 
   ```csharp
    cp nsniffer /usr/local/bin/nsniffer
   ```

---

## Ejemplos de Uso

### Capturar tráfico en una interfaz específica
```csharp
nsniffer -i eth0 -o traffic.pcap
```

### Realizar un ataque MITM entre dos objetivos
```csharp
nsniffer -i eth0 -m 192.168.1.10 192.168.1.20
```

### Escanear dispositivos en la red local
```csharp
nsniffer -s -i eth0
```

### Captura y conversión de datos en modo interactivo
```csharp
nsniffer -i eth0 -I -g capture.log
```

---

> [!WARNING]
> - **Utiliza este software únicamente en redes bajo tu control o con autorización explícita.**
> - El uso inadecuado de esta herramienta puede ser ilegal y tener consecuencias graves.

---

## Licencia

Este proyecto está bajo la licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

## Disclaimer

Este proyecto está destinado para propósitos educativos y éticos exclusivamente. El autor no se hace responsable 
del mal uso de esta herramienta. Consulta el archivo [DISCLAIMER.md](DISCLAIMER.md) para más detalles.
