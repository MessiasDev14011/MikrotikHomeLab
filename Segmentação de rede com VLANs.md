# Projeto 02 - Segmentação de Rede com VLANs no MikroTik

> Evolução do laboratório inicial, migrando parte da infraestrutura para VLANs utilizando o RouterOS, mantendo uma porta física dedicada para gerenciamento seguro.

---

# Objetivo

Aprender o funcionamento das VLANs no MikroTik, entendendo a diferença entre interfaces físicas e interfaces lógicas, além de preparar o laboratório para futuras expansões com switches gerenciáveis.

---

# Motivação

No laboratório inicial cada rede utilizava uma porta física exclusiva.

Exemplo:

```
ether2 -> ADMIN
ether3 -> LAB
ether4 -> SERVERS
ether5 -> TEST
```

Essa abordagem funciona, porém limita a quantidade de redes ao número de portas disponíveis.

Com VLANs, diversas redes podem trafegar pelo mesmo enlace físico.

---

# Topologia Atual

```
                 INTERNET
                     │
                  PPPoE
                     │
              MikroTik hEX
                     │
      ┌──────────────┼──────────────┐
      │              │              │
   ether2        ether3         ether4
   ADMIN          LAB           RESGATE
Sem VLAN        VLAN20         VLAN30
192.168.10.0    192.168.20.0   192.168.30.0
```

---

# Por que manter a ether2 sem VLAN?

Durante os estudos foi decidido manter uma interface exclusivamente para administração.

Vantagens:

- Evita perda de acesso durante testes.
- Permite recuperar o equipamento caso alguma VLAN seja configurada incorretamente.
- Facilita troubleshooting.
- Simula uma porta de gerenciamento dedicada, comum em ambientes corporativos.

A ether2 continuará sendo utilizada apenas para gerenciamento do MikroTik.

---

# Conceitos aprendidos

## Bridge

A Bridge funciona como um switch virtual.

Ela é responsável por conectar todas as interfaces LAN e aplicar as regras de VLAN.

```
Bridge

├── ether2
├── ether3
├── ether4
└── ether5
```

Sem Bridge VLAN Filtering, a Bridge apenas conecta as portas.

Com Bridge VLAN Filtering, ela passa a respeitar as regras de VLAN.

---

## Interface VLAN

Foi criada uma interface lógica:

```
VLAN20
```

Essa interface representa a rede VLAN 20.

Ela não corresponde a uma porta física.

Foi nela que foram configurados:

- Gateway
- DHCP
- Firewall
- Roteamento

---

## IP Address

Anteriormente:

```
192.168.20.1

↓

ether3
```

Agora:

```
192.168.20.1

↓

VLAN20
```

O gateway pertence à interface VLAN e não mais à porta física.

---

## DHCP

O servidor DHCP passou a atender diretamente a interface VLAN20.

Dessa forma apenas equipamentos pertencentes à VLAN recebem endereços dessa rede.

---

## Bridge VLAN

Foi criada a tabela de VLANs.

Exemplo:

VLAN 20

Tagged:

- bridge-lan

Untagged:

- ether3

Isso informa ao MikroTik como tratar os quadros Ethernet pertencentes à VLAN.

---

## Tagged

Pacotes circulam identificados com o ID da VLAN.

Utilizado entre equipamentos que entendem VLAN.

Exemplo:

- MikroTik
- Switch Gerenciável
- Hypervisor
- Access Point com suporte a VLAN

---

## Untagged

Os pacotes chegam sem identificação.

É utilizado para equipamentos que não suportam VLAN.

Exemplo:

- Computadores
- Impressoras
- Access Points simples

---

## PVID

Na porta ether3 foi configurado:

```
PVID = 20
```

Todo pacote recebido sem identificação passa automaticamente a pertencer à VLAN 20.

Fluxo:

```
EX521

↓

ether3

↓

PVID 20

↓

Bridge adiciona a VLAN20

↓

RouterOS
```

---

## VLAN Filtering

Após toda configuração foi habilitado:

```
Bridge VLAN Filtering = Yes
```

A partir desse momento a Bridge passou a respeitar as regras de VLAN.

Sem essa opção:

Todas as portas pertencem ao mesmo domínio.

Com ela:

Cada VLAN torna-se isolada.

---

# Fluxo do tráfego

```
Celular

↓

EX521

↓

ether3

↓

Bridge

↓

VLAN20

↓

Firewall

↓

NAT

↓

PPPoE

↓

Internet
```

No retorno:

```
Internet

↓

PPPoE

↓

NAT

↓

Firewall

↓

VLAN20

↓

Bridge

↓

ether3

↓

EX521

↓

Celular
```

---

# Testes realizados

✔ Recebimento de IP pela VLAN20.

✔ Gateway acessível.

✔ Navegação para Internet.

✔ Tráfego visível na interface VLAN20.

✔ Tráfego visível na interface física.

---

# Descobertas importantes

O tráfego aparece em dois locais distintos.

## ether3

Mostra o tráfego físico passando pelo cabo.

## VLAN20

Mostra o tráfego processado pela interface lógica da VLAN.

Essa diferença permitiu compreender a separação entre camada física e interface lógica no RouterOS.

---

# Problemas encontrados

## Acesso ao MikroTik pela VLAN

Mesmo com:

- DHCP funcionando
- Internet funcionando
- Gateway respondendo

O acesso ao WinBox pela VLAN não foi possível.

Hipóteses levantadas:

- Firewall (Input Chain)
- Interface List LAN
- IP Services (Available From)

Será investigado nas próximas etapas.

---

# Aprendizados

Durante esta implementação foram compreendidos os seguintes conceitos:

- Bridge
- Bridge VLAN Filtering
- Interface VLAN
- Tagged
- Untagged
- PVID
- VLAN ID
- Interface lógica
- Interface física
- DHCP em VLAN
- Gateway em VLAN
- Tráfego Layer 2
- Tráfego Layer 3

---

# Próximos passos

- Resolver acesso ao MikroTik via VLAN.
- Migrar a rede ADMIN para VLAN10 (mantendo ether2 como contingência durante os testes).
- Implementar VLAN40.
- Criar regras de firewall entre VLANs.
- Adicionar um switch gerenciável.
- Configurar portas Trunk.
- Configurar portas Access.
- Implementar WireGuard.
- Criar rede de servidores.
- Monitoramento com Grafana.

---

# Tecnologias utilizadas

- MikroTik RouterOS
- VLAN (IEEE 802.1Q)
- Bridge VLAN Filtering
- DHCP
- NAT
- PPPoE
- Firewall
- Interface Lists
- WinBox
- Linux (CachyOS)

---

# Conclusão

A implementação das VLANs demonstrou como uma única infraestrutura física pode transportar múltiplas redes independentes.

Mesmo utilizando um Access Point sem suporte a VLAN, foi possível compreender os principais conceitos de segmentação de redes no MikroTik, preparando o laboratório para futuras expansões com switches gerenciáveis e equipamentos corporativos.