# 📚 Automação da Busca de Livros com Selenium

Automação web em Python que lê uma planilha de livros, pesquisa cada um deles em sites diferentes usando o **Selenium** e registra automaticamente o **preço** e o **link** encontrados na própria planilha.

---

## 📑 Sumário

- [Sobre o projeto](#-sobre-o-projeto)
- [Funcionalidades](#-funcionalidades)
- [Como funciona](#-como-funciona)
- [Tecnologias](#-tecnologias)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Pré-requisitos](#-pré-requisitos)
- [Instalação](#-instalação)
- [Dados de entrada](#-dados-de-entrada)
- [Como executar](#-como-executar)
- [Exemplo de resultado](#-exemplo-de-resultado)
- [Observações](#-observações)
- [Autor](#-autor)

---

## 📌 Sobre o projeto

Pesquisar o preço de vários livros em diferentes sites, um por um, é um trabalho repetitivo. Este projeto automatiza essa tarefa: o navegador é controlado por código, que pesquisa cada título da planilha, identifica o resultado correto e preenche os dados encontrados.

A busca segue uma ordem de prioridade:

1. Primeiro, o livro é procurado no **[Project Gutenberg](https://www.gutenberg.org/)**, uma biblioteca de livros gratuitos.
2. Se não for encontrado, a busca continua no **[Books to Scrape](https://books.toscrape.com/)**, um site de livros criado para treinar web scraping.

## ✨ Funcionalidades

- Leitura da lista de livros a partir de uma planilha Excel.
- Pesquisa automática no navegador, com preenchimento do campo de busca do Gutenberg.
- Conferência do **título e do autor** nos resultados do Gutenberg, para evitar pegar o livro errado.
- Busca em **todas as páginas** do Books to Scrape, avançando pelo botão *next* até encontrar o título.
- Registro do link e do preço de cada livro encontrado.
- Atualização da planilha com os resultados ao final do processo.

## 🔄 Como funciona

1. **Abre** o navegador Chrome controlado pelo Selenium.
2. **Carrega** a planilha `Produtos.xlsx` com a lista de livros.
3. **Percorre** cada linha da planilha e pesquisa o livro no Gutenberg.
   - Se encontrar, registra o link e define o preço como `0` (livro gratuito).
4. **Se não encontrar** no Gutenberg, pesquisa no Books to Scrape, página por página, comparando o título.
   - Se encontrar, registra o link e o preço exibido no site (em libras, `£`).
5. **Salva** os resultados na planilha e **encerra** o navegador.

## 🛠 Tecnologias

- [Python 3](https://www.python.org/)
- [Jupyter Notebook](https://jupyter.org/)
- [Selenium](https://www.selenium.dev/) para controlar o navegador e extrair os dados das páginas
- [pandas](https://pandas.pydata.org/) e [openpyxl](https://openpyxl.readthedocs.io/) para ler e gravar a planilha
- Google Chrome

## 📁 Estrutura do repositório

```
.
├── BuscaLivros.ipynb    # Notebook com toda a automação
├── Produtos.xlsx      # Planilha de entrada e saída (lista de livros)
└── README.md
```

## ✅ Pré-requisitos

- Python 3 e Jupyter Notebook (ou VS Code com suporte a notebooks)
- Google Chrome instalado
- Conexão com a internet

> As versões recentes do Selenium (4.6 em diante) baixam e configuram o driver do Chrome automaticamente, então normalmente não é preciso instalar o ChromeDriver à mão.

## ⚙ Instalação

```bash
# Clone o repositório
git clone https://github.com/RafaelPortDev/Automacao-Busca-Livros-Selenium.git
cd nome-do-repositorio

# (Opcional) crie e ative um ambiente virtual
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/macOS

# Instale as dependências
pip install selenium pandas openpyxl notebook
```

## 🗂 Dados de entrada

A planilha `Produtos.xlsx` deve ficar na mesma pasta do notebook e conter as colunas:

| Coluna | Descrição | Preenchida por |
|---|---|---|
| `nome` | Título do livro | Você |
| `autor` | Autor do livro (usado na conferência do Gutenberg) | Você |
| `categoria` | Categoria do livro | Você |
| `preco` | Preço encontrado | Automação |
| `link` | Link da página do livro | Automação |

Exemplo da planilha antes da execução:

| nome | autor | categoria | preco | link |
|---|---|---|---|---|
| Frankenstein | Mary Shelley | Classics | | |
| Romeo and Juliet | Shakespeare | Classics | | |
| The Great Gatsby | Fitzgerald | Classics | | |
| Algorithms to Live By | Brian Christian | Nonfiction | | |
| Sapiens | Yuval Harari | History | | |
| Smarter Faster Better | Charles Duhigg | Nonfiction | | |

## ▶ Como executar

1. Deixe o `Produtos.xlsx` na mesma pasta do notebook.
2. Abra o notebook:
   ```bash
   jupyter notebook BuscaLivros.ipynb
   ```
3. Execute as células em ordem (**Kernel → Restart & Run All**).
4. Uma janela do Chrome será aberta e controlada pelo código. Aguarde até o fim, sem fechá-la.
5. Ao terminar, abra o `Produtos.xlsx` para ver os preços e links preenchidos.

## 🧪 Exemplo de resultado

Resultado da execução com a planilha de exemplo:

| Livro | Onde foi encontrado | Preço | Link |
|---|---|---|---|
| Frankenstein | Não encontrado | - | - |
| Romeo and Juliet | Gutenberg | 0 | https://www.gutenberg.org/ebooks/1513 |
| The Great Gatsby | Gutenberg | 0 | https://www.gutenberg.org/ebooks/64317 |
| Algorithms to Live By | Books to Scrape | £30.81 | link da página do livro no site |
| Sapiens | Books to Scrape | £54.23 | link da página do livro no site |
| Smarter Faster Better | Books to Scrape | £38.89 | link da página do livro no site |

Os livros não encontrados em nenhum dos sites ficam com `preco` e `link` vazios.

## 📝 Observações

- O notebook **sobrescreve** o `Produtos.xlsx` com os resultados. Mantenha uma cópia da planilha original se quiser preservá-la.
- A coluna `preco` mistura o valor `0` (livros gratuitos do Gutenberg) com textos como `£30.81` (Books to Scrape).
- No Books to Scrape a comparação é feita apenas pelo **título** (trecho do nome em minúsculas); o autor não é conferido nesse site.
- A coluna `categoria` é lida pelo código, mas ainda não é usada para filtrar a busca.
- O Books to Scrape é um site de treino, com livros e preços fictícios. O objetivo do projeto é praticar automação web.
- Use a automação com moderação e respeite os termos de uso dos sites pesquisados.
