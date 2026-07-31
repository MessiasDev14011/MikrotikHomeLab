# 🖧 MikroTik Homelab - Infraestrutura e Redes

> Laboratório pessoal desenvolvido para aprofundar conhecimentos em infraestrutura de redes, MikroTik RouterOS, segmentação de redes, firewall, NAT, roteamento e futuramente segurança ofensiva e defensiva.

---

# 📖 Sobre o projeto

Este laboratório foi criado com o objetivo de simular um ambiente corporativo real utilizando equipamentos físicos.

Toda a configuração foi realizada manualmente, sem utilizar assistentes (DHCP Setup), permitindo compreender o funcionamento interno do RouterOS.

O projeto continuará evoluindo com a implementação de VLANs, VPN, monitoramento, servidores Linux, firewall avançado e hardening.

---

# 🎯 Objetivos

- Aprender MikroTik RouterOS
- Configurar redes manualmente
- Criar um laboratório para estudos de Infraestrutura
- Documentar toda a evolução do ambiente
- Criar portfólio para futuras vagas de Redes e Segurança

---

# 🖥️ Equipamentos

- MikroTik hEX (RB750Gr3)
- Roteador EX521 (Access Point)
- Computador Linux (CachyOS)
- ONT do provedor

---

# 🌐 Topologia

```text
                    INTERNET
                        │
                    ONT / ONU
                        │
                  ether1 (WAN)
                   MikroTik hEX
        ┌────────────┼────────────┐
        │            │            │
     ether2       ether3       futuras redes
      ADMIN          LAB
192.168.10.0    192.168.20.0
        │            │
      Meu PC      EX521 (AP)
                  192.168.20.2
```

---

# 📡 Planejamento das redes

| Rede | Endereço |
|-------|----------|
| ADMIN | 192.168.10.0/24 |
| LAB | 192.168.20.0/24 |
| SERVERS | Futuro |
| TEST | Futuro |

---

# 🔌 Interfaces

| Porta | Nome |
|--------|------|
| ether1 | WAN |
| ether2 | ADMIN |
| ether3 | LAB |
| ether4 | SERVERS |
| ether5 | TEST |

---

# ⚙️ Configuração realizada

## WAN

- Interface dedicada
- Cliente PPPoE
- Conexão com Internet

---

## Rede ADMIN

Gateway

192.168.10.1

DHCP

192.168.10.100 - 192.168.10.200

Utilizada para administração do ambiente.

---

## Rede LAB

Gateway

192.168.20.1

DHCP

192.168.20.100 - 192.168.20.200

Destinada para equipamentos de teste.

---

## NAT

Configuração manual utilizando:

- Chain: srcnat
- Action: masquerade
- Out Interface List: WAN

---

## Interface Lists

### WAN

- PPPoE Client

### LAN

- ADMIN
- LAB

---

# 🛡️ Firewall

Estrutura baseada nas boas práticas do MikroTik.

Em desenvolvimento:

- Bloqueio da LAB para ADMIN
- Permissão da ADMIN para LAB
- Restrição de gerenciamento
- Publicação de serviços internos

---

# 📶 Access Point

Equipamento

EX521

Configuração

- DHCP desativado
- Gateway apontando para o MikroTik
- Funcionando apenas como Access Point

---

# 🔍 Ferramentas utilizadas

- WinBox
- Torch
- Ping
- Traceroute
- Terminal RouterOS
- Linux Shell

---

# 🧠 Conceitos estudados

- PPPoE
- DHCP
- DHCP Pool
- DHCP Network
- NAT
- Masquerade
- Firewall
- Interface Lists
- Bridge
- Roteamento
- Gateway
- ICMP
- TCP/IP

---

# ⚠️ Problemas encontrados

## Gateway inacessível

Sintoma

PC recebia endereço IP, porém não respondia ao Gateway.

Solução

Reconfiguração da interface e revisão do DHCP.

---

## Interface Not Running

Causa

Ausência de link físico.

Solução

Correção da conexão física da porta.

---

## PPPoE conectado porém sem Internet

Causa

A interface LAB foi adicionada por engano à Interface List WAN.

Solução

Remoção da LAB da Interface List WAN.

---

## NAT

Problema

Internet funcionava apenas no MikroTik.

Solução

Correção da configuração de Masquerade.

---

# 📚 Lições aprendidas

Durante o desenvolvimento deste laboratório foi possível compreender na prática:

- Funcionamento do PPPoE
- Processo de autenticação
- Segmentação de redes
- Funcionamento de Bridges
- Interfaces físicas e lógicas
- DHCP manual
- Interface Lists
- NAT
- Masquerade
- Firewall
- Diagnóstico utilizando Torch
- Diagnóstico utilizando Ping
- Diagnóstico utilizando Traceroute
- Troubleshooting em RouterOS

---

# 🚀 Próximas implementações

- VLANs
- WireGuard VPN
- QoS
- Firewall avançado
- DMZ
- IDS/IPS
- Servidor Linux
- Grafana
- The Dude
- Syslog
- DNS interno
- Reverse Proxy
- Servidor Web
- Servidor SSH
- Monitoramento SNMP


---

# 🛠️ Tecnologias

- MikroTik RouterOS
- Linux
- PPPoE
- DHCP
- Firewall
- NAT
- ICMP
- TCP/IP
- WinBox
- Access Point
- Roteamento IPv4

---

# 🎯 Objetivo final

Este laboratório continuará evoluindo até se tornar um ambiente completo de estudos para:

- Infraestrutura de Redes
- Administração de Sistemas
- Segurança da Informação
- Pentest em ambientes controlados
- Monitoramento
- Hardening
- Virtualização
- Automação

Todo o ambiente será documentado conforme novas implementações forem realizadas.

---

## 👨‍💻 Autor

Projeto desenvolvido por **Miguel Messias**como laboratório de estudos em Infraestrutura de Redes e Segurança da Informação.
