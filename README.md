# Sistema de Controle de Tráfego com Prioridade para Ônibus

## Descrição (Contexto)  
Este projeto foi desenvolvido como parte da disciplina de Sistemas a Eventos Discretos, com o objetivo de modelar e analisar um sistema de controle de tráfego urbano utilizando Redes de Petri Coloridas Hierárquicas (HCPN) na ferramenta CPN Tools.  

O foco do modelo é a simulação do funcionamento de uma interseção urbana que contempla:  
- Veículos comuns (carros)  
- Transporte público (ônibus)  
- Pedestres  

Um diferencial do sistema é a prioridade para ônibus, representada por detectores que ajustam dinamicamente o ciclo semafórico, estendendo o tempo de verde quando necessário.  

---

## Modelo do Sistema  
O sistema foi dividido em páginas hierárquicas para modularizar o comportamento:  

- Interseccao → Página principal, integra os módulos de filas, controle de semáforos e detectores  
- Fila_Veiculos → Modela a chegada e armazenamento de veículos (carros e ônibus)  
- Controle_semaforos → Define os estados dos semáforos (Verde, Amarelo, Vermelho) e sua temporização  
- Detectores de prioridade → Sensores que verificam a presença de ônibus nas filas e ajustam a lógica do ciclo  

Cada módulo é conectado por transições de substituição (substitution transitions), garantindo modularidade e clareza.  
![WhatsApp Image 2025-09-26 at 03 14 16](https://github.com/user-attachments/assets/c613eae0-eb2e-4a41-8d44-a8b5536da1e2)

---

## Função no Sistema  
Fluxo implementado no modelo:  

1. Chegada de veículos → Carros e ônibus gerados em intervalos distintos  
2. Gerenciamento de filas → Transições liberam veículos de acordo com o estado dos semáforos  
3. Controle semafórico:  
   - Ciclo padrão: Verde → Amarelo → Vermelho  
   - Verde: 30s, Amarelo: 5s  
4. Prioridade para ônibus:  
   - Se detectado, o verde é estendido em +15s  
   - Detector é resetado após passagem do ônibus  
5. Pedestres → Travessia liberada apenas quando veículos estão parados  

---

## Condições Finais do Projeto  
- Modelo construído e simulado com sucesso no CPN Tools  
- Filas esvaziam corretamente, respeitando lógica dos semáforos  
- Prioridade para ônibus implementada com detector  
- Semáforos de veículos e pedestres integrados  
- Estrutura hierárquica com:  
  - Port places bem definidos  
  - Bind Port–Sockets correto  
  - Modularização clara  

---

## Considerações  
- O modelo mostra como HCPNs representam de forma eficiente sistemas a eventos discretos complexos  
- A hierarquia simplificou o desenvolvimento ao separar: controle de semáforos, filas e detectores  
- A lógica de prioridade para transporte público reflete cenários reais de mobilidade urbana  
- Possíveis melhorias futuras:  
  - Modelar chegadas com distribuições probabilísticas (Poisson, exponencial)  
  - Coletar métricas (tempo médio de espera, throughput)  
  - Expandir para múltiplas faixas de tráfego  
