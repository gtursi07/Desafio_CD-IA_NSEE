# Desafio de Estágio — CD&IA em Saúde Pública

Solução do desafio técnico para a vaga de estágio em **Ciência de Dados & Inteligência Artificial** no **Núcleo de Sistemas Eletrônicos Embarcados (NSEE) — Instituto Mauá de Tecnologia**.

---

## Objetivo

Explorar, limpar, preparar e pré-processar o conjunto de dados do **Registro Hospitalar de Câncer de São Paulo (RHC/SP)**, deixando-o pronto para aplicação em um modelo de machine learning com foco em predição de óbito em pacientes com câncer de pulmão.

---

## Dataset

- **Fonte:** Registro Hospitalar de Câncer de São Paulo (RHC/SP)
- **Formato original:** `.dbf` (formato legado amplamente utilizado em bases governamentais brasileiras)
- **Tamanho original:** 1.344.819 linhas × 114 colunas

---

## Estrutura do Notebook

### 1. Importação de Bibliotecas
- `pandas`, `numpy`, `dbfread`, `sklearn`

### 2. Carregamento dos Dados
- Leitura do arquivo `.dbf` com `encoding="latin1"`
- Conversão para DataFrame e salvamento em `.parquet` para otimizar carregamentos futuros

### 3. Preparação dos Dados
Aplicação dos filtros conforme especificado no desafio:

| Etapa | Descrição | Pacientes restantes |
|---|---|---|
| 1 | Topografia de pulmão (CID-10: C34) | 62.471 |
| 2 | Residência em SP | 57.346 |
| 3 | Confirmação microscópica (BASEDIAG = 3) | 55.551 |
| 4 | Retirada de categorias 0, X e Y de ECGRUP | 49.633 |
| 5 | Retirada de pacientes com Hormonioterapia E TMO | 49.633 |
| 6 | Ano de diagnóstico até 2019 | 38.998 |
| 7 | Idade >= 20 anos | 38.985 |

- **Etapa 8:** Cálculo e codificação de intervalos de tempo (`CONSDIAG`, `DIAGTRAT`, `TRATCONS`)
- **Etapa 9:** Extração de números das colunas `DRS` e `DRS_INST` via regex
- **Etapa 10:** Criação da coluna binária de óbito a partir de `ULTINFO`
- **Etapa 11:** Remoção de colunas irrelevantes para o modelo

### 4. Pré-processamento
- **Conversão de tipos:** colunas `object` com valores numéricos convertidas para `int64`
- **Remoção de `INSTITU`:** identificador de hospital sem valor preditivo
- **Encoding ordinal:** `ECGRUP` (I→1, II→2, III→3, IV→4)
- **Encoding nominal:** `TOPO` via `pd.get_dummies`
- **Escalonamento:** `StandardScaler` — média 0 e desvio padrão 1 em todas as features
- **Separação:** features `X` e variável alvo `y` (óbito)

**Dataset final:** 38.985 linhas × 29 features + 1 variável alvo

---

## Tecnologias

- Python 3.13
- pandas
- numpy
- dbfread
- scikit-learn

---

## Como executar

1. Clone o repositório
2. Instale as dependências:
```bash
pip install pandas numpy dbfread scikit-learn pyarrow
```
3. Coloque o arquivo `RHC_2000_2025_GERAL.DBF` na raiz do projeto
4. Execute o notebook `desafio_CDIA.ipynb` do início ao fim
