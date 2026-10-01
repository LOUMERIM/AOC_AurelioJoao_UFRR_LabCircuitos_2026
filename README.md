# Laboratório de Circuitos — Subsistema de Memória e Unidade de Controle

## 1. Identificação

- **Disciplina:** Arquitetura e Organização de Computadores (DCC/UFRR)
- **Professor:** Prof. Dr. Herbert Oliveira Rocha
- **Semestre:** 2026.2
- **Integrantes:**

| Nome | Matrícula |
|---|---|
| Aurélio Arendartchuk Júnior | 2022010070 |
| João Guilherme Barros Jácome | 2025015722 |

## 2. Versão do Logisim-Evolution e como abrir os arquivos

Os arquivos `.circ` foram salvos com o **Logisim-Evolution 5.0.0**.

1. Abra o Logisim-Evolution e use **Arquivo → Abrir**, escolhendo o `.circ` desejado.
2. Cada arquivo contém vários circuitos (subcircuitos). Eles aparecem no painel lateral esquerdo; dê duplo clique no nome para abri-lo.
3. Circuito principal de cada arquivo: `MemoriaPrincipal` (`parte1_memoria.circ`), `Cache` (`parte1_cache.circ`), `parte2_cabeada` (`parte2_cabeada.circ`) e `parte2_microprograma` (`parte2_microprograma.circ`) .
4. Use a ferramenta **Poke** (mão) para alternar os pinos de entrada e o menu **Simular** para pulsar o clock manualmente.

Dicas de uso:

- **Memória:** o endereço entra pelos pinos `A0..A5`. Para escrever, defina `Dados_In`, ligue `WE` e dê um pulso de clock. Para ler a RAM, mantenha `A5=0, A4=1`; com a RAM não selecionada, a saída do verificador de paridade fica vermelha (alta impedância), o que é esperado.
- **Cache:** dê um pulso em `Reset` e depois use `Endereco_Atual` (ou `Usar_Seguinte=1` para o endereço seguinte). Leia `ACERTO` **antes** do pulso de clock. O pulso é que grava a linha em caso de falta.

## 3. Arquivos entregues

| Arquivo | Descrição |
|---|---|
| `README.md` | Este documento. |
| `relatorio/relatorio_tecnico.pdf` | Relatório técnico das Partes I e II. |
| `relatorio/Prints/` | Capturas de tela do simulador usadas como evidência dos testes (paridade, T-03, T-04, T-05). |
| `parte1/parte1_memoria.circ` | Módulo de memória de 64 bytes: decodificador de endereços, ROM, RAM, banco de registradores, E/S, MUX de saída e verificação de paridade. |
| `parte1/parte1_cache.circ` | Cache de mapeamento direto (4 linhas, bloco de 2 bytes): comparador de rótulos, armazenamento rótulo/validade, arranjo de dados, contador de acertos, somador de endereço e detector de primos. |
| `parte1/parte1_planilha.xlsx` | Planilha do traço de 16 acessos, do teste de conflito e dos indicadores (acertos, faltas, taxa de acertos, AMAT). |
| `parte2/parte2_cabeada.circ` | Unidade de controle cabeada: 5 estados, 3 flip-flops D e portas lógicas. |
| `parte2/parte2_microprograma.circ` | Unidade de controle microprogramada: µPC de 3 bits, ROM de microcódigo e lógica de despacho. |
| `parte2/microcodigo.txt` | Conteúdo da ROM de microcódigo (formato carregável pelo simulador). |
| `parte2/tabela_tempo.xlsx` | Tabela de tempo (estados × 9 sinais de controle) e cálculo do CPI. |
| `.gitignore` | Arquivos ignorados pelo Git. |

## 4. Onde cada um dos 17 componentes foi instanciado

