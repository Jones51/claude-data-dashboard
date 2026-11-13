# Dashboard de Vendas com Claude (Focinho Pet Shop)

## Introdução

Este projeto é um estudo prático da criação de dashboards com o **Claude**, usando o recurso de dashboards de dados no Claude (Cowork). O caso de uso é a **Focinho**, uma loja online de produtos pet: a partir de uma planilha do Google Sheets com um pedido por linha (jan/2025 até hoje), o dashboard compara 2026 com o mesmo período de 2025.

O ponto central do estudo: o painel inteiro (consulta aos dados, regras de negócio, cálculos, gráficos e visual) foi construído **conversando em linguagem natural**, sem abrir uma ferramenta de BI. O dashboard fica ligado na planilha e se atualiza com um clique.

## Problema de negócio

A loja registra todos os pedidos numa planilha, mas a planilha sozinha não responde rápido às perguntas da gestão:

- Quanto vendemos e quanto sobrou de lucro em 2026, comparado com 2025?
- Quais canais e categorias puxam o resultado?
- Onde estamos perdendo dinheiro: cancelamento, devolução, desconto e comissão de marketplace?

O objetivo foi ter um painel vivo, ligado à planilha, que responda essas perguntas e mude todos os números juntos ao filtrar por canal.

## Metodologia

O dashboard foi construído em iterações, cada uma com um pedido em texto (os pedidos estão em [`estudo/PROMPTS.md`](estudo/PROMPTS.md)):

1. **Conexão com os dados:** conectar o Google Sheets ao Claude e apontar a planilha `Base_Pedidos_Focinho`. O Claude explorou as colunas (canal, categoria, status, desconto, valor, custo) antes de desenhar o painel.
2. **Primeira versão:** KPIs de vendas e lucro 2026 x 2025 (mesmo período, até o último pedido da planilha), gráfico mês a mês, perdas, tabelas por canal e categoria e filtro de canal.
3. **Visual profissional:** tema escuro com degradê, painéis em efeito vidro (glassmorphism), paleta azul/roxo e números grandes, sem mudar nenhum cálculo.
4. **Regras de negócio que não estão na planilha:**
   - faturamento e lucro contam só pedidos **Entregues**; cancelados e devolvidos ficam fora, mas somados à parte;
   - **15% de comissão** do Marketplace descontada do lucro dos pedidos desse canal.
5. **Ajustes finos:** remoção de textos, troca de título e rótulos direto no editor do painel.
6. **Verificação:** a cada mudança, o próprio painel reporta se rodou sem erro (checagem automática da página), e os erros encontrados foram corrigidos na hora.

## Habilidades e tecnologias

- **Claude (Cowork) + dashboards de dados:** criação do painel por conversa, consulta ao vivo, cálculos rastreáveis (cada número mostra de onde veio).
- **Conector Google Sheets:** leitura ao vivo da aba `Pedidos` (`get_values`, faixa `Pedidos!A1:O`).
- **JavaScript + D3.js:** cálculos (`dash.calc`), transformação dos dados (`dash.loader`) e gráficos (linhas, barras, tooltips).
- **HTML/CSS:** layout responsivo, tema escuro, efeito vidro com `backdrop-filter`.
- **Modelagem de negócio:** comparação de períodos equivalentes (YTD), margem, ticket médio, taxas de cancelamento, devolução e desconto, comissão por canal.

## Estrutura do repositório

```
claude-data-dashboard/
├── dashboard/
│   ├── index.html             # página do dashboard (HTML, CSS, cálculos e gráficos)
│   ├── dataset_pedidos.json   # definição da consulta ao vivo no Google Sheets
│   └── config.json            # título, idioma e filtro padrão
├── estudo/
│   └── PROMPTS.md             # os pedidos feitos ao Claude, em ordem, e o que cada um mudou
├── images/                    # prints do dashboard
└── README.md
```

> O `index.html` roda dentro do dashboard do Claude, que fornece `dash` (dados, filtros, cálculos) e `d3`. Ele não abre sozinho no navegador; está aqui para estudo do código.

## Como cada número é calculado

- **Período:** de 1º de janeiro até a data do último pedido de 2026 na planilha, comparado com as mesmas datas de 2025.
- **Faturamento:** soma do Valor Total (já com desconto) dos pedidos com status Entregue.
- **Lucro:** faturamento − custo dos produtos − 15% de comissão sobre os pedidos entregues no Marketplace. Não inclui frete, impostos ou despesas fixas.
- **Cancelamentos e devoluções:** valor dos pedidos com esse status, fora do faturamento. A taxa é sobre o valor de todos os pedidos do período.
- **Descontos:** soma da coluna Desconto nos pedidos entregues. A taxa é sobre o preço cheio.

## Insights

Até 20/10/2026, comparado com o mesmo período de 2025:

- **Faturamento de R$ 149,7 mil (+32,2%)** e **lucro de R$ 54,9 mil (+26,4%)**. O lucro cresce menos que o faturamento: a margem está caindo.
- **Perdas de R$ 18,1 mil (+71,8%)** com cancelamentos, devoluções e descontos. Descontos e cancelamentos foram os que mais cresceram; as devoluções ficaram estáveis.
- **O Site é o canal que mais dá lucro.** O Marketplace é o que mais fatura, mas a comissão de 15% leva cerca de um terço do lucro dele, e a margem cai para ~27% contra 42–45% dos outros canais (dados de 30/09/2026).
- **App e Marketplace são os que mais crescem** (~50% em lucro). O WhatsApp tem a maior taxa de cancelamento.
- **Acessórios** têm a maior taxa de devolução entre as categorias (~12% do valor pedido).

## Confiabilidade dos dados

- A consulta é **ao vivo**: cada vez que o painel é aberto ou atualizado, ele lê a planilha de novo. Pedidos novos aparecem com um clique em atualizar.
- Cada número do painel é ligado à sua origem: dá para clicar e ver a consulta, a transformação e a fórmula que geraram o valor.
- A cada mudança, a página roda uma checagem automática (erros de código, números que não batem com a fonte) e o Claude corrige o que aparece antes de entregar.
- Quem abre o painel usa o próprio acesso à planilha.

## O que aprendi

- Descrever o **problema de negócio**, e não o gráfico, gera um painel melhor: o Claude escolheu KPIs, comparações e cortes a partir das perguntas.
- Regras que não estão nos dados (status válido, comissão) entram como texto e viram cálculo, com a definição registrada no próprio painel.
- Mudanças só de visual não mexem nos números, porque os cálculos ficam separados do desenho.
- Ainda vale revisar: o painel mostra a definição de cada número para que a regra possa ser conferida.

## Projeto final

Adicione os prints em [`images/`](images/) e referencie aqui, por exemplo:

```markdown
![Visão geral](images/visao-geral.png)
![Canais e categorias](images/canais-categorias.png)
```
