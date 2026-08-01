# Firewall

Após a implementação da segmentação por VLANs, foi criado um conjunto de regras de firewall seguindo o princípio de **Default Deny**, permitindo apenas o tráfego explicitamente autorizado.

---

# Objetivos

- Proteger o MikroTik contra acessos não autorizados.
- Restringir a comunicação entre VLANs.
- Reduzir a superfície de ataque.
- Implementar boas práticas de segurança recomendadas pela MikroTik.

---

# Estrutura das regras

As regras foram organizadas de forma lógica para reduzir o processamento do firewall.

```
1. Accept Established/Related

2. Drop Invalid

3. Drop Blacklist

4. Accept Administração

5. Port Knocking

6. Proteção contra Brute Force

7. ICMP limitado

8. Regras entre VLANs

9. Drop Final
```

---

# Accept Established / Related

Permite conexões já estabelecidas e conexões relacionadas.

Exemplos:

- Navegação Web
- Respostas DNS
- Respostas de conexões iniciadas anteriormente

Essa regra evita que todo pacote seja analisado pelas demais regras do firewall.

---

# Drop Invalid

Descarta imediatamente pacotes considerados inválidos pelo Connection Tracking.

Exemplos:

- Pacotes corrompidos.
- Pacotes fora de estado.
- Conexões inconsistentes.
- Alguns tipos de ataques.

Essa regra reduz processamento e melhora a segurança.

---

# Política Default Deny

Foi adotada uma política de negação por padrão.

Todo tráfego destinado ao MikroTik é descartado caso não exista uma regra permitindo explicitamente.

```
Chain Input

↓

Drop
```

---

# Acesso Administrativo

O gerenciamento do MikroTik é permitido apenas através da rede administrativa.

Rede autorizada:

```
192.168.10.0/24
```

As demais VLANs não possuem acesso direto ao equipamento.

---

# Proteção ICMP

Foi criada uma regra permitindo ICMP com limitação de taxa.

Objetivos:

- Evitar Ping Flood.
- Reduzir consumo de CPU.
- Permitir diagnóstico da rede.

Limite configurado:

```
50 pacotes por segundo
```

---

# Port Knocking

O gerenciamento remoto foi protegido utilizando Port Knocking.

Funcionamento:

1. Um host realiza conexão para uma porta previamente definida.

2. O endereço IP é adicionado automaticamente a uma Address List.

3. O endereço permanece autorizado durante 5 horas.

4. Apenas IPs presentes nessa lista conseguem acessar os serviços administrativos.

Fluxo:

```
Internet

↓

Porta X

↓

Address List

↓

5 horas

↓

WinBox
SSH
WebFig
```

Esse mecanismo reduz significativamente a exposição dos serviços administrativos.

---

# Proteção contra Brute Force

Foi implementado um sistema de bloqueio automático para tentativas repetidas de autenticação.

Funcionamento:

```
Tentativa 1

↓

stage1

↓

Tentativa 2

↓

stage2

↓

Tentativa 3

↓

stage3

↓

Tentativa 4

↓

Blacklist
```

Após múltiplas tentativas consecutivas, o endereço IP é adicionado automaticamente à Blacklist, impedindo novas conexões durante o período configurado.

---

# Address Lists

As Address Lists passaram a ser utilizadas para:

- Administração.
- Port Knocking.
- Blacklist.
- Controle de acesso.

Essa abordagem facilita a manutenção das regras de firewall.

---

# Controle entre VLANs

Foi implementada comunicação unidirecional entre as redes.

Política utilizada:

```
ADMIN

↓

LAB

Permitido
```

```
LAB

↓

ADMIN

Bloqueado
```

Isso permite que administradores gerenciem dispositivos da VLAN de laboratório sem permitir o caminho inverso.

Essa política poderá ser expandida futuramente para as demais VLANs.

---

# Ordem das regras

A ordem do firewall foi planejada para minimizar processamento.

Primeiro são aceitos pacotes válidos já estabelecidos.

Depois pacotes inválidos são descartados.

Em seguida são aplicadas as regras específicas de gerenciamento e segurança.

Por último, todo tráfego que não corresponda a nenhuma regra é descartado.

---

# Boas práticas implementadas

- Default Deny.
- Connection Tracking.
- Drop Invalid.
- Rate Limit de ICMP.
- Port Knocking.
- Proteção contra Brute Force.
- Segmentação entre VLANs.
- Controle de gerenciamento.
- Utilização de Address Lists.
- Firewall organizado por prioridade.

---

# Resultado

A configuração resultou em um ambiente significativamente mais seguro, reduzindo a exposição do roteador à Internet e permitindo apenas o tráfego estritamente necessário.

Além da segmentação por VLANs, foram implementadas camadas adicionais de proteção que simulam práticas utilizadas em ambientes corporativos.