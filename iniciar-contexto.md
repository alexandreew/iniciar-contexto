---
description: Analisa um projeto e cria/atualiza a estrutura de contexto persistente (CLAUDE.md + .claude/) sem duplicar documentação existente
argument-hint: "[caminho opcional do projeto — padrão: diretório atual]"
---

Você vai bootstrapar (ou atualizar) a estrutura de contexto persistente de um projeto, para reduzir dependência de memória de sessão e facilitar handoffs entre sessões futuras.

Projeto-alvo: `$ARGUMENTS` (se vazio, use o diretório de trabalho atual).

## Passo 1 — Inventariar o que já existe

Antes de criar qualquer arquivo, procure no projeto (e, se for uma subpasta de algo maior, também um nível acima):

- `CLAUDE.md` já existente (raiz do projeto ou pasta-pai — Claude Code carrega CLAUDE.md hierarquicamente, então não repita o que um CLAUDE.md de nível superior já cobre)
- pasta `.claude/` já existente
- `README.md`, wikis, documentação técnica
- qualquer `SKILLS.md`, `handoff*.md`, `HANDOFF*.md`, `CHANGELOG.md` ou equivalente ad-hoc
- se for repositório git: histórico recente (`git log --oneline -20`) só para entender o momento atual, nunca para copiar como handoff

Liste o que encontrou antes de decidir o que criar.

## Passo 2 — Classificar o projeto

**Primeiro, pergunte ao usuário** (nunca infira isso por nome de pasta, quantidade de subpastas ou
qualquer heurística — pergunte sempre, mesmo que a pasta-pai já tenha sido classificada antes):

> "Esta pasta é (a) um projeto/setor específico onde o trabalho de fato acontece, ou (b) uma
> pasta-raiz que agrupa vários projetos/setores diferentes dentro dela (ex.: nome de
> empresa/cliente/contrato), sem trabalho técnico próprio?"

- **Se (b), raiz agregadora**: não é um projeto de trabalho. Não cria `tasks.md`, `decisions.md`
  nem `historico/` aqui — "o que fazer a seguir" e "decisões tomadas" pertencem sempre ao
  projeto/setor que gera o trabalho, nunca à pasta-pai que só os agrupa, mesmo que ela cresça.
  `context.md`/`decisions.md` próprios da raiz só fazem sentido se surgir conteúdo genuinamente
  transversal aos projetos de dentro, em volume que justifique (raro). Ver Passo 4 para o que
  pode ser criado aqui. Depois de classificar a raiz, pergunte se há pasta(s) de projeto/setor já
  existentes dentro dela e ofereça rodar `/iniciar-contexto` em cada uma.
- **Se (a), projeto/setor**: segue a estrutura completa do Passo 4. Verifique se a pasta-pai (ou
  uma acima) já foi classificada — se não houver nenhum registro do papel dela (nem em
  `CLAUDE.md`/`README.md` da pasta-pai), pergunte também sobre ela antes de prosseguir, mesmo que
  o comando tenha sido chamado só para esta pasta.

Depois de classificar, decida com base no que existe:
- É um repositório de código (tem `.git`, `package.json`, etc.) ou uma pasta de trabalho não-código (documentos, planilhas, relatórios)?
- Há documentação numerosa o bastante para justificar um `project-index.md`, ou isso seria um índice vazio de 2-3 arquivos?
- Algum arquivo existente já cobre inteiramente o papel de um dos arquivos-padrão abaixo?

## Passo 3 — Política fixa: nunca mover, renomear ou apagar

Todo arquivo `.md` (ou qualquer outro) que já existir permanece exatamente onde está, com o nome atual. Você não está reorganizando a pasta — está adicionando uma camada nova de contexto persistente por cima do que já existe.

Para cada arquivo-padrão abaixo:
- Se o conteúdo já existe em outro arquivo do projeto: crie a versão nova só com um resumo curto (2-5 linhas) e um link relativo para o arquivo original. Nunca copie o conteúdo por extenso.
- Se um arquivo existente já cobre 100% o papel de um arquivo-padrão: **não crie o arquivo-padrão**. Apenas registre isso no `README.md`/`CLAUDE.md`, com link para o arquivo real.
- Numa **raiz agregadora** (classificada como "(b)" no Passo 2): crie só o que realmente agrega valor — os arquivos marcados **[só projeto/setor]** no Passo 4 nunca são criados aqui, e `context.md`/`decisions.md`/`handoff.md` só se já houver conteúdo transversal real.
- Num **projeto/setor** (classificado como "(a)" no Passo 2), mesmo recém-criado e sem conteúdo ainda: crie **todos** os arquivos-padrão do Passo 4 de uma vez, mesmo vazios — cada um com uma nota curta indicando que está vazio e onde o conteúdo futuro deve entrar. Decisão do usuário: melhor deixar a estrutura pronta desde o início do que lembrar de criar cada arquivo depois, quando o primeiro conteúdo relevante para ele surgir. Isso não contradiz "crie só o que agrega valor" — um projeto/setor real sempre vai gerar tasks/handoff/decisions/context mais cedo ou mais tarde, então o placeholder já agrega valor ao deixar claro onde cada tipo de informação deve ser registrado.

