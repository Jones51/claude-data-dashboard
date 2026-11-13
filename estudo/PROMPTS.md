# Pedidos feitos ao Claude

Os pedidos abaixo foram usados, em ordem, para construir o dashboard. Cada um vem com o que mudou no painel.

## 1. Primeira versão

> Tenho uma loja online de produtos pet, a Focinho. A planilha Base_Pedidos_Focinho, no meu Google Drive, tem cada pedido de janeiro de 2025 até hoje, um por linha.
>
> Quero entender como está 2026 comparado com o mesmo período de 2025:
> - quanto a gente vendeu e quanto sobrou de lucro;
> - quais canais e categorias puxam o resultado;
> - onde a gente está perdendo dinheiro: cancelamento, devolução e desconto.
>
> Coloca um filtro de canal que mude todos os números juntos. E deixa o dashboard ligado na planilha: quando entrarem pedidos novos, eu só quero clicar em atualizar.

**Resultado:**

- O Claude pediu para conectar o Google Sheets e depois leu a estrutura da planilha.
- Criou uma consulta ao vivo da aba `Pedidos` e cálculos para:
  - o período comparado;
  - os KPIs;
  - a série mensal;
  - os resultados por canal e por categoria.
- Montou cards, um gráfico de linhas, a seção de perdas, as tabelas e o filtro de canal.
- Corrigiu dois erros apontados pela checagem automática da página.

## 2. Visual profissional

> Agora deixa esse dashboard bonitão, com cara de painel profissional:
> - fundo escuro, com um degradê suave de cores atrás de tudo;
> - os cartões e os gráficos em painéis que parecem de vidro: meio transparentes, com borda fina e um brilho leve;
> - poucas cores que combinem entre si, como azul e roxo, pra destacar o que importa;
> - números grandes e fáceis de ler.
>
> Muda só o visual: os números, os filtros e as contas continuam iguais.

**Resultado:** o CSS foi reescrito e os gráficos ganharam degradês, brilho e área sombreada. Nenhum cálculo mudou.

## 3. Regras de negócio

> Duas regras da loja que não estão na planilha:
> 1. Faturamento e lucro contam só pedidos com status Entregue. Cancelado e devolvido saem dessas contas, mas quero ver quanto dinheiro eles somam.
> 2. O marketplace fica com 15% de comissão de cada venda. Desconta essa comissão do lucro dos pedidos do Marketplace.
>
> Depois me diz qual canal dá mais lucro de verdade.

**Resultado:**

- Os cálculos passaram a usar só pedidos Entregues.
- A comissão do Marketplace virou uma constante, usada nos cálculos e mostrada no texto do painel.
- Entraram um card de comissão e as colunas de lucro por pedido e de comissão.
- A tabela de canais passou a ser ordenada por lucro.

**Resposta:** o Site é o canal que mais lucra. O Marketplace fatura mais, mas tem margem bem menor por causa da comissão.

## 4. Ajustes finos

> Remover esse subtítulo

**Resultado:** o parágrafo de resumo saiu do topo. O título e os rótulos dos cards também foram trocados direto no editor do painel, sem pedido ao Claude.
