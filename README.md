# Dashboard — Fluxo de Atendimentos

Painel mensal dos atendimentos comerciais do escritório Borges Macedo Advocacia, lido em
tempo real da planilha **Fluxo de Atendimentos [NOVA]** (`1L01ullcCOWFsUsWlBg8ofV42luRG5-NmyyGfXMJelOc`,
aba `Fluxo de Atendimentos`) e, para faturado e recebido, da planilha **Fluxo de Clientes 2026**
(aba `DADOS`). *(Até 28/09/2026 a origem era a planilha "CRM - BM Advocacia".)*

**Colunas são achadas pelo nome do cabeçalho**, nunca pela posição: a planilha ganha e renomeia
colunas com frequência, e ler por letra faria o painel exibir o campo errado sem avisar. Campo que
não existe simplesmente não vira coluna/medida, e os essenciais que faltarem são apontados em
"Pontos de atenção".

**Acesso geral.** Os três perfis da Central — Administração, Lideranças e Equipe — têm cofre
neste painel. Entra-se pela [Central de Dashboards](https://borgesmacedoadvocacia.github.io/).

## Regras de cálculo

- **Base**: só leads com **Data do Agendamento** a partir de 01/01/2026.
- **Mês de cada medida = Data do Atendimento**. O menu permite escolher um ou mais meses
  (os cartões somam os meses marcados) e um ou mais produtos jurídicos.
- Leads com data de agendamento mas sem data de atendimento ficam fora das medidas e são
  apontados em "Pontos de atenção".
- A planilha tem **uma coluna de situação só** (`Status de Fechamento`): *Atendimento Agendado*,
  *Remarcar*, *Em Negociação*, *Cliente Ganho* e *Lead Perdido*. Uma reunião conta como
  **realizada** quando já teve desfecho — em negociação, ganha ou perdida.

| Cartão | Como é calculado |
|---|---|
| Reuniões agendadas | todos os atendimentos com data no mês, qualquer situação |
| Reuniões realizadas | situação **Em Negociação**, **Cliente Ganho** ou **Lead Perdido** |
| Clientes em negociação | situação **Em Negociação** |
| Valores em negociação | soma de **Valor da Proposta (Honorários Iniciais)** dos leads em negociação; o cartão avisa quantos estão sem valor |
| Reuniões para remarcar | situação **Remarcar** |
| Leads perdidos | situação **Lead Perdido** (a lista mostra o *Motivo de Perda do Lead*) |
| Ticket médio das propostas | média dos honorários iniciais entre as propostas com valor no mês |
| Clientes ganhos (CRM) | situação **Cliente Ganho** (pelo mês do atendimento) |

> O **cliente ganho não carrega proposta** nesta planilha — o valor fechado vive no Fluxo de
> Clientes como honorários. Por isso o ticket médio é calculado sobre as propostas em negociação
> e perdidas, e a lacuna apontada em "Pontos de atenção" considera só esses casos.

Todo cartão abre a lista dos leads por trás do número (nome, produto, SDR, closer, datas,
situação, proposta, telefone; nas negociações também *Situação da Negociação (Closer)*, *ligação*
e *FUP*; nos perdidos, o motivo). Colunas que a planilha vier a preencher — *Etapa do Funil*,
*Detalhes do Produto Jurídico*, *Honorários de Êxito* — aparecem sozinhas nas listas.

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