## Passo 4 — Arquivos-padrão (criar/atualizar só o que fizer sentido)

Os arquivos abaixo marcados **[só projeto/setor]** nunca são criados numa pasta-raiz agregadora
(classificada como "(b)" no Passo 2) — pertencem sempre ao projeto/setor específico, mesmo que a
raiz cresça em volume. Os demais podem existir em qualquer uma das duas.

Num **projeto/setor** (classificado como "(a)"), crie todos os arquivos-padrão de uma vez, mesmo
sem conteúdo real ainda — cada um recebe uma nota curta tipo "vazio ainda, o conteúdo entra aqui
quando [condição]" (ver exemplos em cada item abaixo), em vez de ser pulado até o primeiro
conteúdo real surgir.

- **`CLAUDE.md`** (raiz do projeto): permanente e estável — objetivo do projeto, arquitetura geral, stack, convenções, comandos úteis, fluxo de trabalho. Sem histórico de sessões. Deve orientar quando consultar cada arquivo de `.claude/` e cada documento pré-existente relevante.
- **`.claude/context.md`**: conhecimento estável (arquitetura, regras de negócio, integrações, estrutura de dados, limitações conhecidas). Atualizar só quando o projeto de fato mudar. Numa raiz agregadora, só existe se surgir conhecimento genuinamente transversal aos projetos de dentro (raro).
- **`.claude/tasks.md`** **[só projeto/setor]**: Current / Next / Done. Sempre pequeno. Substituir, nunca acumular como histórico. "O que fazer a seguir" é sempre de um projeto específico, nunca da pasta-pai que só agrupa.
- **`.claude/handoff.md`**: log cronológico completo do projeto — o que foi feito em cada sessão, quando, por quê, o que foi tentado e não deu certo, achados técnicos, pendências e seus motivos. **Nunca sobrescrito, sempre uma seção nova por sessão/período** (changelog datado, com tabela de navegação Data/Período/Resumo no topo — ver Passo 5 para o critério de arquivamento quando crescer demais). Este é o padrão default de todo projeto/setor criado por este comando, não uma exceção. Numa raiz agregadora, só existe para infraestrutura genuinamente transversal (ex.: automação de sessão compartilhada) — nunca para registrar trabalho de um projeto específico de dentro.
- **`.claude/decisions.md`** **[só projeto/setor]**: só decisões relevantes (arquiteturais, técnicas, de negócio, de workflow) daquele projeto, cada uma com data, decisão, motivação, impacto. Não registrar implementações pequenas.
- **`.claude/historico/`** **[só projeto/setor]**: destino do arquivamento por período (ver Passo 5). Uma raiz agregadora não acumula changelog de projeto, então não tem o que arquivar.
- **`.claude/project-index.md`**: só se realmente agregar valor sobre navegar a documentação existente — nunca duplicar um README/wiki já bom.
- **`.claude/README.md`**: explica a finalidade de cada arquivo de `.claude/` e lista, com link, os documentos pré-existentes do projeto que continuam sendo a fonte primária de algum assunto (em vez de terem sido duplicados). Numa raiz agregadora, deve também listar os projetos/setores de dentro e apontar para o `.claude/README.md` de cada um.

## Passo 4a — Como os arquivos-padrão se relacionam (frequência e fluxo entre eles)

Cada arquivo do Passo 4 tem uma frequência de atualização própria e um papel específico — nenhum
duplica o conteúdo do outro, cada um é o resumo (ou o detalhe) de outro nível:

| Arquivo | Atualiza | Quando |
|---|---|---|
| `CLAUDE.md` | Raramente | Só quando arquitetura, stack ou convenção do projeto muda de verdade |
| `context.md` | Raramente | Só quando o projeto muda de forma (nova integração, regra de negócio nova) |
| `decisions.md` | Ocasional | Só quando uma decisão relevante acontece (não toda sessão, não implementação pequena) |
| `tasks.md` | Toda sessão | Ao fechar/pausar, sempre sobrescrito |
| `handoff.md` | Toda sessão | Ao fechar/pausar — nunca sobrescrito, recebe sempre uma seção/período novo (log cronológico completo, ver Passo 5 para arquivamento quando crescer demais) |
| `README.md` / `project-index.md` | Raramente | Só quando a lista de arquivos de `.claude/` muda (arquivo novo criado/removido) |

