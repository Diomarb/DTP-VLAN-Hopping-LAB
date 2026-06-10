# DTP / VLAN Hopping Attack

**Matrícula:** 2024-1185 | **Red:** 10.11.85.0/24 | **Plataforma:** GNS3 — Cisco IOS L2

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
- Puerto Gi0/2 del switch en modo `dynamic desirable` o `dynamic auto`

---

## Uso

```bash
sudo python3 dtp_vlan_hopping.py -i eth0 --target-vlan 10 --attacker-ip 10.11.85.200 --count 5
```

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
[Kali: 10.11.85.50] ──Gi0/2 (dynamic desirable)──► [SW-Core: 10.11.85.1]
                                                      /              \
                                               [SW-Access1]     [SW-Access2]
                                               10.11.85.2        10.11.85.3
                                               PC1 (.10) VLAN10  PC3 (.12) VLAN10
                                               PC2 (.11) VLAN20  PC4 (.13) VLAN20
```

---

## Verificación en el switch

```
SW-Core# show interfaces trunk
SW-Core# show interfaces Gi0/2 switchport
```

---

## Contra-medida

```
SW-Core(config)# interface Gi0/2
SW-Core(config-if)# switchport mode access
SW-Core(config-if)# switchport nonegotiate
```

---

## Archivos

```
dtp-vlan-hopping/
├── dtp_vlan_hopping.py
├── README.md
└── docs/
    └── DTP_VLANHopping_Documentacion.docx
```
