# Projeto de Rede Empresarial — Cisco Packet Tracer

> Simulação de uma rede empresarial completa desenvolvida após a conclusão do curso **Networking Basics** da Cisco Networking Academy.

---

## Descrição do Projeto

Este projeto simula uma rede empresarial real composta por três localizações físicas distintas:

- **Prédio (sede)** — 3 andares com VLANs separadas
- **Server Room** — servidores centralizados com IPs estáticos
- **Casa do Dono** — rede residencial com escalabilidade para visitas

As três localizações estão interligadas via routers ISR4331 através de um router central de Internet, usando **Static Routing**, **NAT Overloading** e **RIPv2**.

---

##  Topologia da Rede


![Imagem da rede](rede.png)

**! Trocar esta imagem quando acabar de configurar tudo !**


---

##  Equipamentos

| Localização | Equipamento | Modelo | Qtd |
|---|---|---|---|
| Prédio — Andares | Switch | Cisco 2960-24TT | 4 |
| Prédio — Core | Router | ISR4331 | 1 |
| Server Room | Switch | Cisco 2960-24TT | 1 |
| Server Room | Router | ISR4331 | 1 |
| Casa do Dono | Switch | Cisco 2960-24TT | 1 |
| Casa do Dono | Router | ISR4331 | 1 |
| Internet | Router | ISR4331 | 1 |

> O router ISR4331 foi escolhido por suportar sub-interfaces para inter-VLAN routing (router-on-a-stick), comandos IOS completos e static routing — modelo padrão em laboratórios CCNA.

---

##  Endereçamento IP — VLSM

O VLSM foi aplicado alocando subnets **do maior para o menor número de hosts**, sem desperdício de endereços.

### Prédio — 192.168.1.0/24

| Ordem | VLAN | Hosts req. | Network | Máscara | Gateway | Broadcast | Hosts úteis |
|---|---|---|---|---|---|---|---|
| 1.º | VLAN 20 — IT | 64 | 192.168.1.0/25 | 255.255.255.128 | 192.168.1.1 | 192.168.1.127 | 126 |
| 2.º | VLAN 10 — RH | 32 | 192.168.1.128/26 | 255.255.255.192 | 192.168.1.129 | 192.168.1.191 | 62 |
| 3.º | VLAN 30 — Chefe | 5 | 192.168.1.192/29 | 255.255.255.248 | 192.168.1.193 | 192.168.1.199 | 6 |
| 4.º | VLAN 40 — VoIP | 4 | 192.168.1.200/29 | 255.255.255.248 | 192.168.1.201 | 192.168.1.207 | 6 |

### Server Room — 192.168.2.0/24

| Servidor | IP Estático | Gateway | Broadcast |
|---|---|---|---|
| DHCP Server | 192.168.2.2 | 192.168.2.1 | 192.168.2.15 |
| DNS Server | 192.168.2.3 | 192.168.2.1 | 192.168.2.15 |
| NTP Server | 192.168.2.4 | 192.168.2.1 | 192.168.2.15 |
| Web Server | 192.168.2.5 | 192.168.2.1 | 192.168.2.15 |
| Email Server | 192.168.2.6 | 192.168.2.1 | 192.168.2.15 |
| FTP Server | 192.168.2.7 | 192.168.2.1 | 192.168.2.15 |

### Casa do Dono — 192.168.3.0/24

| Network | Hosts req. | Máscara | Gateway | Broadcast | Hosts úteis |
|---|---|---|---|---|---|
| 192.168.3.0/27 | 30 (com visitas) | 255.255.255.224 | 192.168.3.1 | 192.168.3.31 | 30 |

### Ligações WAN — Point-to-Point /30

| Ligação | Network | IP lado A | IP lado B | Broadcast |
|---|---|---|---|---|
| R-Predio ↔ R-Internet | 10.0.0.0/30 | 10.0.0.1 | 10.0.0.2 | 10.0.0.3 |
| R-Datacenter ↔ R-Internet | 10.0.0.4/30 | 10.0.0.5 | 10.0.0.6 | 10.0.0.7 |
| R-Casa ↔ R-Internet | 10.0.0.8/30 | 10.0.0.9 | 10.0.0.10 | 10.0.0.11 |

> As ligações WAN usam /30 porque numa ligação point-to-point apenas são necessários 2 hosts (os dois routers), evitando desperdício de endereços.

---

##  VLANs e Configuração dos Switches

Todos os switches foram configurados com:

- **Banner MOTD** com aviso de acesso restrito
- **Password enable secret** para acesso ao modo privilegiado
- **Password nas linhas console e VTY**
- **Portas não utilizadas em shutdown** como medida de segurança
- **Falta adicionar lockout quando se insere palavra passe errada**

---

## Porquê não usar Spanning Tree nem LACP

**Spanning Tree (STP)**
O Cisco 2960 ativa o STP automaticamente — não requer configuração manual. A topologia é em cascata linear (Switch2 → Switch0 → Switch1 → Switch5/6 → R-Predio) sem loops físicos, pelo que o STP manual não é necessário.

**LACP (Link Aggregation)**
O LACP agrega múltiplos cabos entre dois switches para redundância e maior largura de banda. Na topologia atual existe apenas um cabo entre cada par de switches, pelo que não se aplica.

---

##  TODO

- [ ] LockOut de 1 min apos inserir password errada 3x 
- [ ] Configuração do R-Predio (router-on-a-stick, sub-interfaces)
- [ ] Configuração do R-Datacenter e R-Casa
- [ ] Configuração do R-Internet
- [ ] NAT Overloading (PAT)
- [ ] RIPv2
- [ ] Configuração dos servidores (DHCP, DNS, NTP, Web, Email, FTP)
- [ ] Testes de conectividade end-to-end (ping e tracert)

---

##  Certificação

Este projeto foi desenvolvido após a conclusão do curso **Networking Basics** da Cisco Networking Academy.

![Cisco Networking Academy](https://www.netacad.com/favicon.ico)

---

*Lucas da Silva — Junho 2026*
