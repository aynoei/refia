# Sistema `.refia/`

Especificação da pasta `.refia/` — a memória persistente de um projeto entre sessões de IA. Complementa o `README.md` do projeto e o `CLAUDE.md`/`AGENTS.md` (que documentam arquitetura/estado atual): aqui fica o que é histórico, pendência formal ou recado pontual entre sessões, não "como o sistema funciona hoje".

## Por que existe

Memória de sessão de um agente de IA é efêmera e isolada — o que foi combinado numa conversa não sobrevive ao fim dela, e não é visível para outra sessão (outra pessoa, outro dia, outro agente) trabalhando no mesmo projeto. Sem um lugar real em disco (versionado com o código) para isso, decisão, pendência e descoberta técnica se perdem ou ficam presas na cabeça de quem participou daquela conversa específica.

## Cabeçalho padrão

Todo arquivo do `.refia/` — inclusive vazio — começa com H1:
```
# {TIPO EM MAIÚSCULO}
```
Arquivo recém-criado sem conteúdo traz uma linha indicando isso (`Sem entradas ainda.`).

## Os 6 arquivos

### `briefing.md`
**Retrato do estado atual** do projeto — não histórico, é sobrescrito a cada atualização relevante. Serve pra quem abre o projeto (pessoa ou sessão de IA) context-switchar rápido sem ler o changelog inteiro. Deve responder: onde o projeto está agora, convenções/gotchas técnicas, o que uma sessão nova precisa saber antes de mexer.

### `changelog.md`
Histórico cronológico do que foi feito. **Um arquivo só, que cresce** — não criar arquivo novo por data. Entradas mais novas sempre no **topo**, cada entrada com data.

```markdown
## 2026-08-28
Resumo do que mudou, por quê, e o que ficou pendente (se algo).
```

### `pendencias.md`
Tabela única de tudo em aberto no projeto. Colunas: `ID`, `Status`, `Descrição`, `Solução`, `Aberto em`, `Resolvido em`. ID sequencial pro arquivo inteiro, nunca reaproveitado; nunca apagar linha, só mudar status; solução resumida (detalhe grande fica no `changelog.md`, citar de lá). Nasce/fecha item só como consequência real de uma entrada no `changelog.md` ou de uma decisão no `debate.md` — nunca uma fonte paralela desconectada do que de fato aconteceu.

### `debate.md`
Canal de pergunta/aviso **entre sessões** (ou entre áreas, se o projeto tiver mais de uma) — em vez de supor ou agir sobre incerteza. **Um arquivo só, que cresce**, entrada nova sempre no topo. Duas naturezas de entrada, cada uma com seu próprio ciclo de status:

- **`[PERGUNTA]` → `[RESPONDIDA]`** — espera resposta de conteúdo. Quem responde edita o **mesmo bloco** (não cria um novo), troca o status e preenche a resposta.
- **`[AVISO]` → `[CIENTE]`** — só informa, não espera resposta de conteúdo, só confirmação de leitura. Quem lê edita o **mesmo bloco**, troca o status e registra a data em que tomou ciência.

Todo bloco tem `#ID` obrigatório logo depois do status — sequencial pro arquivo inteiro, nunca reaproveitado.

```markdown
## [PERGUNTA] #3 2026-08-28 — {quem pergunta} → {quem deve responder}
**Pergunta:** ...

---
```
```markdown
## [RESPONDIDA] #3 2026-08-28 → 2026-08-29 — {quem pergunta} → {quem respondeu}
**Pergunta:** ...
**Resposta:** ...

---
```

### `apontamentos.md`
Canal **usuário → sessão** (diferente do `debate.md`, que é sessão↔sessão): quem usa o projeto registra ali qualquer coisa que notou — pergunta, comentário, pedido — sem precisar parar o fluxo em andamento pra discutir na hora. A sessão seguinte lê no início do trabalho, resolve (ou traz de volta pra discussão se precisar de decisão) e marca.

Formato: bullet simples, agrupado por seção `##` (a seção identifica de onde veio o apontamento — página, módulo, arquivo etc., livre). Ao resolver, acrescenta `- (dd/mm/aaaa)` no final da própria linha:

```markdown
## Nome do módulo/área
- Apontamento ainda em aberto.
- Apontamento já resolvido, com a marca no final da linha - (28/08/2026).
```

### `boas_praticas.md`
**Conhecimento já implementado/validado** — padrão que funciona, gotcha de ferramenta, forma de normalizar dado, armadilha que custaria tempo de novo. Não é fila de ações — se uma boa prática ainda tem "algo a fazer" associado, a entrada aponta para onde aquilo aguarda (`pendencias.md` ou `debate.md`) com a marca `aguarda: {arquivo}`; quando resolver, vira `(aguarda → resolvido dd/mm/aaaa)`, mas a boa prática nunca sai daqui.

Formato: bullet simples agrupado por seção `##` (categoria/área do conhecimento).

## Quando usar cada um

- Terminou uma tarefa relevante → entrada no `changelog.md` (topo do arquivo).
- Mudou o estado geral de algo de um jeito que quem retomar o projeto precisa saber antes de começar → atualizar (sobrescrever a parte relevante) o `briefing.md`.
- Surgiu uma pendência nova, ou uma existente foi resolvida → atualizar a tabela em `pendencias.md`.
- Dúvida sobre algo que a sessão atual não tem contexto suficiente pra decidir sozinha → registrar em `debate.md` (`[PERGUNTA]`) em vez de supor.
- Recado/aviso pontual pra quem retomar o trabalho depois → registrar em `debate.md` (`[AVISO]`).
- Usuário notou algo durante o uso e quer registrar sem parar o fluxo atual → `apontamentos.md`.
- Descobriu/validou um padrão, gotcha ou armadilha que vale lembrar → `boas_praticas.md`, na hora do achado.

## Extensão opcional — snapshot de sessão

Para projetos onde sessões longas correm risco de crash/interrupção no meio, um arquivo adicional `_sessao_atual.md` (fora do padrão `{tipo}.md`, é descartável) pode registrar o contexto vivo da sessão em andamento — decisões recentes ainda não absorvidas nos arquivos reais, próximo passo combinado. Deve trazer sempre data/hora de início no topo (para não confundir sessões paralelas) e ser esvaziado/absorvido nos arquivos reais ao fim natural da sessão. Não é fonte de verdade de nada — só uma rede de segurança contra perda de contexto vivo.

## Monorepo — sufixo por área

Se o projeto for um monorepo com áreas bem distintas (dono/contexto próprio cada uma), use sufixo: `briefing_{area}.md`, `changelog_{area}.md`, etc. Cada arquivo com sufixo tem **uma área dona** — só ela escreve nele; outras áreas só leem. Único tipo compartilhado entre áreas continua sendo o `debate_{area}.md` (qualquer área pode adicionar uma entrada endereçada a outra; só a área endereçada resolve). `apontamentos_{area}.md` mantém a exceção de que o usuário escreve diretamente, não só a área dona.
