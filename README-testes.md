# Roteiro e Relatório de Testes - PCBs de Impedância 50 Ohm (2L e 4L)

[🇺🇸 English](./README-tests.md) | [🇧🇷 Português](./README-testes.md)

## Introdução
Projetar PCBs de RF e high speed com boa previsibilidade elétrica é desafiador porque a impedância real de uma trilha não depende apenas da largura da linha. O resultado final é fortemente influenciado por stackup, distância ao plano de referência, constante dielétrica efetiva, espessura de cobre, máscara de solda, transições de conector, vias e continuidade do caminho de retorno.

Na prática, dois layouts visualmente parecidos podem apresentar comportamento muito diferente em `S11` e `S21` quando fabricados. Pequenas descontinuidades, como um slot no plano de GND ou uma mudança de retorno de corrente em região de via, podem gerar reflexão adicional, ripple e notch de transmissão, principalmente com o aumento da frequência.

Por isso, o casamento de impedância precisa ser validado por medição, não apenas por cálculo ou simulador. Em sistemas RF e interfaces high speed, um mismatch recorrente degrada eficiência de transferência de energia, piora margem de sinal, aumenta sensibilidade a variações de processo e pode comprometer compliance elétrica do produto.

Este roteiro foi criado para transformar o teste em um processo repetível e comparável entre cupons de 2 camadas e 4 camadas, permitindo correlacionar decisões de layout com desempenho medido. O foco é gerar dados objetivos para suportar decisões de projeto, reduzir iterações e aumentar a robustez de futuras placas.

---

**Projeto:** Cupons de Impedância 50 Ohm  
**Responsável:** Marcos Vinicius Goncalves  
**Data:** YYYY-MM-DD  
**VNA:** NanoVNA-F V2 (50 kHz - 3 GHz)  
**Software PC:** NanoVNA Saver  
**Fabricante PCB:** JLCPCB  
**Objetivo:** Validar e comparar o comportamento de linhas 50 Ohm (microstrip/CPWG) em diferentes cenários (baseline, vias, vias + return path, descontinuidade de GND, matching), em placas de 2 camadas e 4 camadas.

---

## 1. Escopo

### 1.1 Placas testadas
- [ ] **PCB 4 camadas** (stackup: JLC04161H-3313)  
- [ ] **PCB 2 camadas** (FR-4 padrão)

### 1.2 Cupons incluídos (somente 50 Ohm)

#### PCB 4L (conforme silk/labels)
1. Microstrip 50 Ohm baseline (W=350)
2. CPWG 50 Ohm (W=285, G=200)
3. CPWG 50 Ohm (W=210, G=120)
4. CPWG 50 Ohm + matching (W=285, G=200)
5. Microstrip 50 Ohm + vias
6. Microstrip 50 Ohm + vias + return path
7. Microstrip 50 Ohm + descontinuidade de GND (slot no L2)

#### PCB 2L (conforme silk/labels)
1. Microstrip 50 Ohm (W=2700)
2. CPWG 50 Ohm baseline (W=800, G=200)
3. CPWG 50 Ohm (W=380, G=120)
4. CPWG 50 Ohm + matching (W=800, G=200)
5. CPWG 50 Ohm + vias
6. CPWG 50 Ohm + vias + return path
7. CPWG 50 Ohm + descontinuidade de GND (slot)

---

## 2. Equipamentos, acessórios e configurações

### 2.1 Equipamentos
- NanoVNA-F V2
- PC com NanoVNA Saver
- Cabos SMA macho-macho (qty: ___, modelo: ___)
- Kit de calibração SMA: Open / Short / Load / Thru

### 2.2 Configuração de medição (padrão)
- **Faixa:** Start = ___ MHz | Stop = ___ MHz  
- **Pontos:** ___ (401 ou 801)  
- **Traços observados:**
  - S11 LogMag (dB)
  - S21 LogMag (dB)

### 2.3 Calibração
- Tipo: **SOLT 2-port**
- Local da calibração: **ponta dos cabos (plano de referência nos conectores SMA)**
- Slot de calibração usado: ___
- Observações: ___

### 2.4 Condições de teste
- Aperto SMA: [ ] mão consistente  [ ] torque (___ N.m)  
- Adaptadores usados: [ ] não  [ ] sim (listar: ___)  
- Temperatura ambiente (aprox): ___ degC

---

## 3. Critérios de avaliação

### 3.1 Métricas registradas
Para cada cupom:
- **S11 (Return Loss):**
  - pior S11 (dB) em 1-3 GHz
  - (opcional) pior S11 em sub-faixas (ex.: 700-1000, 1700-2200, 2500-3000 MHz)
- **S21 (Insertion Loss):**
  - S21 em 1 GHz, 2 GHz, 3 GHz (dB)
  - presença de ripple/notch (sim/não + descrição)

### 3.2 Expectativas (hipóteses)
- **Baseline:** melhor S11 (mais negativo) e S21 mais suave.
- **Vias vs Vias+RP:** vias+RP deve reduzir reflexão e ripple.
- **Descontinuidade de GND:** deve aumentar reflexão (piora de S11) e ripple/notch em S21.
- **CPWG G120 vs G200:** ambos 50 Ohm, mas G120 tende a maior confinamento (pode melhorar launch / pode ser mais sensível a tolerância).

---

## 4. Procedimento passo a passo (executado)

### 4.1 Checklist pré-teste
- [ ] Inspeção visual (máscara, gap CPW, plano/slot, vias RP)
- [ ] Conectores SMA bem soldados (sem solda invadindo gap)
- [ ] Cabos sem folga mecânica (sem tracionar SMA)

