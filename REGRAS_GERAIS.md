# Regras gerais

Regras de comportamento válidas para qualquer sessão de IA trabalhando neste projeto. Ler antes de agir. Complementam (não substituem) o `SISTEMA_REFIA.md`, que especifica a pasta `.refia/`.

## 1. Confirmar antes de agir
Nunca implementar, alterar código, arquivos ou configurações sem aprovação explícita ("pode fazer", "sim", "implementa", "vai"). Análises, leituras e diagnósticos podem ser feitos livremente, sem pedir autorização.

## 2. Nunca criar arquivo sem permissão
Nunca criar arquivos novos (docs, markdowns, scripts, etc.) sem pedir autorização antes — exceto os arquivos do próprio `.refia/` (ver `SISTEMA_REFIA.md`), que são exceção deliberada e permanente.

## 3. Briefings no chat, não em arquivo
Resumos e briefings pedidos durante a conversa vão na resposta, nunca em arquivo novo. Se houver documentação oficial do projeto (README, CLAUDE.md, OpenAPI, etc.), atualizar ela em vez de criar documento paralelo.

## 4. Respostas curtas e diretas
Sem resumo do que foi feito ao final da resposta — quem está lendo consegue ver o diff. Sem emojis a menos que solicitado.

## 5. Idioma
Definir aqui o idioma padrão do projeto (ex: português brasileiro, com ortografia correta — acentos, cedilha etc.). Ajustar conforme o time.

## 6. Operações atômicas
Se um processo tem múltiplos passos (ex: criação de recurso com efeitos colaterais em mais de um lugar), ou tudo funciona ou tudo é revertido. Não deixar passos parciais concluídos com outros falhos sem rollback.

## 7. Diagnóstico e estabilidade contínua
À medida que o sistema evolui, verificar proativamente as soluções aplicadas — não deixar nada "solto". Ao identificar ou corrigir um problema, sugerir proativamente mitigações de risco, mesmo sem ter sido perguntado diretamente.

## 8. Soluções genéricas, não pontuais
Antes de propor algo como "resolvido", perguntar: (a) isso cobre outras situações que causam o mesmo sintoma, não só a que apareceu primeiro? (b) isso é ativo (alcança todo mundo automaticamente) ou passivo (só ajuda quem já sabe procurar)? Se a resposta de qualquer uma for "não", a solução ainda não é genérica — ampliar antes de considerar concluído. Preferir mecanismos automáticos a documentação/avisos que dependem de alguém ler.

## 9. Segurança em testes com dados/arquivos reais
Antes de sobrescrever ou testar em cima de qualquer arquivo/dado real existente: (a) ler o conteúdo atual primeiro pra confirmar que é mesmo descartável, nunca assumir pelo nome; (b) se precisar sobrescrever, fazer backup e **confirmar explicitamente** que o backup existe e tem o tamanho esperado antes do overwrite; (c) preferir testar com um recurso novo em vez de sobrescrever um existente, sempre que o objetivo for só validar algo pontual.

## 10. Nunca usar ferramenta de alto risco sem permissão explícita
Ações com blast radius alto (navegador automatizado, push direto, deploy, comandos destrutivos) pedem permissão explícita antes, mesmo que a tarefa maior já tenha sido aprovada. Preferir sempre a alternativa de menor risco (curl em vez de navegador, leitura de log em vez de ação direta) quando ela resolve o mesmo problema.

## 11. Documentar comportamento interno descoberto, mesmo sem correção necessária
Quando investigar um fluxo revelar um comportamento não óbvio (que só se descobre rastreando o código de verdade), documentar isso — mesmo que não haja correção necessária ou possível. Registrar em dois lugares: comentário no código perto de onde acontece, e `.refia/boas_praticas.md` quando o achado tiver relevância maior que o arquivo isolado.

## 12. Escopo definido — não misturar contexto
Se o projeto tiver múltiplas áreas/sessões trabalhando em paralelo, cada sessão atua só no próprio escopo. Dúvida sobre outra área vira pergunta registrada em `.refia/debate.md`, nunca suposição.

## 13. Numerar itens ao apresentar múltiplos pontos
Sempre que uma resposta tiver mais de um item — perguntas, tópicos, achados, avisos, opções — numerar cada um. Permite responder referenciando só o número, sem reescrever o assunto inteiro.

