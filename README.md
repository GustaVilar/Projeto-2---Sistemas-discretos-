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
- Controle_semaforos → Define os estados dos semáforos (Verde, Amarelo, Vermelho) e sua temporização, incluindo sincronização com sinais de pedestres  
- Detectores de prioridade → Sensores que verificam a presença de ônibus nas filas e ajustam a lógica do ciclo  

Cada módulo é conectado por transições de substituição (substitution transitions), garantindo modularidade e clareza.  

### Construção da Página Mãe (Interseccao)  
A página Interseccao é a visão geral do sistema.  
- Nela estão os lugares que representam as filas, os semáforos de veículos e pedestres e os detectores de prioridade.  
- Esses lugares são ligados às subpáginas através de portas de entrada e saída (Port Places) configuradas como I/O.  
- A Interseccao conecta:  
  - As filas de veículos à subpágina Fila_Veiculos.  
  - Os estados dos semáforos à subpágina Controle_semaforos.  
  - Os detectores à subpágina de prioridade.  

Dessa forma, a página mãe não implementa a lógica em si, mas organiza a comunicação entre os módulos.  

### Construção das Páginas Filhas  
- **Fila_Veiculos**: contém os lugares para carros e ônibus em cada direção. Tokens coloridos representam os tipos de veículos, e transições temporizadas simulam chegadas (por exemplo, carro a cada 6 unidades de tempo, ônibus a cada 20).  
- **Controle_semaforos**: utiliza lugares para armazenar o estado atual dos sinais (Verde, Amarelo, Vermelho). As transições temporizadas modelam a mudança de estado e guardas garantem que duas vias conflitantes nunca fiquem verdes ao mesmo tempo. Os sinais de pedestres são sincronizados: ficam verdes quando os veículos da mesma direção estão vermelhos.  
- **Detector de prioridade**: monitora as filas de ônibus. Se um ônibus é detectado, coloca o lugar Detector em True. Esse valor é lido em Controle_semaforos, onde a transição de Verde para Amarelo passa a considerar um atraso extra de 15 unidades de tempo. Após a passagem do ônibus, o detector volta para False.  

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

---

## Demonstração em Vídeo  
[![Assista no YouTube](https://img.youtube.com/vi/KyRkV3Nw9oQ/0.jpg)](https://www.youtube.com/watch?v=KyRkV3Nw9oQ)
