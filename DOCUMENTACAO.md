# Gestão Financeira — Protótipo Mensal (HTML)

**Ficheiro associado:** `gestao-financeira-prototipo.html`
**Última atualização:** 31 de agosto de 2026
**Estado:** Protótipo validado em chat, autossuficiente (HTML/CSS/JS, sem dependências de servidor). Próximo passo planeado: recriar em Google Sheets, e mais tarde numa app web com contas de utilizador.

---

## 1. Objetivo do projeto

Ferramenta para ensinar e ajudar qualquer pessoa (público misto, com percurso progressivo) a gerir dinheiro e a começar a investir, passo a passo. Esta fase do projeto foca-se **apenas** na base: um registo mensal simples que calcula automaticamente quanto se poupou, sem exigir categorização de despesas.

## 2. Lógica central (importante manter ao migrar para Sheets/Web)

Em vez de pedir à pessoa que categorize cada despesa (o que ninguém mantém a longo prazo), o método usa a **variação do saldo das contas** como proxy do que foi gasto:

```
Total Rendimentos = soma de Rendimentos Fixos + soma de Rendimentos Extra
Total Contas       = soma dos saldos de todas as contas nesse mês
Variação           = Total Contas (mês atual) − Total Contas (mês anterior)
Gasto Total        = Total Rendimentos − Variação
Poupança (€)       = Variação
Poupança (%)       = Poupança € ÷ Total Rendimentos × 100
```

- No **primeiro mês** registado, não há mês anterior para comparar, por isso Variação/Gasto Total/Poupança aparecem como "—" (null). A partir do segundo mês, o cálculo funciona normalmente.
- A "Despesa Extra" (ex: IRS a pagar) já fica automaticamente refletida no cálculo acima (via a variação de saldo) — não precisa de ser somada à parte.

## 3. Estrutura de dados (colunas)

Quatro grupos de colunas, todos **editáveis e expansíveis** pelo utilizador (renomear, adicionar, remover):

1. **Rendimentos Fixos** — recorrentes (ex: Salário líquido, Subsídio Alimentação, Outros)
2. **Rendimentos Extra** — pontuais (ex: reembolso de IRS, venda de algo)
3. **Despesa Extra** — pontuais (ex: IRS a pagar)
4. **Contas** — inclui contas à ordem e a prazo (os juros de a prazo são normalmente demasiado reduzidos para entrarem no cálculo de rentabilidade de investimentos — servem só para refletir o património total)

Cada mês (linha) tem uma data completa (`AAAA-MM-DD`). Ao adicionar um novo mês, sugere-se automaticamente o dia 15 do mês seguinte ao último, mas é editável.

**Transporte automático de valores:** ao criar um novo mês, o Salário líquido, o Subsídio Alimentação e os saldos das Contas são pré-preenchidos com os valores do mês anterior (editável). Os restantes campos começam vazios/zero. Esta lógica está fixa no código (array `state.recurring`), sem controlo visual — foi assim por pedido explícito do utilizador (o ícone de alternar esta opção foi removido).

## 4. Funcionalidades implementadas

- **Tabela horizontal** (não em cartões — decisão explícita do utilizador para "maior aproveitamento de espaço", mesmo em mobile).
- **Mobile-first**: o design compacto é o único, sem media queries de desktop.
- Campos com valor 0 aparecem **vazios** visualmente (continuam a valer 0 nos cálculos).
- **Aviso de data fora de ordem** (linha destacada a laranja + ícone ⚠) se um mês tiver data anterior ao mês de cima.
- Só é possível **apagar o último mês** da tabela, e só se todos os valores de input (exceto a data) estiverem a zero — botão sempre visível, mas mostra aviso explicativo se as condições não estiverem reunidas.
- **Backup em JSON** (exporta/importa fielmente todo o estado) e **exportação em CSV** (para abrir em Excel/Numbers/Google Sheets; usa `;` como separador, compatível com Excel em português).
- **Botão "Limpar tudo"** com aviso de confirmação (reduz a tabela a uma única linha vazia, mantendo as categorias configuradas).
- **Gravação automática** no `localStorage` do browser (aviso "Guardado automaticamente às HH:MM" visível).
- Tooltips clicáveis (ⓘ) em vez de `title` nativo — funcionam em mobile, ao contrário do title do browser.
- Sombra de scroll horizontal (indica visualmente que há mais colunas por ver).

## 5. Identidade visual

Paleta e tipografia extraídas do style guide da marca em `https://eltziweb.vercel.app/en/style-guide`:

**Cores:**
- `--ink: #0F1B38` (texto/títulos)
- `--ink-soft: #394658` (texto secundário)
- `--primary: #2C4FDE` (azul forte — usado para foco, sublinhado do título, acentos)
- `--accent: #668CE8` (azul claro — hovers)
- `--highlight: #EC9140` (laranja — botão principal "+ Adicionar mês")
- `--navy: #1B2F4D`

**Tipografia:**
- Display/títulos: **Montserrat** (700/800, tracking -3%)
- Corpo/UI: **Public Sans** (400/500/600), incluindo os campos numéricos (com `font-variant-numeric: tabular-nums` para alinhamento)

Verde/vermelho mantidos à parte da paleta da marca, só para semântica financeira universal (poupança positiva/negativa).

## 6. Decisões e histórico relevante (para não repetir perguntas already resolvidas)

- **Sem "Mês 0"/Saldo Inicial**: foi testado e depois removido — considerou-se desnecessário, já que o primeiro mês simplesmente não tem cálculo de variação.
- **Sem reordenação de meses** (setas ↑↓ removidas) — os meses ficam pela ordem em que são criados.
- **Backup CSV vs JSON**: JSON é o único formato reimportável (preserva categorias exatamente); CSV é só para consumo externo.
- O ícone de transporte automático de valores (🔁) foi testado e depois **removido da interface**, mas a lógica por trás manteve-se fixa nos valores por defeito (salário, subsídio, contas).

## 7. Próximos passos planeados

1. Recriar esta mesma lógica em **Google Sheets** (via Claude Code), mantendo a estrutura de colunas e fórmulas equivalentes às daqui.
2. Mais tarde, evoluir para uma **aplicação web com contas de utilizador registadas**, usando uma estrutura de dados normalizada (tabela de "lançamentos" longa, em vez do formato largo usado neste protótipo) — mais adequada para uma base de dados multi-utilizador.
3. Módulos futuros da ferramenta educativa mais alargada (definidos no início da conversa, ainda não desenvolvidos): fundamentos (orçamento, fundo de emergência, dívida), preparação para investir (juro composto, perfil de risco), investir na prática (classes de ativos, alocação, simulador), manutenção (rebalanceamento).

---

*Este ficheiro documenta o estado do protótipo à data acima. Para o código completo, ver `gestao-financeira-prototipo.html` na mesma pasta.*
