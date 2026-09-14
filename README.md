# Dashboard — Fluxo de Atendimentos

Painel mensal dos atendimentos comerciais do escritório Borges Macedo Advocacia, lido em
tempo real da planilha **CRM - BM Advocacia** (aba `Fluxo de Atendimentos`) e, para faturado
e recebido, da planilha **Fluxo de Clientes 2026** (aba `DADOS`).

**Acesso geral.** Os três perfis da Central — Administração, Lideranças e Equipe — têm cofre
neste painel. Entra-se pela [Central de Dashboards](https://borgesmacedoadvocacia.github.io/).

## Regras de cálculo

- **Base**: só leads com **Data do Agendamento (F)** a partir de 01/01/2026.
- **Mês de cada medida = Data do Atendimento (H)**. O menu permite escolher um ou mais meses
  (os cartões somam os meses marcados) e um ou mais produtos jurídicos (coluna C).
- Leads com data de agendamento mas sem data de atendimento ficam fora das medidas e são
  apontados em "Pontos de atenção".

| Cartão | Como é calculado |
|---|---|
| Reuniões agendadas | todos os atendimentos com data no mês, qualquer status (Realizado, No Show, Remarcar, Agendado) |
| Reuniões realizadas | Status do Atendimento = **Realizado** |
| Clientes em negociação | Status de Fechamento = **Em Negociação** |
| Valores em negociação | soma da coluna **Valor da Proposta (M)** dos leads em negociação; o cartão avisa quantos estão sem valor |
| Reuniões para remarcar | Status do Atendimento = **Remarcar** |
| No-show | Status do Atendimento = **No Show** |
| Ticket médio das propostas | média de Valor da Proposta entre as propostas com valor no mês |
| Clientes ganhos (CRM) | Status de Fechamento = **Cliente Ganho** (pelo mês do atendimento) |

Todo cartão abre a lista dos leads por trás do número (nome, produto, SDR, closer, datas,
status, proposta, telefone; nas negociações também próximo contato e o que está travando).

## Projeção de recebimento — últimos 30 e 60 dias

Janela **móvel**, contada da data do atendimento até hoje (não depende do mês marcado no menu;
o filtro de produto vale). Botões de 30 e 60 dias, campo livre para outra janela, e uma tabela
30 × 60 lado a lado. A taxa de conversão ideal e a janela escolhida ficam salvas no navegador.

```
Oportunidades      = soma das propostas dos atendimentos REALIZADOS na janela
Clientes esperados = reuniões realizadas na janela × taxa de conversão ideal
Potencial          = clientes esperados × ticket médio da janela      ← sempre pelo ticket médio
Faturado           = honorários iniciais dos fechamentos da janela (Fluxo de Clientes, data do fechamento)
Recebido           = idem, só com PAGAMENTO CONFIRMADO = Sim
Falta              = potencial − faturado
```

Exemplo (30 dias): 75 reuniões realizadas × 40% = 30 clientes × ticket médio R$ 3.627 = R$ 108.810
de potencial; faturado R$ 52.470 → faltam R$ 56.340 em oportunidades reais.

Quando um produto é filtrado, o Fluxo de Clientes é casado por palavras-chave
(`Rev. Plano de Saúde` ↔ `Revisional de Plano de Saúde`; `Rev. Financiamento Imobiliário` ↔
`Revisional de Contratos Bancários`; `Neg. Procedimento Médico` ↔ `Neg. de Proc. Méd.`).

## Estrutura

- `index.html` — painel completo (cofres, guarda da central, leitura via Sheets API v4, gráficos Chart.js).
- Publicação: GitHub Pages via Actions (`.github/workflows/pages.yml`), a cada push em `main`.
- Atualização automática dos dados a cada 5 minutos enquanto a aba estiver aberta.