**O fluxo, ao concluir algo dentro de uma sessão** (não ao fechar a sessão inteira — isso é o
próprio ato de trabalhar, pode acontecer várias vezes numa sessão longa):

1. **Detalhe completo vai para o `handoff.md`** — o que foi feito, como, onde no código/arquivo,
   o resultado. Se o handoff usa changelog datado por período (ver Passo 5), isso vira uma seção
   nova (ex.: "12.3 Autenticação por token expandida...").
2. **Se o handoff tem changelog no topo** (tabela Data/Período/Resumo), adicione uma linha
   resumida ali, apontando para a seção nova do passo 1. Este é o nível intermediário de detalhe.
3. **`tasks.md` recebe só uma linha curtíssima** em "Done" (ou o item sai de "Current"/"Next"
   inteiramente, se não for relevante manter registrado) — sem repetir o passo a passo, só um
   resumo com data. Não duplique o texto do handoff aqui, referencie por data/assunto.
4. **`decisions.md` só recebe algo se o que foi concluído envolveu uma decisão relevante**
   (arquitetural, técnica, de negócio) — não é todo item de "Done" que vira uma entrada aqui; a
   maioria não deveria.
5. **`context.md`, `CLAUDE.md` e `README.md` só mudam se o trabalho concluído alterou algo
   estrutural** (nova arquitetura, nova convenção, arquivo novo criado em `.claude/`) — na
   maioria das sessões, nenhum dos três muda.

Nunca copie o mesmo texto por extenso em mais de um arquivo — cada nível existe para dar a quem lê
(você numa sessão futura, ou eu) a opção de ver só o resumo (tasks.md) ou ir fundo (seção do
handoff), sem forçar ler tudo pra achar uma informação pequena.

## Passo 5 — Manutenção: quando um arquivo crescer demais

Regra original deste framework, que vale tanto ao criar quanto ao revisitar um projeto depois: quando algum arquivo de `.claude/` (ou o `CLAUDE.md`) começar a crescer excessivamente, reorganize-o ou arquive informações antigas em vez de deixá-lo aumentar sem limite. Na prática:

- Separe **arquitetura estável** (o que muda pouco: estrutura, integrações, regras de negócio fixas) para um `context.md` próprio, mantendo o restante (changelog datado, decisões técnicas, pendências vivas) no arquivo original.
- Ao mover uma seção, deixe uma nota curta no lugar de onde ela saiu (ex.: "Seção X movida para `context.md`") e corrija toda referência cruzada que apontava para ela (`ver seção X` → `ver seção X em context.md`) — não deixe ponteiros órfãos.
- Isso é diferente de criar `context.md` do zero no Passo 4: aqui você está **dividindo** um arquivo que já existe e cresceu, não decidindo a estrutura inicial.

### Arquivamento por período (para handoffs com changelog datado por período/seção numerada)

Quando o handoff usa uma tabela de changelog no topo (Data / Período / Resumo / Seção, como o padrão já usado em outro projeto de referência), o critério de arquivamento não é o fechamento de um trimestre calendário — é o **volume de períodos** combinado com **ausência de pendência ativa**:

- **Gatilho**: quando houver pelo menos **10 períodos consecutivos mais antigos** no handoff (mantendo sempre uma janela dos períodos mais recentes no arquivo vivo).
- **Condição de bloco**: os 10+ períodos candidatos só são arquivados **como bloco inteiro**, e só se **nenhum deles** tiver pendência ativa. Antes de arquivar, releia o changelog e o texto de cada período candidato procurando por pendências que ainda apareçam referenciadas em períodos mais recentes (ex.: "seção X ainda não confirmada", "aguardando resposta de Y") — uma pendência de um período antigo que segue sem solução nos períodos mais novos barra o arquivamento do bloco inteiro, mesmo que os demais períodos do bloco estejam concluídos. Isso evita um histórico com buracos (períodos não sequenciais) e evita perder rastreabilidade de algo ainda pendente.
- **Destino**: mover o bloco para `.claude/historico/handoff-historico-AAAA-Q#.md` (rótulo do trimestre em que a maioria dos períodos do bloco ocorreu, mesmo que o gatilho de arquivamento não seja o fechamento desse trimestre).
- **No handoff vivo**, após arquivar, mantenha: (a) o changelog resumido de uma linha por período (já existente no topo), e (b) uma nota indicando onde está o histórico (ex.: "seções 2-11 → ver `historico/handoff-historico-2026-Q3.md`").
- Antes de aplicar, confira se algum bloco já qualifica: liste os períodos candidatos, confirme pendência zero em cada um contra o texto completo do handoff (não só o changelog resumido, que pode não registrar todas as pendências), e só então mova.