| Nº | Componente | Arquivo | Subcircuito |
|---|---|---|---|
| 01 | Flip-flop D e flip-flop JK | `parte1_cache.circ` | `Reg_Cache` (4 flip-flops D, bits de validade) |
| 01 | (registrador de estado) | `parte2_cabeada.circ` | `parte2_cabeada` (flip-flops D Q2, Q1, Q0) |
| 02 | Multiplexador de 4 entradas | `parte1_memoria.circ` | `MemoriaPrincipal` (MUX de saída, selecionado por A5, A4) |
| 02 | (leitura das linhas da cache) | `parte1_cache.circ` | `Reg_Cache` (MUX do rótulo e MUX da validade) |
| 03 | XOR a partir de AND, NOT e OR | `parte1_cache.circ` | `XOR_manual`, instanciado 3 vezes em `Cache` |
| 04 | Somador de 8 bits com constante 4 | não utilizado | - |
| 05 | Memória ROM de 8 bits | `parte1_memoria.circ` | `MemoriaPrincipal` (região 0x00–0x0F) |
| 05 | (memória de microcódigo) | `parte2_microprograma.circ` | `parte2_microprograma`  |
| 06 | Memória RAM de 8 bits | `parte1_memoria.circ` | `MemoriaPrincipal` (região 0x10–0x1F) |
| 06 | (arranjo de dados da cache) | `parte1_cache.circ` | `Cache` (RAM 8×8) |
| 07 | Banco de registradores de 8 bits | `parte1_memoria.circ` | `BancoReg` (16 registradores, região 0x20–0x2F) |
| 08 | Somador de 8 bits | `parte1_cache.circ` | `Somador_Manual`, usado em `Cache` (`Endereco_Seguinte`) |
| 09 | Detector da sequência "101" | `parte2_microprograma.circ` | `parte2_microprograma` |
| 10 | ULA de 8 bits | `parte2_microprograma.circ` | `parte2_microprograma` |
| 11 | Extensor de sinal de 4 para 8 bits | não utilizado | - |
| 12 | Máquina de estados com portas lógicas | `parte2_cabeada.circ` | `parte2_cabeada` |
| 13 | Contador síncrono (µPC de 3 bits) | `parte2_microprograma.circ` | [PREENCHER] |
| 14 | Detector de paridade ímpar | `parte1_memoria.circ` | `GeradorParidade`, instanciado 2 vezes em `MemoriaPrincipal` (escrita e leitura) |
| 15 | Otimização por mapas de Karnaugh | Relatório (Seção 3.3) | Equações implementadas em `parte2_cabeada` |
| 16 | Decodificador de 7 segmentos | `parte1_cache.circ` | `Cache` (contador de acertos) |
| 16 | (estado atual) | `parte2_cabeada.circ` | `parte2_cabeada` |
| 17 | Detector de número primo (4 bits) | `parte1_cache.circ` | `DetectorPrimo`, instanciado em `Cache` |

## 5. Declaração de uso de ferramentas de IA generativa

**Ferramenta:** Claude (Anthropic), via Claude Code e Gemini 3.1 Pro (Pro Extendido).

**Parte I (Aurélio):**

- *Construção:* a IA atuou como ajudante de construção nos passo a passo no Logisim-Evolution. A IA não montou nenhum circuito; cada componente, conexão e teste foi executado manualmente pelos integrantes, com a IA explicando os conceitos, auxiliando em possíveis passos conceituais e revisando o circuito em casos de apontamento de 25 erros de conexão. e revisou capturas de tela para apontar erros de conexão, como curto-circuito, largura de barramento, sinal de habilitação invertido e ordem de bits.
- *Cálculos e planilha:* auxiliou na nos conceitos das formulações das fórmulas da planilha eletrônica, incluindo o mapa de Karnaugh do detector de primos, na tabela de acessos esperada, nas fórmulas da planilha e na correção de referências de célula.
- *Relatório:* na revisão do texto.

**Parte II (João):**
- *Construção:* A IA foi usada para diagnosticar erros ao longo do percurso e justificar porque certas ligações apresentavam problemas e como conserta-las. Nenhum dos dois circuitos foi gerado por meio da Inteligência Artificial, tendo o papel de meramente guiar.
- *Cálculos e planilha:* Auxiliou na formação das tabelas e nos mapas de Karnough encontrados, e foi utilizada para fazer o diagrama de acordo com o funcionamento do circuito.
- *Relatório:* Estruturação e revisão do texto.

## 6. Divisão do trabalho

| Integrante | Responsabilidade |
|---|---|
| Aurélio Arendartchuk Júnior | Parte I (memória, paridade, cache, detector de primos, planilha), revisão final |
| João Guilherme Barros Jácome | Parte II (unidade de controle cabeada e microprogramada, tabela de tempo) |
| Ambos | Integração do relatório técnico |
