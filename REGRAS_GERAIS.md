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

---

**Como usar este arquivo:** é um ponto de partida. Regras específicas de negócio, infraestrutura ou stack do projeto devem ser adicionadas aqui ou no `CLAUDE.md`/`AGENTS.md` do próprio projeto — não neste template genérico.
