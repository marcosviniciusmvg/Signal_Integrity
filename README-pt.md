# Integridade de Sinais - Cupons de PCB 50 Ohm (2L e 4L)

[🇺🇸 English](./README.md) | [🇧🇷 Português](./README-pt.md)

## Visão geral
Este repositório reúne material de projeto e validação de cupons de PCB de 50 Ohm, com foco em integridade de sinais para aplicações de RF e high speed.

O projeto compara o comportamento de linhas em placas de 2 e 4 camadas, com ênfase em:
- controle de impedância;
- qualidade de transmissão;
- sensibilidade a descontinuidades de layout.

## Por que este projeto existe
Em muitos desenvolvimentos práticos de PCBs de RF/high speed, simulação sozinha não captura toda a variabilidade de fabricação e montagem. Tolerâncias de fabricação, variação do dielétrico, soldagem, transições e qualidade do caminho de retorno podem alterar significativamente a impedância e as perdas medidas.

Este projeto foi criado para medir esses efeitos de forma controlada e extrair diretrizes de layout baseadas em dados reais.

## Escopo técnico
- Placas:
  - 2L (FR-4 padrão)
  - 4L (stackup JLC04161H-3313)
- Estruturas em teste:
  - Microstrip 50 Ohm:
    - Conceito básico: trilha de sinal em camada externa referenciada a um plano de GND abaixo.
    - Vantagens: roteamento simples, menor uso de cobre, implementação direta e ampla adoção em placas mistas e RF.
    - Desvantagens: maior exposição de campo ao ar/ambiente, maior sensibilidade a descontinuidades próximas e, em geral, menor confinamento que CPWG.
    - Aplicações típicas: interconexões RF gerais, linhas high speed moderadas e projetos orientados a custo.
  - CPWG (Coplanar Waveguide with Ground) 50 Ohm:
    - Conceito básico: trilha de sinal em camada externa com GND coplanar nas laterais e plano de referência inferior.
    - Na prática (principalmente em placas 2 camadas), essa estrutura frequentemente permite atingir 50 Ohm com trilhas mais finas que no microstrip simples, dependendo do stackup e dos limites de fabricação.
    - Vantagens: melhor confinamento eletromagnético, melhor controle da corrente de retorno próxima à trilha e, em muitos casos, melhor comportamento em transições/conectores.
    - Desvantagens: exige controle mais rígido de layout (geometria trilha-gap), pode ser mais sensível a tolerâncias de fabricação em gaps estreitos e pode consumir mais área de roteamento.
    - Aplicações típicas: caminhos RF que exigem maior controle de campo, launches/conectores e canais high speed com forte exigência de controle de descontinuidades.
- Cenários comparados:
  - Baseline
  - Vias
  - Vias + return path
  - Descontinuidade de GND (slot)
  - Variante com matching (`matching` = rede/geometria de ajuste de impedância usada para reduzir reflexões e melhorar transferência de potência; isso é especialmente importante em cadeias de RF e links high speed sensíveis a reflexão para preservar integridade de sinal, reduzir return loss e estabilizar a resposta em frequência)
- Instrumentação:
  - NanoVNA-F V2
  - NanoVNA Saver

### Impacto do stackup neste contexto
- Impedância característica é um parâmetro dependente do stackup, não apenas da largura da trilha.
- Para o mesmo alvo (50 Ohm), a geometria necessária muda com:
  - espessura dielétrica entre sinal e plano de referência;
  - constante dielétrica efetiva (`Er`);
  - espessura de cobre e tolerâncias de fabricação;
  - máscara de solda e geometria de GND ao redor.
- Maior controle de stackup tende a melhorar a repetibilidade de `S11/S21` entre lotes de fabricação.

### Por que microstrip é mais difícil em placas 2 camadas
- Em placas 2 camadas, obter 50 Ohm em microstrip geralmente exige trilhas relativamente largas, pois a distância ao plano de referência é maior.
- Trilhas mais largas geram limitações práticas:
  - maior consumo de área de roteamento;
  - compatibilidade limitada com os pitches de pinos dos encapsulamentos modernos, onde trilhas muito largas frequentemente ficam difíceis de conectar diretamente em pads/escapes sem transições de estreitamento e descontinuidades associadas;
  - maior acoplamento com descontinuidades próximas e conectores;
  - maior sensibilidade a variações locais de stackup e montagem.
- Em muitos processos 2L de baixo custo, as tolerâncias de dielétrico e cobre são menos rígidas do que em stackups 4L controlados, aumentando a dispersão de impedância.
- CPWG é frequentemente usado em 2L como estratégia de mitigação, porque os GNDs coplanares melhoram confinamento de campo e controle da corrente de retorno em relação ao microstrip simples.

## Detalhes do projeto das placas

### Placa 2 camadas (`Signal_Integrity_2L_Simplified`)
Características principais:
- Implementação de 2 camadas orientada a custo em FR-4 padrão, usada para quantificar o impacto de menor controle de stackup no roteamento de 50 Ohm.
- Mantém a mesma filosofia de medição da placa de 4 camadas para comparação direta:
  - estruturas baseline;
  - cenários com vias e vias + return path;
  - cenário com descontinuidade de GND;
  - variante com matching.
- Útil para avaliar trade-offs entre manufaturabilidade/custo e estabilidade de desempenho RF/high speed.