## 14. Sugestão "pra depois" só existe se virar pendência registrada
Toda sugestão de melhoria/próximo passo que não for implementada na hora tem que virar linha em `.refia/pendencias.md` — com o quê, o porquê, e o que precisa acontecer antes de virar prioridade. Não basta mencionar no chat: o chat não é persistente para quem lê depois.

## 15. Boas práticas descobertas viram registro persistente
Quando uma sessão descobrir/validar uma boa prática — padrão que funciona, gotcha de ferramenta, armadilha que custaria tempo de novo — registrar em `.refia/boas_praticas.md` na hora do achado, não no final da sessão. Não deixar só na memória do chat.

## 16. Nunca rodar chamada de API paga sem autorização explícita a cada vez
Qualquer ação que gaste dinheiro real via serviço de terceiro (LLM pago, API de terceiro cobrada por uso) nunca roda em modo que gaste sem autorização explícita **daquela vez específica** — autorização de "implementar a feature" não é autorização de "gastar". Mostrar estimativa de custo quando disponível antes de perguntar.

## 17. Checklist ao implantar algo com superfície externa
Ao implementar/alterar algo com superfície externa (endpoint novo, campo novo numa resposta já existente, mudança de comportamento visível em dado já exposto), a tarefa só está encerrada depois de:
1. Atualizar `.refia/changelog.md`.
2. Atualizar o contrato/schema formal da camada, se existir um (OpenAPI, JSON Schema, tipos exportados, etc.) — "implantado" não é considerado completo enquanto ele não bate com o código.
3. Avisar quem consome isso do outro lado (outro serviço, outro time, outro repositório) pelo canal que existir — dentro deste projeto, `.refia/debate.md`.

## 18. O sistema `.refia/`

Especificação da pasta `.refia/` — a memória persistente do projeto entre sessões de IA, referenciada nas regras acima. Complementa o `README.md`/`CLAUDE.md`/`AGENTS.md` (que documentam arquitetura/estado atual): aqui fica o que é histórico, pendência formal ou recado pontual entre sessões, não "como o sistema funciona hoje".

**Por que existe:** memória de sessão de um agente de IA é efêmera e isolada — o que foi combinado numa conversa não sobrevive ao fim dela, e não é visível para outra sessão (outra pessoa, outro dia, outro agente) trabalhando no mesmo projeto. Sem um lugar real em disco (versionado com o código) para isso, decisão, pendência e descoberta técnica se perdem ou ficam presas na cabeça de quem participou daquela conversa específica.

**Cabeçalho padrão:** todo arquivo do `.refia/` — inclusive vazio — começa com H1 `# {TIPO EM MAIÚSCULO}`. Arquivo recém-criado sem conteúdo traz uma linha indicando isso (`Sem entradas ainda.`).

### Os 6 arquivos

**`briefing.md`** — retrato do estado atual do projeto, não histórico, é sobrescrito a cada atualização relevante. Serve pra quem abre o projeto context-switchar rápido sem ler o changelog inteiro. Deve responder: onde o projeto está agora, convenções/gotchas técnicas, o que uma sessão nova precisa saber antes de mexer.

**`changelog.md`** — histórico cronológico do que foi feito. Um arquivo só, que cresce — não criar arquivo novo por data. Entradas mais novas sempre no topo, cada entrada com data:
```markdown
## 2026-08-28
Resumo do que mudou, por quê, e o que ficou pendente (se algo).
```

**`pendencias.md`** — tabela única de tudo em aberto. Colunas: `ID`, `Status`, `Descrição`, `Solução`, `Aberto em`, `Resolvido em`. ID sequencial pro arquivo inteiro, nunca reaproveitado; nunca apagar linha, só mudar status; solução resumida (detalhe grande fica no `changelog.md`, citar de lá). Nasce/fecha item só como consequência real de uma entrada no `changelog.md` ou de uma decisão no `debate.md` — nunca uma fonte paralela desconectada do que de fato aconteceu.

