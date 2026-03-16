# Roteiro e Relatório de Testes - PCBs de Impedância 50 Ohm (2L e 4L)

[🇺🇸 English](./README-tests.md) | [🇧🇷 Português](./README-testes.md)

[⬅ Voltar para o README Geral](./README-pt.md)

## Índice
- [1. Escopo](#scope)
- [1.1 Sub-placas (cupons 50 Ohm)](#included-coupons)
- [2. Equipamentos, acessórios e configurações](#equipment-settings)
- [2.1 Equipamentos](#equipment)
- [2.2 Configuração de medição (padrão)](#measurement-setup)
- [2.3 Calibração](#calibration)
- [2.4 Condições de teste](#test-conditions)
- [3. Critérios de avaliação](#evaluation-criteria)
- [3.1 Métricas registradas](#recorded-metrics)
- [3.2 Expectativas (hipóteses)](#expectations)
- [4. Procedimento passo a passo (executado)](#procedure)
- [4.1 Checklist pré-teste](#pretest-checklist)
- [4.2 Sequência de medições](#measurement-sequence)
- [4.3 Repetibilidade (recomendado)](#repeatability)
- [5. Resultados - Tabela mestre](#results-master-table)
- [6. Comparações e análise](#comparisons-analysis)
- [7. Conclusões](#conclusions)
- [8. Anexos](#attachments)
- [9. Log de alterações do relatório](#report-change-log)

**Projeto:** Cupons de Impedância 50 Ohm  
**Responsável:** Marcos Vinicius Goncalves  
**Data:** YYYY-MM-DD  
**VNA:** NanoVNA-F V2 (50 kHz - 3 GHz)  
**Software PC:** NanoVNA Saver  
**Fabricante PCB:** JLCPCB  
**Objetivo:** Validar e comparar o comportamento de linhas 50 Ohm (microstrip/CPWG) em diferentes cenários (baseline, vias, vias + return path, descontinuidade de GND, matching), em placas de 2 camadas e 4 camadas.

---

<a id="scope"></a>
## 1. Escopo

<a id="included-coupons"></a>
### 1.1 Sub-placas (cupons 50 Ohm)

Cada cupom é tratado como uma sub-placa independente (ID = PCB + número do cupom).

1. `4L_01` - Microstrip 50 Ohm baseline (W=350)
2. `4L_02` - CPWG 50 Ohm (W=285, G=200)
3. `4L_03` - CPWG 50 Ohm (W=210, G=120)
4. `4L_04` - CPWG 50 Ohm + matching (W=285, G=200)
5. `4L_05` - Microstrip 50 Ohm + vias
6. `4L_06` - Microstrip 50 Ohm + vias + return path
7. `4L_07` - Microstrip 50 Ohm + descontinuidade de GND (slot no L2)
8. `2L_01` - Microstrip 50 Ohm (W=2700)
9. `2L_02` - CPWG 50 Ohm baseline (W=800, G=200)
10. `2L_03` - CPWG 50 Ohm (W=380, G=120)
11. `2L_04` - CPWG 50 Ohm + matching (W=800, G=200)
12. `2L_05` - CPWG 50 Ohm + vias
13. `2L_06` - CPWG 50 Ohm + vias + return path
14. `2L_07` - CPWG 50 Ohm + descontinuidade de GND (slot)

---

<a id="equipment-settings"></a>
## 2. Equipamentos, acessórios e configurações

<a id="equipment"></a>
### 2.1 Equipamentos
- NanoVNA-F V2
- PC com NanoVNA Saver
- Cabos SMA macho-macho (qty: ___, modelo: ___)
- Kit de calibração SMA: Open / Short / Load / Thru

<a id="measurement-setup"></a>
### 2.2 Configuração de medição (padrão)
- **Faixa:** Start = ___ MHz | Stop = ___ MHz  
- **Pontos:** ___ (401 ou 801)  
- **Traços observados:**
  - S11 LogMag (dB)
  - S21 LogMag (dB)

<a id="calibration"></a>
### 2.3 Calibração
- Tipo: **SOLT 2-port**
- Local da calibração: **ponta dos cabos (plano de referência nos conectores SMA)**
- Slot de calibração usado: ___
- Observações: ___

<a id="test-conditions"></a>
### 2.4 Condições de teste
- Aperto SMA: [ ] mão consistente  [ ] torque (___ N.m)  
- Adaptadores usados: [ ] não  [ ] sim (listar: ___)  
- Temperatura ambiente (aprox): ___ degC

---

<a id="evaluation-criteria"></a>
## 3. Critérios de avaliação

<a id="recorded-metrics"></a>
### 3.1 Métricas registradas
Para cada cupom:
- **S11 (Return Loss):**
  - pior S11 (dB) em 1-3 GHz
  - (opcional) pior S11 em sub-faixas (ex.: 700-1000, 1700-2200, 2500-3000 MHz)
- **S21 (Insertion Loss):**
  - S21 em 1 GHz, 2 GHz, 3 GHz (dB)
  - presença de ripple/notch (sim/não + descrição)

<a id="expectations"></a>
### 3.2 Expectativas (hipóteses)
- **Baseline:** melhor S11 (mais negativo) e S21 mais suave.
- **Vias vs Vias+RP:** vias+RP deve reduzir reflexão e ripple.
- **Descontinuidade de GND:** deve aumentar reflexão (piora de S11) e ripple/notch em S21.
- **CPWG G120 vs G200:** ambos 50 Ohm, mas G120 tende a maior confinamento (pode melhorar launch / pode ser mais sensível a tolerância).

---

<a id="procedure"></a>
## 4. Procedimento passo a passo (executado)

<a id="pretest-checklist"></a>
### 4.1 Checklist pré-teste
- [ ] Inspeção visual (máscara, gap CPW, plano/slot, vias RP)
- [ ] Conectores SMA bem soldados (sem solda invadindo gap)
- [ ] Cabos sem folga mecânica (sem tracionar SMA)

<a id="measurement-sequence"></a>
### 4.2 Sequência de medições

Ordem recomendada por sub-placa:
- [ ] `4L_01` Microstrip baseline
- [ ] `4L_02` CPWG W285 G200
- [ ] `4L_03` CPWG W210 G120
- [ ] `4L_05` Microstrip + vias
- [ ] `4L_06` Microstrip + vias + RP
- [ ] `4L_07` Microstrip + GND desc (L2)
- [ ] `4L_04` CPWG + matching (bypass)
- [ ] `2L_01` Microstrip
- [ ] `2L_02` CPWG baseline W800 G200
- [ ] `2L_03` CPWG W380 G120
- [ ] `2L_05` CPWG + vias
- [ ] `2L_06` CPWG + vias + RP
- [ ] `2L_07` CPWG + GND desc
- [ ] `2L_04` CPWG + matching (bypass)

<a id="repeatability"></a>
### 4.3 Repetibilidade (recomendado)
Selecionar 3 sub-placas e repetir reconexão:
- [ ] Baseline (R1, R2)
- [ ] Vias (R1, R2)
- [ ] GND desc (R1, R2)

---

<a id="results-master-table"></a>
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

<a id="comparisons-analysis"></a>
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

<a id="conclusions"></a>
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

<a id="attachments"></a>
## 8. Anexos

### 8.1 Arquivos gerados
- Pasta de resultados: `tests_50ohm/YYYY-MM-DD/`
- Lista de .s2p incluídos: ___

### 8.2 Fotos do setup
- Foto 1: cabos e VNA conectados (___)
- Foto 2: detalhe do SMA soldado (___)
- Foto 3: detalhe do gap CPW / via fence (___)

---

<a id="report-change-log"></a>
## 9. Log de alterações do relatório
| Data | Autor | Alteração |
|---|---|---|
| YYYY-MM-DD | Marcos | Criação do template |
|  |  |  |


