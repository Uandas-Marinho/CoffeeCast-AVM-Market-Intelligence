# ☕ CoffeeCast Mobile: Real-Time Coffee & Currency Intelligence

> Um protótipo funcional de painel inteligente projetado para simular a experiência de um aplicativo móvel voltado à análise de mercado, cruzando dados históricos e em tempo real da cotação do café (Yahoo Finance) e da variação cambial do Dólar (USD/BRL).

---

## 📝 Descrição do Projeto (Extended Description)

O **CoffeeCast Mobile** foi concebido para resolver um desafio prático de produtores e analistas do agronegócio: a necessidade de visualizar de forma rápida, centralizada e intuitiva a evolução do preço estimado da saca de café de 60kg em moeda local (R$).

Como o preço internacional do café é cotado originalmente em centavos de dólar por libra-peso (na Bolsa de Nova York - ICE) e o mercado nacional é ditado pela moeda brasileira, a tomada de decisão exige um cálculo constante que combina duas variáveis altamente voláteis: o fechamento diário da commodity e a taxa de câmbio USD/BRL.

Este projeto centraliza e automatiza todo este pipeline, entregando uma interface no estilo *Mobile-First* diretamente no ambiente do Jupyter Notebook / Google Colab.

### 🔄 Como Funciona o Cálculo?
A precificação estimada da saca é obtida através da fórmula:
$$\text{Preço da Saca (R\$)} = \left( \frac{\text{Preço ICE (US\$ Cents)} \times 132.2773}{100} \right) \times \text{Câmbio (USD/BRL)}$$
*Onde $132.2773$ é o fator de conversão de libras-peso para uma saca padrão de 60kg.*

---

## 🛠️ Stack Tecnológica & Arquitetura

*   **Ingestão de Dados:** `yfinance` (Yahoo Finance API) para coleta diária e automatizada dos tickers `KC=F` (Coffee C) e `USDBRL=X` (USD/BRL).
*   **Manipulação & Análise:** `pandas` para estruturação de séries temporais, tratamento de dados ausentes e cálculos financeiros.
*   **Interface de Usuário (UI):** `ipywidgets` (HTML e ToggleButtons) simulando uma interface responsiva nativa para celulares.
*   **Visualização de Dados (Engine Robusta):** `matplotlib` com conversão dinâmica para imagem via buffer de memória (`io.BytesIO`) codificada em **Base64**.

### 💡 Engenharia de Compatibilidade (O diferencial técnico)
Para superar limitações comuns em ambientes de nuvem (como o Google Colab restringindo a execução de códigos Javascript e gerando problemas de renderização de gráficos interativos do Plotly em determinados navegadores), a arquitetura deste projeto implementa renderização estática assíncrona:
1. O usuário altera o período de análise (Ex: 30 dias, 6 meses, 1 ano).
2. O back-end filtra a série temporal e renderiza o gráfico de forma limpa em segundo plano no servidor.
3. O gráfico é convertido diretamente em uma string de texto Base64.
4. O HTML do painel injeta o Base64 na tag `<img src="data:image/png;base64,..." />` instantaneamente, garantindo compatibilidade de 100% em qualquer navegador moderno e mantendo o gráfico posicionado de maneira correta abaixo dos botões de período.

---

## 🖥️ Demonstração da Interface

A interface foi estruturada em um contêiner elegante de tamanho fixo (`max-width: 450px`) simulando a tela de um smartphone, contendo:
*   **Cabeçalho Temático** institucional.
*   **KPI Cards (Cartões de Indicadores):** Exibição do último preço calculado da saca de café (R$) e câmbio comercial atualizado, com bordas coloridas indicativas.
*   **Abas de Período:** Seletores rápidos de tempo dinâmicos.
*   **Subplots Alinhados:** Gráficos paralelos de evolução temporal compartilhando o mesmo eixo temporal para correlação visual rápida.

---

## 🚀 Como Executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/coffeecast-mobile.git
