# RefIA

Padrão leve para trabalhar com assistentes de IA (Claude Code, Cursor, etc.) em projetos de desenvolvimento: um conjunto de **regras de comportamento** e um formato de **memória persistente entre sessões**, versionado junto do código.

Resolve dois problemas comuns:
1. A memória de sessão de um agente de IA é efêmera — decisões, pendências e descobertas se perdem quando a sessão fecha ou troca de contexto.
2. Sessões diferentes (ou pessoas diferentes usando IA no mesmo repositório) não têm um canal formal para deixar perguntas, avisos ou pendências uns para os outros.

## Os dois arquivos do padrão

- **[`REGRAS_GERAIS.md`](REGRAS_GERAIS.md)** — regras de comportamento válidas para qualquer sessão de IA trabalhando no projeto (quando confirmar antes de agir, como responder, como lidar com testes em dados reais, etc.).
- **[`SISTEMA_REFIA.md`](SISTEMA_REFIA.md)** — especificação da pasta `.refia/`: os 6 arquivos que compõem a memória persistente do projeto (briefing, changelog, pendências, debate, apontamentos, boas práticas), formato de cada um e quando usar cada um.

## Como adotar num projeto existente

1. Copie a pasta [`templates/.refia/`](templates/.refia) deste repositório para a raiz do projeto alvo:
   ```bash
   cp -r templates/.refia /caminho/do/projeto/.refia
   ```
2. Versione normalmente (`.refia/` **não** é gitignorado — é documentação viva, não estado descartável).
3. No arquivo de instruções do seu assistente de IA (`CLAUDE.md`, `AGENTS.md`, `.cursor/rules`, etc.), referencie este padrão — cole o conteúdo de `REGRAS_GERAIS.md` e `SISTEMA_REFIA.md`, ou linke este repositório se o assistente conseguir buscar URLs.
4. Ajuste `REGRAS_GERAIS.md` às particularidades do projeto (idioma, stack, convenções próprias) — é um ponto de partida, não uma camisa de força.

## Quando usar sufixo por área (monorepo)

Por padrão, cada arquivo de `.refia/` é único (`briefing.md`, `changelog.md`, ...), porque a maioria dos repositórios é um projeto só. Se o repositório for um **monorepo** com áreas bem distintas (ex: `backend/` e `frontend/` no mesmo repo, cada uma com dono/contexto próprio), use sufixo por área: `briefing_backend.md`, `briefing_frontend.md`, etc. — ver `SISTEMA_REFIA.md`.

## Origem

Extraído e generalizado a partir do sistema de memória usado internamente na Sistemando (`_referencias/`), depois de rodar por várias semanas coordenando sessões de IA em múltiplos projetos.
