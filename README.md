# Reconstrução e análise da largura longitudinal de cordões produzidos por DED

Este repositório reúne o pipeline desenvolvido para reconstruir, avaliar e analisar o perfil longitudinal de largura de nove cordões produzidos por **Directed Energy Deposition (DED)**.

O trabalho está organizado em três notebooks principais:

1. `y_reconstruction_validation.ipynb`: reconstrução geométrica semiautomática da largura;
2. `y_reconstruction_repeatability.ipynb`: estudo-piloto de repetibilidade do procedimento;
3. `eda.ipynb`: análise exploratória consolidada dos alvos, perfis e implicações para modelagem.

A variável de interesse é a largura do cordão, em milímetros, ao longo da direção longitudinal. Duas versões do alvo são preservadas:

- `y_original`: alvo utilizado no pipeline anterior, obtido a partir de medições manuais pelo ImageJ em posições longitudinais e interpolado para os frames;
- `y_reconstruido`: alvo obtido por reconstrução geométrica semiautomática a partir das fotografias dos cordões solidificados.

O alvo reconstruído melhora a documentação e a rastreabilidade do processo de obtenção de `y`.Não há uma medição metrológica independente que permita determinar qual dos dois alvos é mais próximo do valor real.

---

## Organização do pipeline

```text
y_reconstruction_validation.ipynb
        │
        │ gera os perfis reconstruídos, o dataset atualizado
        │ e os arquivos de auditoria
        ▼
y_reconstruction_repeatability.ipynb
        │
        │ avalia a repetibilidade condicional do procedimento
        ▼
eda.ipynb
        │
        │ consolida a análise exploratória e exporta
        │ tabelas, figuras, manifests e verificações
        ▼
eda_sophia_results/
```

Os notebooks devem ser executados nessa ordem quando todo o pipeline precisar ser reconstruído.

---

## Notebooks

### 1. `y_reconstruction_validation.ipynb`

Reconstrói o perfil longitudinal de largura dos experimentos L1 a L9.

O fluxo inclui:

1. carregamento e validação das fotografias;
2. seleção da moeda usada como referência dimensional;
3. detecção circular da moeda e cálculo de uma escala individual em mm/pixel;
4. seleção da região de interesse de cada cordão;
5. marcação das extremidades e alinhamento do eixo longitudinal;
6. marcação de pontos-guia para as bordas superior e inferior;
7. rastreamento das bordas por programação dinâmica;
8. cálculo do perfil denso de largura;
9. reamostragem dos perfis;
10. definição da orientação longitudinal;
11. integração do novo alvo ao dataset estático;
12. exportação de imagens, tabelas, JSONs e manifests de auditoria.

As etapas manuais utilizam janelas gráficas do OpenCV. 

#### Observação sobre L9

A fotografia do experimento L9 veio de um conjunto de todos os experimentos, sua fotografia isolada se perdeu. Ela também estava orientada de forma oposta às demais. A imagem foi rotacionada antes da reconstrução atual, e as etapas de calibração, marcação, rastreamento e geração do perfil foram refeitas.

---

### 2. `y_reconstruction_repeatability.ipynb`

Avalia a **repetibilidade condicional** do procedimento de reconstrução.

O estudo inclui:

- experimentos L2, L6 e L9;
- dois operadores;
- três reconstruções por operador e experimento;
- 18 reconstruções no total.

A seleção foi planejada para incluir diferentes níveis de dificuldade operacional:

- L2: condição visual mais favorável;
- L6: condição intermediária;
- L9: condição mais complicada, com menor resolução e fronteiras mais ambíguas.

São refeitos em cada repetição:

- as marcações das extremidades;
- os pontos-guia;
- as trajetórias das bordas;
- o perfil resultante.

---

### 3. `eda.ipynb`

Consolida a análise exploratória dos resultados produzidos pelos dois notebooks anteriores.

A EDA abrange:

- validação dos arquivos e das colunas obrigatórias;
- auditoria dos 26.634 frames;
- comparação entre `y_original` e `y_reconstruido`;
- comparação longitudinal por experimento;
- análise de diferenças, percentis, correlações e amplitudes;
- avaliação de cortes longitudinais;
- análise dos trechos inicial, intermediário e final;
- distribuições por experimento;
- repetibilidade intraoperador;
- comparação entre operadores;
- relações descritivas com as condições de deposição;
- limitações para modelagem;
- decisões metodológicas;
- exportação de figuras, tabelas, ambiente computacional e manifests com hashes.

---

## Requisitos

A última execução consolidada da EDA utilizou Python 3.11. As principais dependências são:

- Python 3.11;
- NumPy;
- pandas;
- Matplotlib;
- OpenCV;
- IPython.

---

## Localização da raiz do projeto

Os notebooks procuram automaticamente a raiz do projeto pela estrutura de diretórios.

Quando a detecção automática não funcionar, existem duas opções.

### Opção 1: configurar no notebook

Preencher:

```python
PROJECT_DIR_OVERRIDE = r"C:\caminho\para\o\projeto"
```

### Opção 2: usar uma variável de ambiente

Definir:

```text
IC_ML_DED_PROJECT_DIR
```

com o caminho absoluto da raiz do projeto.

---

## Ordem de execução

### Execução completa

1. Abrir `y_reconstruction_validation.ipynb`.
2. Conferir os caminhos e as opções de reutilização de anotações.
3. Executar o notebook integralmente.
4. Inspecionar visualmente as calibrações, alinhamentos e bordas.
5. Confirmar a geração dos perfis e do dataset reconstruído.
6. Abrir `y_reconstruction_repeatability.ipynb`.
7. Conferir as 18 combinações de experimento e repetição.
8. Executar a consolidação e a exportação do estudo.
9. Abrir `eda.ipynb`.
10. Executar integralmente para gerar as análises finais.

### Quando os produtos já existem

Quando as anotações e os resultados intermediários já estiverem presentes, os notebooks podem reutilizá-los. Ainda assim, recomenda-se:

- preservar uma cópia dos JSONs antes de novas marcações;
- conferir as opções de aprovação e salvamento;
- evitar sobrescrever resultados sem inspeção visual;
- executar cada notebook com o kernel reiniciado;
- salvar somente depois de uma execução completa e sem erros.