**`debate.md`** — canal de pergunta/aviso entre sessões (ou entre áreas, se o projeto tiver mais de uma) — em vez de supor ou agir sobre incerteza. Um arquivo só, que cresce, entrada nova sempre no topo. Duas naturezas de entrada, cada uma com seu próprio ciclo de status:
- **`[PERGUNTA]` → `[RESPONDIDA]`** — espera resposta de conteúdo. Quem responde edita o mesmo bloco (não cria um novo), troca o status e preenche a resposta.
- **`[AVISO]` → `[CIENTE]`** — só informa, não espera resposta de conteúdo, só confirmação de leitura. Quem lê edita o mesmo bloco, troca o status e registra a data em que tomou ciência.

Todo bloco tem `#ID` obrigatório logo depois do status — sequencial pro arquivo inteiro, nunca reaproveitado:
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

**`apontamentos.md`** — canal usuário → sessão (diferente do `debate.md`, que é sessão↔sessão): quem usa o projeto registra ali qualquer coisa que notou — pergunta, comentário, pedido — sem precisar parar o fluxo em andamento pra discutir na hora. A sessão seguinte lê no início do trabalho, resolve (ou traz de volta pra discussão se precisar de decisão) e marca. Formato: bullet simples, agrupado por seção `##` (a seção identifica de onde veio o apontamento). Ao resolver, acrescenta `- (dd/mm/aaaa)` no final da própria linha:
```markdown
## Nome do módulo/área
- Apontamento ainda em aberto.
- Apontamento já resolvido, com a marca no final da linha - (28/08/2026).
```

**`boas_praticas.md`** — conhecimento já implementado/validado: padrão que funciona, gotcha de ferramenta, forma de normalizar dado, armadilha que custaria tempo de novo. Não é fila de ações — se uma boa prática ainda tem "algo a fazer" associado, a entrada aponta para onde aquilo aguarda (`pendencias.md` ou `debate.md`) com a marca `aguarda: {arquivo}`; quando resolver, vira `(aguarda → resolvido dd/mm/aaaa)`, mas a boa prática nunca sai daqui. Formato: bullet simples agrupado por seção `##`.

### Quando usar cada um

- Terminou uma tarefa relevante → entrada no `changelog.md` (topo do arquivo).
- Mudou o estado geral de algo de um jeito que quem retomar o projeto precisa saber antes de começar → atualizar (sobrescrever a parte relevante) o `briefing.md`.
- Surgiu uma pendência nova, ou uma existente foi resolvida → atualizar a tabela em `pendencias.md`.
- Dúvida sobre algo que a sessão atual não tem contexto suficiente pra decidir sozinha → registrar em `debate.md` (`[PERGUNTA]`) em vez de supor.
- Recado/aviso pontual pra quem retomar o trabalho depois → registrar em `debate.md` (`[AVISO]`).
- Usuário notou algo durante o uso e quer registrar sem parar o fluxo atual → `apontamentos.md`.
- Descobriu/validou um padrão, gotcha ou armadilha que vale lembrar → `boas_praticas.md`, na hora do achado.

### Extensão opcional — snapshot de sessão

Para projetos onde sessões longas correm risco de crash/interrupção no meio, um arquivo adicional `_sessao_atual.md` (fora do padrão `{tipo}.md`, é descartável) pode registrar o contexto vivo da sessão em andamento — decisões recentes ainda não absorvidas nos arquivos reais, próximo passo combinado. Deve trazer sempre data/hora de início no topo (pra não confundir sessões paralelas) e ser esvaziado/absorvido nos arquivos reais ao fim natural da sessão. Não é fonte de verdade de nada — só uma rede de segurança contra perda de contexto vivo.

### Monorepo — sufixo por área

Se o projeto for um monorepo com áreas bem distintas (dono/contexto próprio cada uma), use sufixo: `briefing_{area}.md`, `changelog_{area}.md`, etc. Cada arquivo com sufixo tem uma área dona — só ela escreve nele; outras áreas só leem. Único tipo compartilhado entre áreas continua sendo o `debate_{area}.md` (qualquer área pode adicionar uma entrada endereçada a outra; só a área endereçada resolve). `apontamentos_{area}.md` mantém a exceção de que o usuário escreve diretamente, não só a área dona.

---

**Como usar este arquivo:** é um ponto de partida. Regras específicas de negócio, infraestrutura ou stack do projeto devem ser adicionadas aqui ou no `CLAUDE.md`/`AGENTS.md` do próprio projeto — não neste template genérico.
