# DTP-VLAN-Hopping-LAB

Ataque DTP VLAN Hopping: Convierta una interfaz de acceso en una interfaz troncal

| **Red:** 10.11.85.0/24 | **Plataforma:** GNS3 — Cisco IOS L2

Link de Youtube: https://youtu.be/b8EJACneX9g
---

## Descripción

Script que explota el protocolo **DTP (Dynamic Trunking Protocol)** para convertir un puerto de acceso en un enlace trunk 802.1Q, obteniendo acceso a todas las VLANs de la red.

---

## Requisitos

```bash
pip install scapy
sudo python3 dtp_vlan_hopping.py ...
```

- Python 3.8+
- Ejecutar con `sudo`
- Puerto e2/0 del switch en modo `dynamic desirable` o `dynamic auto`

---

## Uso

```bash
sudo python3 dtp_vlan_hopping.py
```

## Red

## Dispositivos

| Dispositivo | Rol | Sistema Operativo | Interfaces usadas | IP / VLAN |
|------------|-----|------------------|------------------|-----------|
| SW-Core | Switch núcleo / VTP Server | Cisco IOS L2 (IOU) | e0/0, e0/1, e2/0 | SVI VLAN10: 10.11.85.1/24 |
| SW-Access1 | Switch de acceso / VTP Client | Cisco IOS L2 (IOU) | e0/0 | - |
| Kali Linux | Máquina atacante | Kali Linux | eth0 | 10.11.85.50/24 |
| PC3 | Host víctima | VPCS | eth0 | 10.11.85.10/24 — VLAN 10 |
| PC4 | Host víctima | VPCS | eth0 | 10.11.85.11/24 — VLAN 20 |

---

## Direccionamiento IP

| Host | Dirección IP | Máscara de Red | Gateway | VLAN |
|------|-------------|---------------|---------|------|
| SW-Core (SVI) | 10.11.85.1 | 255.255.255.0 | - | VLAN 10 |
| SW-Access1 (SVI) | - | - | - | - |
| Kali (Atacante) | 10.11.85.50 | 255.255.255.0 | 10.11.85.1 | Nativa / Trunk |
| PC3 | 10.11.85.10 | 255.255.255.0 | 10.11.85.1 | VLAN 10 |
| PC4 | 10.11.85.11 | 255.255.255.0 | 10.11.85.1 | VLAN 20 |

---

## Configuración de VLANs

| VLAN ID | Nombre | Hosts asignados | Rango de IPs |
|---------|---------|----------------|--------------|
| 10 | VENTAS | PC3 | 10.11.85.10, 10.11.85.12 |
| 20 | RRHH | PC4 | 10.11.85.11, 10.11.85.13 |

---

## Parámetros del Script

| Parámetro Corto | Parámetro Largo | Tipo de Dato | Valor por Defecto |
|----------------|----------------|--------------|------------------|
| `-i` | `--iface` | Texto | `eth0` |
| `-n` | `--count` | Número entero | `5` |
| `-t` | `--intervalo` | Número decimal | `1.0` |
| - | `--dst-ip` | Texto | `10.0.20.10` |
| - | `--solo-dtp` | Booleano (Bandera) | `False` |
| - | `--solo-sniff` | Booleano (Bandera) | `False` |
| - | `--sniff-timeout` | Número entero | `15` |

---

## Descripción de Parámetros

| Parámetro | Descripción |
|-----------|-------------|
| `--iface` | Define la interfaz de red de Kali conectada al switch. |
| `--count` | Cantidad de paquetes DTP enviados durante la negociación. |
| `--intervalo` | Tiempo de espera entre cada paquete DTP enviado. |
| `--dst-ip` | Dirección IP del host objetivo. |
| `--solo-dtp` | Ejecuta únicamente la negociación DTP y termina. |
| `--solo-sniff` | Solo captura tráfico etiquetado sin realizar ataques. |
| `--sniff-timeout` | Tiempo máximo de captura de tráfico en segundos. |

---

## Topología

```
```
<img width="403" height="279" alt="image" src="https://github.com/user-attachments/assets/91e50645-4c7f-4252-acab-d8ad5888197e" />

---

## Verificación en el switch

```
SW-Core# show interfaces trunk
SW-Core# show interfaces e2/0 switchport
```
<img width="291" height="239" alt="Captura de pantalla 2026-06-11 092304" src="https://github.com/user-attachments/assets/cc333f5c-5546-486c-9b29-5a88fbaf9db3" />

## Despues del ataque

<img width="249" height="190" alt="Captura de pantalla 2026-06-11 092343" src="https://github.com/user-attachments/assets/40b3d580-ddb7-4a4b-9791-b49aa968cecf" />


<img width="286" height="218" alt="Captura de pantalla 2026-06-11 092443" src="https://github.com/user-attachments/assets/ff285aa7-6513-4009-926b-a95eae0d7a51" />


<img width="148" height="134" alt="Captura de pantalla 2026-06-11 093311" src="https://github.com/user-attachments/assets/891295c4-bb9d-46d0-bb0e-e30ce3159bef" />

## Contra-medida
```
SW-Core(config)# interface e2/0
SW-Core(config-if)# switchport mode access
SW-Core(config-if)# switchport nonegotiate
```
```
```
<img width="166" height="152" alt="Captura de pantalla 2026-06-11 093204" src="https://github.com/user-attachments/assets/dbfb537b-589f-4cdb-ab96-e029de111fe8" />

## Archivos

`
dtp-vlan-hopping/
├── dtp_vlan_hopping.py
├── README.md
└── docs/
    └── DTP_VLANHopping_Documentacion.docx
```

