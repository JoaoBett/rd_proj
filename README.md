# Projeto de Rede de Dados com Deteção de Ameaças IoT

## Descrição do Projeto

Este repositório contém o ficheiro packet tracer, tal como, os seus backups, o ficheiro python e a documentação relativa ao projeto da unidade curricular de **Redes de Dados** (2024/2025). O objetivo foi planear, implementar e testar uma infraestrutura de rede segura e resiliente para uma empresa de cibersegurança.

O projeto está dividido em duas componentes principais:
1.  A **Infraestrutura de Rede**, implementada no Cisco Packet Tracer, que inclui uma arquitetura multi-site com serviços essenciais como DHCP, NAT, OSPF e uma malha completa de VPNs IPSec.
2.  A **Simulação de Cibersegurança**, que utiliza um programa em Python e uma Rede Neuronal Artificial (ANN) para diferenciar e detetar tráfego malicioso de tráfego benigno, com base no dataset IoT-23.

---

## 🎯 Objetivos Principais

* **Primeira Parte:** 
* **Topologia** para uma Sede e duas Filiais (F1 e F2), com segmentação de rede através de VLANs para os departamentos de Vendas, RH e Administração.
* **Planear e implementar endereçamento IP** utilizando VLSM a partir do bloco `172.16.0.0/21`.
* **Configurar encaminhamento dinâmico** com OSPF para garantir conectividade total entre todos os sites.
* **Implementar serviços de rede**, incluindo DHCP para atribuição automática de IPs e NAT Overload (PAT) para acesso à Internet através de um único IP público.
* **Assegurar a comunicação inter-site** através de uma malha completa de túneis VPN IPSec.

* **Segunda Parte:** 
* **Simular a deteção de ciberataques IoT** utilizando uma Rede Neuronal Artificial treinada com dados do dataset IoT-23.

---

## 🌐 Topologia da Rede

A rede foi planeada com três principais divisões (Sede, Filial 1, Filial 2) interligados numa topologia WAN em malha completa (full mesh) para garantir redundância e disponibilidade. O acesso à Internet e aos serviços públicos é centralizado no router da Sede, assim, este iria ser o que faria o papel de tradução dos IPs privados para um unico IP público.

---

## ⚙️ Componentes e Configurações de Rede

### VLANs e Inter-VLANs
* Cada site foi divido em 3 VLANs: Vendas (64PCs), RH (20PCs) e Administração (5PCs).
* Foi utilizada a técnica de **Router-on-a-Stick**, com sub-interfaces nos routers e encapsulamento `802.1Q` para permitir a comunicação entre as diferentes VLANs dentro de cada site.

### Routing Dinâmico (OSPF)
* Os protocolos permitidos para este projeto eram **OSPF** ou **EIGPR**, no qual foi decidido utilizar o **OSPF** por prefrência de comodidade.
* O protocolo **OSPF** foi configurado em todos os routers numa arquitetura de área única (`area 0`).
* As redes LAN e WAN de cada router foram anunciadas no processo OSPF.
* Foram usadas `passive-interfaces` nas sub-interfaces LAN para otimizar o envio de atualizações de routing.

### NAT (PAT)
* Foi configurado **NAT Overload (PAT)** no router da Sede (R-Sede) para traduzir todos os endereços IP privados da rede (`172.16.0.0/21`) para um único endereço IP "público" na interface de saída.
* Foi utilizada uma Access Control List (ACL) para definir o tráfego de origem a ser traduzido.

### VPN IPSec Site-to-Site
* Para proteger o tráfego WAN, foram configurados **três túneis VPN IPSec** em malha completa, seguindo as diretrizes do `Lab - VPN .pdf`, disponibilizado como folha de apoio para o projeto.
* A configuração utilizou **Fase 1** (com encriptação AES, hash sha e autenticação por chave pré-partilhada) e **Fase 2** (com `transform-set` usando ESP-3DES e ESP-SHA-HMAC).
* Foram criadas ACLs específicas para cada túnel, definindo o tráfego entre as LANs de cada par de sites como "tráfego interessante" a ser encriptado.

### Políticas de Acesso (ACLs)
* Foi implementada uma política de acesso para aumentar a segurança das VLANs de Administração. Esta política, configurada via Extended ACL, impede que as VLANs de Vendas e RH  pingem (`ping`) para as VLANs de Administração, enquanto permite acesso total ao Servidor de Ficheiros central, que reside na VLAN Admin da Sede.

---

## 🧠 Simulação de Deteção de Ameaças (IA/Python)

*Esta secção do projeto foi desenvolvida pelo Guilherme Teixeira, devido ao poder computacional superior comparado ao de João Bettencourt.*

O trabalho consistiu em reutilizar um script em Python (`ANN IoT23 v12 para estudantes.ipynb` ) para treinar um modelo de Rede Neuronal Artificial (ANN) capaz de distinguir entre tráfego de rede IoT benigno e malicioso. Foram preparados dois datasets customizados a partir de 4 tipos de ataques diferentes do dataset IoT-23 (), mantendo um equilíbrio de 50/50 entre amostras benignas e malignas.

O modelo final foi treinado consecutivamente com ambos os datasets, e foi realizada uma pesquisa sobre a sua potencial aplicação como um Sistema de Deteção de Intrusão (IDS) e sobre os ataques estudados.

---

## 🚀 Como Utilizar os Ficheiros

### Rede no Packet Tracer
1.  É necessário ter o **Cisco Packet Tracer** (versão 8.0 ou superior recomendada) instalado.
2.  Descarrega o ficheiro `.pkt` do projeto.
3.  Abre o ficheiro no Packet Tracer, testa a conectividade e analisa as configurações dos dispositivos.

## 🧠 Simulação de Deteção de Ameaças (IA/Python)

*Esta secção do projeto foi desenvolvida pelo Guilherme Teixeira, devido ao poder computacional superior comparado ao de João Bettencourt.*

O trabalho consistiu em reutilizar e adaptar um script em Python (`ANN IoT23 v12 para estudantes.ipynb` ) para treinar um modelo de Rede Neuronal Artificial (ANN) capaz de distinguir entre tráfego de rede benigno e malicioso. Foram preparados dois datasets personalizados a partir de 4 tipos de ataques diferentes do dataset IoT-23 (Mirai, HideNSeek, IRCBot, Hajime), mantendo um equilíbrio de 50/50 entre amostras benignas e malignas.

O modelo final foi treinado consecutivamente (15) com ambos os datasets, e foi realizada uma pesquisa sobre a sua potencial aplicação como um Sistema de Deteção de Intrusão (IDS) e sobre os ataques estudados.

---

## 👥 Autores

* **João Bettencourt** 
* **Guilherme Teixeira**
