# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Para que serve no Edu? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores, ou seja, dar continuidade ao atendimento de forma mais eficiente. |
| `perfil_usuario.json` | JSON | Personalizar as explicações sobre as dúvidas e necessidades de aprendizado do cliente. |
| `produtos_financeiros.json` | JSON | Conhecer os produtos disponíveis para que eles possam ser ensinados ao cliente. |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usar essas informações de forma didática. |
| `cartoes.json` | JSON  | Identificar limites, faturas e comportamento de uso do cartões. |
| `credito_automatico.json` |JSON  | Identificar gastos ativos, tipo de conta, saldos. |
| `generos.json` | JSON | Catégorizar cada gasto em sua classe.|


---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados modificados foram adaptados para representar comportamentos financeiros comuns, como gastos recorrentes, variação mensal de despesas, picos de consumo 'e generos (alimentação, transporte, lazer, moradia). Também foram incluídos campos adicionais, como genero da despesa, tipo de transação 'PIX, débito, crédito' e recorrência, para facilitar a geração de respostas pelo agente.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.
Existem duas possibilidades, injetar os dados diretamente no prompt (ctrl + c, ctrl + v) ou carregar os arquivos via código, como no exemplo abaixo:



import pandas as pd
    import json


``` carregamento dos arquivos CSV ```
    transacoes = pd.read_csv('data/transacoes.csv')
    historico_interacoes = pd.read_csv('data/historico_interacoes.csv')


```Carregamento dos arquivos JSON ```

with open('data/perfil_usuario.json', 'r', encoding='utf-8') as f:
    perfil_cliente = json.load(f)

with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f: 
    contas = json.load(f)

with open('data/cartoes.json', 'r', encoding='utf-8') as f:
        cartoes = json.load(f)

with open('data/generos.json', 'r', encoding='utf-8') as f:
        categorias = json.load(f)

with open('data/credito_automatico.json', 'r', encoding='utf-8') as f:
        assinaturas = json.load(f)


Esses dados podem ser utilizados para:
    - Montar o contexto do prompt
    - Analisar gastos e padroes
    - Gerar informações importantes
    - Mantes o historico do usuário

```
## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo de contexto montado abaixo, se baiseia nos dados originais da base de conhecimento, mas os sintetiza deixando apenas as informações mais relevantes, otimizando assim o consumo de tokens. Entretanto, vale lembrar que mais importante do que economizar tokens, é ter todas as informações relevantes disponíveis em seu contexto.

```
DADOS DO CLIENTE:
- Nome: João Silva
- Perfil: Moderado
- Objetivo: Construir reserva de emergência
- Reserva atual: R$ 10.000 (meta: R$ 15.000)

RESUMO DE GASTOS:
- Moradia: R$ 1.380
- Alimentação: R$ 570
- Transporte: R$ 295
- Saúde: R$ 188
- Lazer: R$ 55,90
- Total de saídas: R$ 2.488,90

PRODUTOS DISPONÍVEIS PARA EXPLICAR:
- Tesouro Selic (risco baixo)
- CDB Liquidez Diária (risco baixo)
- LCI/LCA (risco baixo)
- Fundo Imobiliário - FII (risco médio)
- Fundo de Ações (risco alto)
```