Variações de cupons e objetivos:
- `2L_01 - Microstrip 50 Ohm (W2700)`: referência baseline do comportamento de microstrip em 2 camadas.
- `2L_02 - CPWG 50 Ohm baseline (W800/G200)`: referência baseline de CPWG em 2 camadas.
- `2L_03 - CPWG 50 Ohm (W380/G120)`: variante geométrica de CPWG para comparar confinamento e sensibilidade de processo versus G200.
- `2L_04 - CPWG 50 Ohm + matching (W800/G200)`: avalia o impacto da estratégia de matching em relação ao CPWG baseline.
- `2L_05 - CPWG 50 Ohm + vias`: quantifica descontinuidade de transição e reflexão/ripple adicionais.
- `2L_06 - CPWG 50 Ohm + vias + return path`: verifica melhoria ao reforçar continuidade de corrente de retorno nas transições.
- `2L_07 - CPWG 50 Ohm + descontinuidade de GND (slot)`: mede degradação causada por interrupção intencional do caminho de retorno.

#### Esquemático
<img src="./images/sch_2l.png" alt="Esquemático 2 camadas" width="700">

#### Stackup
<img src="./images/stackup_2l.png" alt="Stackup 2 camadas" width="700">

#### Vistas de layout PCB
![2 camadas visão geral](./images/pcb_2d_2l.png)

![2 camadas top](./images/pcb_2d_top_2l.png)

![2 camadas bottom](./images/pcb_2d_bot_2l.png)

#### Vista 3D
![2 camadas 3D top](./images/pcb_3d_top_2l.png)

### Placa 4 camadas (`Signal_Integrity_4L_Simplified`)
Características principais:
- Ambiente de impedância controlada com stackup de 4 camadas (`JLC04161H-3313`), melhorando continuidade do caminho de retorno e confinamento de campo.
- Inclui variantes de cupons de 50 Ohm para isolar efeitos de layout:
  - referências baseline de microstrip e CPWG;
  - caso com transição em via e caso com via + melhoria de return path;
  - descontinuidade intencional no plano de GND (slot) para medir degradação;
  - variante com matching para comparação com o comportamento baseline.
- Funciona como plataforma de referência de maior controle para medições de integridade de sinal RF/high speed.

Variações de cupons e objetivos:
- `4L_01 - Microstrip 50 Ohm baseline (W350)`: referência baseline de microstrip com impedância controlada em 4 camadas.
- `4L_02 - CPWG 50 Ohm baseline (W285/G200)`: referência baseline de CPWG em 4 camadas.
- `4L_03 - CPWG 50 Ohm (W210/G120)`: variante geométrica de CPWG para comparar confinamento de campo e sensibilidade de tolerância versus G200.
- `4L_04 - CPWG 50 Ohm + matching (W285/G200)`: avalia o impacto da estratégia de matching em relação ao CPWG baseline.
- `4L_05 - Microstrip 50 Ohm + vias`: quantifica o impacto da descontinuidade de transição por via.
- `4L_06 - Microstrip 50 Ohm + vias + return path`: verifica redução de reflexão/ripple com melhoria do caminho de retorno próximo das transições.
- `4L_07 - Microstrip 50 Ohm + descontinuidade de GND (slot no L2)`: mede sensibilidade à interrupção intencional do plano de referência interno.

#### Esquemático
<img src="./images/sch_4l.png" alt="Esquemático 4 camadas" width="700">

#### Stackup
<img src="./images/stackup_4l.png" alt="Stackup 4 camadas" width="700">

#### Vistas de layout PCB
![4 camadas top](./images/pcb_2d_top_4l.png)

![4 camadas camada interna 2](./images/pcb_2d_ly2_4l.png)

![4 camadas camada interna 3](./images/pcb_2d_ly3_4l.png)

![4 camadas bottom](./images/pcb_2d_bot_4l.png)

#### Vista 3D
![4 camadas 3D top](./images/pcb_3d_top_4l.png)

## NanoVNA-F V2 neste projeto
O NanoVNA-F V2 é usado como analisador vetorial de redes compacto para caracterizar reflexão e transmissão nos cupons de PCB.

- Princípio de funcionamento:
  - Executa medição varrida em `CW` (um tom senoidal por ponto de frequência).
  - Usa amostragem direcional e detecção coerente `I/Q` para separar ondas incidente, refletida e transmitida.
  - Calcula parâmetros de espalhamento por medidas de razão (`S11` e `S21`).
- Funções utilizadas neste ensaio:
  - Calibração `SOLT 2-port` na ponta dos cabos (plano de referência nos conectores SMA).
  - `S11 LogMag` para análise de retorno / descasamento.
  - `S21 LogMag` para análise de perda de inserção e ripple/notch.
  - Varredura de frequência na banda selecionada (tipicamente 1-3 GHz) com 401 ou 801 pontos.
  - Exportação `.s2p` no NanoVNA Saver para documentação e pós-processamento opcional em domínio do tempo.

## Estrutura do repositório
- `Signal_Integrity_2L_Simplified/`: arquivos de projeto da placa de 2 camadas.
- `Signal_Integrity_4L_Simplified/`: arquivos de projeto da placa de 4 camadas.

## Links dos projetos
| Projeto | Arquivos no GitHub | Altium 365 |
|---|---|---|
| 2 Camadas | [Signal_Integrity_2L_Simplified](./Signal_Integrity_2L_Simplified/) | [Abrir no Altium 365](https://365.altium.com/files/987D5E5E-FB93-4543-97C9-5E7F92589402) |
| 4 Camadas | [Signal_Integrity_4L_Simplified](./Signal_Integrity_4L_Simplified/) | [Abrir no Altium 365](https://365.altium.com/files/C1313425-98CB-430A-858C-CD9CB5CA370C) |

## Testes (Roteiro e Relatório)
Para execução completa dos testes, ordem de medição, critérios de avaliação e tabela de resultados, consulte:

- [README-tests.md](./README-tests.md) (English)
- [README-testes.md](./README-testes.md) (Português)


