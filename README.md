# Dashboard — Fluxo de Atendimentos

Painel mensal dos atendimentos comerciais do escritório Borges Macedo Advocacia, lido em
tempo real da planilha **CRM - BM Advocacia** (aba `Fluxo de Atendimentos`) e, para faturado
e recebido, da planilha **Fluxo de Clientes 2026** (aba `DADOS`).

**Acesso exclusivo das lideranças.** Os perfis Administração e Lideranças têm cofre neste
painel; a credencial do perfil Equipe não decifra nada. Entra-se pela
[Central de Dashboards](https://borgesmacedoadvocacia.github.io/).

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

## Projeção de recebimento do mês

```
Oportunidades      = soma das propostas dos atendimentos REALIZADOS
Clientes esperados = reuniões realizadas × taxa de conversão ideal (campo manual, salvo no navegador)
Potencial          = clientes esperados × ticket médio            ← sempre pelo ticket médio
Faturado           = honorários iniciais dos fechamentos do mês (Fluxo de Clientes, data do fechamento)
Recebido           = idem, só com PAGAMENTO CONFIRMADO = Sim
Falta              = potencial − faturado
```

Exemplo: 37 reuniões realizadas × 40% = 14,8 clientes × ticket médio R$ 3.107 = R$ 45.989 de
potencial; faturado R$ 21.000 → faltam R$ 24.989 em oportunidades reais.

Quando um produto é filtrado, o Fluxo de Clientes é casado por palavras-chave
(`Rev. Plano de Saúde` ↔ `Revisional de Plano de Saúde`; `Rev. Financiamento Imobiliário` ↔
`Revisional de Contratos Bancários`; `Neg. Procedimento Médico` ↔ `Neg. de Proc. Méd.`).

## Estrutura

- `index.html` — painel completo (cofres, guarda da central, leitura via Sheets API v4, gráficos Chart.js).
- Publicação: GitHub Pages via Actions (`.github/workflows/pages.yml`), a cada push em `main`.
- Atualização automática dos dados a cada 5 minutos enquanto a aba estiver aberta.
