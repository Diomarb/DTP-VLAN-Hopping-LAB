# DTP-VLAN-Hopping-LAB
Ataque DTP VLAN Hopping: Convierta una interfaz de acceso en una interfaz troncal
| **Red:** 10.11.85.0/24 | **Plataforma:** GNS3 — Cisco IOS L2
Link de Youtube: https://youtu.be/2Okw52qOtas
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
- Puerto e1/0 del switch en modo `dynamic desirable` o `dynamic auto`

---

## Uso

```bash
sudo python3 dtp_vlan_hopping.py -i eth0 --target-vlan 10 --attacker-ip 10.11.85.200 --count 5
```
<img width="297" height="444" alt="Captura de pantalla 2026-06-09 152743" src="https://github.com/user-attachments/assets/f881b001-f95c-453a-8410-3e32325e52e8" />


### Parámetros

| Parámetro | Descripción | Default |
|-----------|-------------|---------|
| `-i / --iface` | Interfaz de red | requerido |
| `--target-vlan` | VLAN objetivo | 10 |
| `--attacker-ip` | IP del atacante en la VLAN | 10.11.85.200 |
| `--count` | Paquetes DTP a enviar | 5 |
| `--interval` | Segundos entre paquetes | 1.0 |
| `--skip-hop` | Solo enviar DTP, sin crear subinterfaz | False |
| `--cleanup` | Eliminar subinterfaz al terminar | False |

---

## Topología

```
```
<img width="475" height="257" alt="Captura de pantalla 2026-06-09 204653" src="https://github.com/user-attachments/assets/a1583004-2a52-4559-94fa-691ba29c6c7e" />

---

## Verificación en el switch

```
SW-Core# show interfaces trunk
SW-Core# show interfaces e1/0 switchport
```
<img width="608" height="317" alt="Captura de pantalla 2026-06-09 145303" src="https://github.com/user-attachments/assets/139a547c-054c-40f6-8c27-eef6aa3cc24b" />

<img width="481" height="252" alt="Captura de pantalla 2026-06-09 152639" src="https://github.com/user-attachments/assets/eab7b1b5-63b7-4a45-8277-9ed7cc0bdbc5" />

<img width="544" height="259" alt="Captura de pantalla 2026-06-09 152702" src="https://github.com/user-attachments/assets/71c2ecc2-fe43-47e6-99de-698187685906" />

---

## Contra-medida

```
SW-Core(config)# interface Gi0/2
SW-Core(config-if)# switchport mode access
SW-Core(config-if)# switchport nonegotiate
```
<img width="160" height="157" alt="Captura de pantalla 2026-06-09 201507" src="https://github.com/user-attachments/assets/72ff3acc-d13a-4848-99b5-062fc0e11e97" />

---

## Archivos

```
dtp-vlan-hopping/
├── dtp_vlan_hopping.py
├── README.md
└── docs/
    └── DTP_VLANHopping_Documentacion.docx
```

