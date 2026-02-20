# 📊 Projeto: Predição de Preço de Aluguel com Regressão Linear

## 📌 Sobre o Projeto

Projeto desenvolvido no **Módulo 18 – Profissão Cientista de Dados**, focado na construção de modelos de regressão linear simples e múltipla para predição de preços de imóveis para alugar.

A regressão linear é uma técnica estatística utilizada para modelar a relação entre uma variável dependente (target) e uma ou mais variáveis independentes (features). No contexto imobiliário, permite estimar o valor de aluguel com base em características do imóvel como metragem, número de quartos, banheiros, suítes e vagas.

O **objetivo principal** deste projeto é desenvolver um modelo capaz de prever o valor do aluguel de um imóvel com base em suas características, comparando diferentes cenários de transformação de variáveis (com e sem aplicação de log) e avaliando a performance entre regressão linear simples e múltipla.

Este projeto marca um momento especial na minha trajetória: foi o primeiro modelo de Machine Learning que treinei, colocando em prática os conceitos de regressão linear vistos no curso.

---

## 🎯 Objetivos

* Realizar pré-processamento completo dos dados imobiliários
* Identificar e tratar outliers utilizando regras de plausibilidade do mercado imobiliário
* Analisar correlações entre as variáveis e o valor do aluguel
* Treinar e comparar modelos de regressão linear simples e múltipla
* Avaliar performance nos dados de treino e teste usando o coeficiente R²

---

## 📁 Estrutura do Projeto

```
├── notebooks/
│   └── Profissao_Cientista_de_Dados_M18_Pratique.ipynb
├── data/
│   └── ALUGUEL_MOD12.csv
└── README.md
```

---

## 🛠️ Tecnologias Utilizadas

* **Python 3.8+**
* **Pandas** - Manipulação e análise de dados
* **NumPy** - Operações numéricas e transformações logarítmicas
* **Matplotlib** - Visualização de dados e gráficos de dispersão
* **Seaborn** - Visualizações estatísticas avançadas
* **Plotly Express** - Gráficos interativos (box plots, scatter plots, heatmaps)
* **Scikit-learn** - Divisão de dados e modelo de Regressão Linear
* **Jupyter Notebook** - Ambiente de desenvolvimento

---

## 📊 Descrição dos Dados

O dataset contém informações de imóveis disponíveis para aluguel com as seguintes variáveis:

| Variável | Descrição |
|----------|-----------|
| **Valor_Aluguel** | Valor total pago no aluguel (variável alvo) |
| **Valor_Condominio** | Valor do condomínio |
| **Metragem** | Metragem do apartamento (m²) |
| **N_Quartos** | Número de quartos do imóvel |
| **N_banheiros** | Número de banheiros |
| **N_Suites** | Número de suítes |
| **N_Vagas** | Número de vagas de garagem |

---

## 📈 Análises Realizadas

### **Etapa 1: Pré-processamento dos Dados**

#### ✅ Verificação e Ajuste de Tipos de Dados
- Conversão de `Valor_Aluguel`, `Valor_Condominio` e `Metragem` para tipo `float`
- Verificação e confirmação de ausência de dados faltantes no dataset

#### ✅ Análise de Outliers
As variáveis `Valor_Aluguel`, `Valor_Condominio` e `Metragem` apresentaram indícios de outliers, com média e mediana distantes e valores máximos muito acima do 3º quartil.

**Estratégia adotada:** Os valores altos de aluguel foram considerados plausíveis (imóveis de alto padrão), portanto optou-se por **não remover os outliers**, mas sim aplicar transformação logarítmica para reduzir seu impacto sem perda de informação.

#### ✅ Validação por Regras de Plausibilidade do Mercado Imobiliário
Para detectar possíveis erros de digitação, foram aplicadas regras baseadas em padrões reais do mercado:

- 2 quartos → metragem ≥ 55 m²
- 3 quartos → metragem ≥ 80 m²
- 4 quartos → metragem ≥ 100 m²
- 2+ banheiros → metragem ≥ 60 m²
- Condomínio > R$ 2.000 com metragem < 70 m² → suspeito

---

### **Etapa 2: Análise Exploratória de Dados (EDA)**

#### 📊 Análise Bivariada

Os gráficos de barra e dispersão revelaram que **todas as variáveis possuem correlação positiva com o valor do aluguel**: quanto maior o valor de cada variável, maior tende a ser o valor do aluguel.

**Principais correlações identificadas (matriz de correlação):**

| Par de Variáveis | Correlação |
|-----------------|-----------|
| N_banheiros vs N_Suites | **0.92** (mais alta) |
| Metragem vs Valor_Condominio | **0.81** |
| N_Vagas vs Metragem | **0.74** |
| Metragem vs Valor_Aluguel | **0.73** (maior correlação com o target) |
| N_Quartos vs Valor_Aluguel | **0.41** (menor correlação) |

