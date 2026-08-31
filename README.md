# Asset-FMTool — Gestão Financeira (Protótipo)

Ferramenta de teste para gestão financeira mensal: registas rendimentos (fixos e extra) e saldos de contas, e a poupança é calculada automaticamente pela variação de património — sem precisares de categorizar despesas.

- **App**: [`index.html`](./gestao-financeira-prototipo.html) — HTML/CSS/JS autossuficiente, sem dependências de servidor. Os dados ficam guardados no `localStorage` do browser (com exportação/importação de backup em JSON e exportação em CSV).
- **Documentação**: ver [`DOCUMENTACAO.md`](./DOCUMENTACAO.md) para a lógica de cálculo, estrutura de dados, decisões de design e próximos passos planeados.

## Acesso online

Este repositório está configurado para publicar automaticamente via **GitHub Pages** a cada push para `main`. Depois do primeiro deployment, a ferramenta fica acessível em:

```
https://dblxpt.github.io/asset-fmtool/
```

(Pode levar alguns minutos após o primeiro push para o link ficar ativo — ver o separador *Actions* do repositório para o estado do deployment.)

## Estado

Protótipo de teste, validado em chat. Próximos passos planeados: recriar a mesma lógica em Google Sheets e, mais tarde, evoluir para uma aplicação web com contas de utilizador registadas e dados em back-end.