Um `handoff.md`/equivalente que já existia antes de rodar este comando, com outro formato (ex.: sempre sobrescrito, sem changelog), deve ser convertido para o padrão de log cronológico ao rodar `/iniciar-contexto` — registre a conversão e a data no `.claude/README.md`, preservando o conteúdo já existente como o primeiro período do changelog em vez de descartá-lo.

## Passo 6 — Instalar o comando no projeto

Copie este mesmo arquivo de comando para `<projeto>/.claude/commands/iniciar-contexto.md`. Isso torna `/iniciar-contexto` disponível para qualquer outra pessoa que tenha acesso a esta pasta (OneDrive compartilhado ou git) e abra o Claude Code aqui — comandos dentro de `.claude/commands/` de um projeto são carregados automaticamente por qualquer usuário que abra aquele diretório, diferente de `~/.claude/commands/`, que é exclusivo de quem o criou.

## Passo 6a — Ocultar os arquivos de infraestrutura/contexto (preferência do usuário)

Depois de criar/atualizar os arquivos, aplique o atributo Oculto do Windows (`attrib +h`, via PowerShell `Get-Item -Force` para conferir) em todo item de infraestrutura/contexto pessoal criado ou já existente no projeto — para não "sujar" a pasta com arquivos que só o usuário usa. Isso não afeta a leitura pelo Claude Code (que sempre acessa por caminho direto), só some da listagem padrão do Explorer/OneDrive.

Aplicar em:
- `.claude/` (a pasta inteira, uma vez — o atributo Oculto de uma pasta já esconde tudo dentro dela no Explorer, não precisa marcar item por item)
- `.vscode/`, se existir
- `.gitignore`, se existir
- **Todo arquivo `.md` solto na raiz do projeto** que seja de contexto/uso pessoal: `CLAUDE.md` sempre; `context.md`, `handoff*.md`/`HANDOFF*.md` (incluindo variantes `.bak`), `decisions.md`, `ACHADO-*.md`/achados técnicos e qualquer outro `.md` de nota/contexto que não seja documentação destinada a outra pessoa (ex: um README de entrega para o cliente não deve ser ocultado).

**Não ocultar:** dados de trabalho (planilhas, arquivos de dados/relatório específicos da ferramenta usada, `.json` de dados, pastas de dados do projeto) — só o que é infraestrutura de contexto/sessão do Claude Code.

Se o usuário pedir para não fazer isso, ou se o projeto for destinado a ser compartilhado abertamente com outra pessoa que precise ver `CLAUDE.md` sem instrução extra, pule este passo e avise no resumo final.

## Passo 7 — Resumo final

Termine com um resumo curto: o que foi criado, o que foi reaproveitado (com link), o que foi pulado e por quê, e confirmação de que o comando foi instalado no projeto.

## Rotina de uso, depois de instalado (não é bootstrap — é o dia a dia)

**Ao abrir uma sessão neste projeto:**
1. `CLAUDE.md` já carrega sozinho (e o de uma pasta-pai, se houver).
2. Leia `.claude/tasks.md` primeiro — o plano atual (Current/Next), o resumo mais rápido de onde
   parou.
3. Leia a tabela de changelog no topo de `.claude/handoff.md` — as últimas entradas mostram o que
   foi feito recentemente. Só abra a seção completa de um período específico se precisar do
   detalhe técnico.
4. Só abra `context.md`/`decisions.md`/documentos pré-existentes quando precisar de detalhe que os
   arquivos acima não têm.

**Durante o trabalho:** não precisa atualizar nada a cada mensagem — só ao concluir algo (ver Passo
4a) ou quando uma regra de negócio nova, uma decisão relevante, ou uma mudança de rumo realmente
acontecer.

**Ao fechar/pausar a sessão:**
- **Acrescente uma seção nova** em `.claude/handoff.md` (nunca sobrescreva, nunca apague uma
  seção existente) com: o que foi feito, como, achados, o que não deu certo e por quê, próximos
  passos, riscos. Atualize a tabela de changelog no topo com uma linha resumida apontando para a
  seção nova.
- Atualize `.claude/tasks.md`, movendo itens entre Current/Next/Done (este sim é sempre
  sobrescrito — reflete só o presente).
- Só toque `context.md`/`decisions.md` se algo estrutural mudou de fato — não por hábito.