> A variável **Metragem** é a que possui maior correlação com o target `Valor_Aluguel`.

---

### **Etapa 3: Transformação de Variáveis e Separação das Bases**

#### 🔄 Cenários de Transformação Logarítmica

Foram criadas três versões do dataset para comparação:

| Cenário | Transformações Aplicadas |
|---------|--------------------------|
| **Caso 1** | Log apenas no target (`Valor_Aluguel`) |
| **Caso 2** | Log no target e em `Metragem` |
| **Caso 3** | Log no target, em `Metragem` e em `Valor_Condominio` |

#### ✂️ Divisão dos Dados

**Proporção:** 75% treino / 25% teste (`random_state=42`) — aplicada igualmente aos 3 cenários.

---

### **Etapa 4: Regressão Linear Simples**

Modelo treinado utilizando apenas a variável `Metragem` (ou `Log_Metragem`) como preditora.

#### Equações encontradas:

- **Caso 1:** `Log_Valor_Aluguel = 0.007 * Metragem + 7`
- **Caso 2:** `Log_Valor_Aluguel = 0.94 * Log_Metragem + 3.6`
- **Caso 3:** `Log_Valor_Aluguel = 0.94 * Log_Metragem + 3.6`

#### Avaliação (R²):
- Os **Casos 2 e 3** apresentaram desempenho superior ao Caso 1
- A diferença do R² entre treino e teste foi pequena em todos os cenários, indicando boa capacidade de generalização
- Casos 2 e 3 produziram resultados idênticos, indicando que o log em `Valor_Condominio` não trouxe diferença significativa na regressão simples

---

### **Etapa 5: Regressão Linear Múltipla**

Modelo treinado com todas as variáveis independentes disponíveis.

#### Variáveis utilizadas por cenário:

- **Caso 1:** Metragem, N_Quartos, N_Suites, N_Vagas, N_banheiros, Valor_Condominio
- **Caso 2:** Log_Metragem, N_Quartos, N_Suites, N_Vagas, N_banheiros, Valor_Condominio
- **Caso 3:** Log_Metragem, N_Quartos, N_Suites, N_Vagas, N_banheiros, Log_Valor_Condominio

#### Avaliação (R²):
- A regressão múltipla superou a regressão simples nos 3 cenários, confirmando que todas as variáveis contribuem para a predição
- O **Caso 2** se mostrou o mais estável, tanto na regressão simples quanto na múltipla

---

## 🔍 Principais Insights

### 💡 Sobre os Dados

1. **Ausência de valores faltantes**: Dataset de boa qualidade, sem necessidade de imputação
2. **Outliers plausíveis**: Valores altos refletem imóveis de alto padrão, não erros
3. **Alta multicolinearidade**: Várias variáveis independentes possuem forte correlação entre si (ex.: banheiros e suítes com 0.92)

### 💡 Sobre Relações entre Variáveis

1. **Metragem é a variável mais preditiva**: Maior correlação com o valor do aluguel (0.73)
2. **Todas as variáveis contribuem positivamente**: Mais quartos, banheiros, suítes e vagas → maior aluguel
3. **Transformação log melhora o modelo**: Aplicar log na metragem e no target reduz o impacto dos valores extremos e melhora o R²

### 💡 Sobre os Modelos

1. **Regressão múltipla > regressão simples**: O uso de todas as variáveis explica melhor a variação no preço do aluguel
2. **Melhor cenário**: Caso 2 (log em Metragem e no target), com resultado mais estável e sem diferença entre treino e teste
3. **Log em Valor_Condominio**: Não trouxe ganho significativo de performance em nenhum dos modelos testados

---

## 📌 Conclusão

Este projeto desenvolveu com sucesso **modelos de regressão linear simples e múltipla** para predição do valor de aluguel de imóveis, experimentando diferentes cenários de transformação de variáveis.

### ✅ Conquistas do Projeto:

1. **Pré-processamento consistente**: Tratamento adequado dos dados com validação baseada em regras do mercado imobiliário
2. **Análise exploratória completa**: Identificação das variáveis mais relevantes para predição
3. **Comparação de cenários**: Avaliação de 3 configurações distintas de transformação de variáveis
4. **Modelos treinados e avaliados**: Métricas calculadas para treino e teste, confirmando boa generalização
5. **Insight sobre transformação logarítmica**: Confirmação de que o log na metragem e no target produz o melhor resultado

---

## 👩‍💻 Autora

**Bruna S. R. Santos**

* 🔗 LinkedIn: [www.linkedin.com/in/brunasrsantos](https://www.linkedin.com/in/brunasrsantos)
* 📧 Email: brunasrsantos@gmail.com

---

## 📝 Licença

Este projeto está licenciado sob a **MIT License**.