### 4.2 Sequência de medições

**PCB 4L** (ordem):
- [ ] 4L_01 Microstrip baseline
- [ ] 4L_02 CPWG W285 G200
- [ ] 4L_03 CPWG W210 G120
- [ ] 4L_05 Microstrip + vias
- [ ] 4L_06 Microstrip + vias + RP
- [ ] 4L_07 Microstrip + GND desc (L2)
- [ ] 4L_04 CPWG + matching (bypass)

**PCB 2L** (ordem):
- [ ] 2L_01 Microstrip
- [ ] 2L_02 CPWG baseline W800 G200
- [ ] 2L_03 CPWG W380 G120
- [ ] 2L_05 CPWG + vias
- [ ] 2L_06 CPWG + vias + RP
- [ ] 2L_07 CPWG + GND desc
- [ ] 2L_04 CPWG + matching (bypass)

### 4.3 Repetibilidade (recomendado)
Selecionar 3 cupons por placa e repetir reconexão:
- [ ] Baseline (R1, R2)
- [ ] Vias (R1, R2)
- [ ] GND desc (R1, R2)

---

## 5. Resultados - Tabela mestre

> Preencher esta tabela com os valores extraídos do NanoVNA Saver (ou manualmente).

| PCB | Cupom | Estrutura | W / G | Cenário | Arquivo .s2p | S11 pior 1-3GHz (dB) | S21 @1GHz (dB) | S21 @2GHz (dB) | S21 @3GHz (dB) | Ripple/Notch (sim/não) | Observações |
|---|---:|---|---|---|---|---:|---:|---:|---:|---|---|
| 4L | 01 | Microstrip | W350 | Baseline | 4L_01_....s2p |  |  |  |  |  |  |
| 4L | 02 | CPWG | W285/G200 | Baseline | 4L_02_....s2p |  |  |  |  |  |  |
| 4L | 03 | CPWG | W210/G120 | Baseline | 4L_03_....s2p |  |  |  |  |  |  |
| 4L | 05 | Microstrip | W350/W300 | Vias | 4L_05_....s2p |  |  |  |  |  |  |
| 4L | 06 | Microstrip | W350/W300 | Vias + RP | 4L_06_....s2p |  |  |  |  |  |  |
| 4L | 07 | Microstrip | W350 | GND desc (L2) | 4L_07_....s2p |  |  |  |  |  |  |
| 4L | 04 | CPWG | W285/G200 | Matching (bypass) | 4L_04_....s2p |  |  |  |  |  |  |
| 2L | 01 | Microstrip | W2700 | Baseline | 2L_01_....s2p |  |  |  |  |  |  |
| 2L | 02 | CPWG | W800/G200 | Baseline | 2L_02_....s2p |  |  |  |  |  |  |
| 2L | 03 | CPWG | W380/G120 | Baseline | 2L_03_....s2p |  |  |  |  |  |  |
| 2L | 05 | CPWG | W800 | Vias | 2L_05_....s2p |  |  |  |  |  |  |
| 2L | 06 | CPWG | W800 | Vias + RP | 2L_06_....s2p |  |  |  |  |  |  |
| 2L | 07 | CPWG | W800 | GND desc | 2L_07_....s2p |  |  |  |  |  |  |
| 2L | 04 | CPWG | W800/G200 | Matching (bypass) | 2L_04_....s2p |  |  |  |  |  |  |

---

## 6. Comparações e análise (preencher após medir)

### 6.1 Baseline - 4L vs 2L
- **4L microstrip baseline vs 2L microstrip**  
  Observações: ___  
- **4L CPWG baseline vs 2L CPWG baseline**  
  Observações: ___

### 6.2 CPWG: G200 vs G120 (mesmo stackup)
- 4L: (W285/G200) vs (W210/G120)  
  Observado: ___
- 2L: (W800/G200) vs (W380/G120)  
  Observado: ___

### 6.3 Vias vs Vias + Return Path
- 4L microstrip: vias vs vias+RP  
  Observado: ___  
- 2L CPWG: vias vs vias+RP  
  Observado: ___  

### 6.4 Baseline vs descontinuidade de GND
- 4L (slot no L2): baseline vs GND desc  
  Observado: ___  
- 2L (slot): baseline vs GND desc  
  Observado: ___  

### 6.5 Matching (bypass)
- 4L CPWG + matching (bypass) vs CPWG baseline  
  Observado: ___  
- 2L CPWG + matching (bypass) vs CPWG baseline  
  Observado: ___  

---

## 7. Conclusões

### 7.1 Principais achados
- ___
- ___
- ___

### 7.2 Recomendações de layout (com base nos dados)
- Para 4L:
  - ___
- Para 2L:
  - ___

### 7.3 Próximos passos
- [ ] Rodar medições com 801 pontos em cupons-chave (baseline/vias/slot)
- [ ] Fazer "time-domain/TDR via VNA" no PC (opcional)
- [ ] Ajuste de matching (se aplicável)
- [ ] Repetir medições com outra amostra de PCB (se houver)

---

## 8. Anexos

### 8.1 Arquivos gerados
- Pasta de resultados: `tests_50ohm/YYYY-MM-DD/`
- Lista de .s2p incluídos: ___

### 8.2 Fotos do setup
- Foto 1: cabos e VNA conectados (___)
- Foto 2: detalhe do SMA soldado (___)
- Foto 3: detalhe do gap CPW / via fence (___)

---

## 9. Log de alterações do relatório
| Data | Autor | Alteração |
|---|---|---|
| YYYY-MM-DD | Marcos | Criação do template |
|  |  |  |


