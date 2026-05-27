

# Painel de Monitoramento Centralizado — FortiGate 40F (PRODEPA / CLARO)

Este repositório armazena a documentação, estrutura de métricas e guias operacionais do painel (dashboard) de monitoramento em tempo real do firewall corporativo **FortiGate 40F**, configurado para gerenciar a redundância e o tráfego dos links de internet da **PRODEPA** (incluindo o link de trânsito da operadora **Claro**).

---

## 📊 Estrutura e Componentes do Painel

O dashboard foi projetado para fornecer visibilidade imediata sobre a saúde do hardware, o comportamento do tráfego de dados e a estabilidade dos links de comunicação (Multi-WAN / SD-WAN). Ele está dividido nos seguintes blocos visuais:

### 1. Painel Frontal e Recursos de Hardware (Topo)

* **Mapeamento de Portas Físicas:** Representação gráfica interativa do chassi do FortiGate 40F. Permite checar visualmente e em tempo real o status de conexão das portas físicas (USB, Console, WAN, LAN e interfaces de rede `A`, `B`, `1`, `2`, `3`).
* **System Uptime:** Indicador do tempo contínuo de atividade do dispositivo (registrando estabilidade de longa duração, ultrapassando 236 dias de operação contínua).
* **CPU Load:** Gráfico de medidor (*gauge*) indicando o percentual de uso instantâneo do processador (operando na faixa segura de 7.0%).

### 2. Throughput (Vazão de Dados - Links PRODEPA & CLARO)

* Gráficos de linhas históricos que monitoram o volume de tráfego (em bits por segundo) trafegados pelas principais interfaces da unidade, cobrindo o consumo de banda de cada provedor:
* `Interface VPN_SDWAN01` (Orquestração de caminhos dinâmicos)
* `Interface VLAN_24 (WAN_PRODEPA)`
* Interfaces dedicadas ao link de operadora (**Claro**)
* `Interface lan0` (Tráfego da rede local)



### 3. Health-Check & Latência (Qualidade do Link)

* **Health-Check:** Análise percentual histórica de perda de pacotes (*packet loss*) nas rotas críticas, nos links da **Claro**, da **PRODEPA** e nos túneis de VPN.
* **Latência da Unidade (ICMP):** Monitoramento de tempo de resposta em milissegundos ($ms$) utilizando pacotes ICMP (Ping), essencial para identificar gargalos, jitter ou degradação de sinal diretamente em cada operadora.

### 4. Status Operacional & Consumo de Memória (Rodapé)

* **Status Operacional | PRODEPA & CLARO:** Painel integrado de alarmes e incidentes (Zabbix Triggers). Exibe colunas como *Host, Severity, Status, Problem, Ack* e *Time*. No momento da captura, exibe o estado ideal: `"No problems found"` (Nenhum problema encontrado).
* **Monitoramento de Memória - FW-CECOMT:** Gráfico de linha que acompanha a evolução do consumo da memória RAM do equipamento, ajudando a prever estouros de capacidade por tabelas de roteamento ou sessões NAT excessivas.

---

## 🛠️ Tecnologias e Protocolos Utilizados

* **Coleta de Dados:** Protocolo **SNMPv2c / SNMPv3** (Simple Network Management Protocol) e monitoramento ativo via **ICMP Ping**.
* **Orquestração de Eventos:** **Zabbix** (Triggers de severidade, gerenciamento de alarmes e inventário).
* **Visualização Avançada:** **Grafana** integrado ao Zabbix ou painel nativo customizado com plugin de *Image Mapping* para renderização do chassi do hardware.

---

## 🚀 Guia de Operação e Resposta a Incidentes (Cenário Multi-WAN)

Para a equipe de monitoramento do NOC / Redes, este painel deve ser interpretado utilizando os seguintes limiares de atenção (*thresholds*):

| Indicador | Estado Normal | Atenção (Warning) | Crítico (Disaster) | Ação Recomendada |
| --- | --- | --- | --- | --- |
| **CPU Load** | $< 70\%$ | $70\% - 85\%$ | $> 85\%$ | Verificar processos de inspeção profunda de pacotes (Deep SSL) ou possíveis ataques de negação de serviço (DoS). |
| **Latência (ICMP)** | $< 50ms$ | $50ms - 100ms$ | $> 100ms$ | Verificar saturação de link ou problemas de roteamento externo na operadora correspondente (**Claro** ou **PRODEPA**). |
| **Health-Check** | $0\%$ Perda | $1\% - 5\%$ Perda | $> 5\%$ Perda | Instabilidade no link. O SD-WAN deve priorizar rotas alternativas e fazer o *failover* automaticamente. |
| **Status de Porta** | Verde (Acesa) | - | Vermelha / Apagada | Verificar se o cabo de rede físico foi desconectado ou se o conversor de mídia/modem da operadora caiu. |

---

## 📝 Notas de Identificação do Ativo

* **Nome do Host em Monitoramento:** `FW-FORTIGATE-DE-200F` / `FW-CECOMT`
* **Localidade/Alocação:** Infraestrutura Interna / Gateway de Borda da Rede.

<img width="1894" height="1003" alt="image" src="https://github.com/user-attachments/assets/5fe571e2-020a-413d-976c-e329cc7bdcb3" />
