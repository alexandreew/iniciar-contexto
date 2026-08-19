# /iniciar-contexto

Comando personalizado para [Claude Code](https://claude.com/claude-code) que analisa um projeto e cria (ou atualiza) uma estrutura de contexto persistente, para reduzir a dependência de memória de sessão e facilitar handoffs entre sessões futuras.

## O que ele faz

- Inventaria o que já existe no projeto (`CLAUDE.md`, `.claude/`, `README.md`, handoffs, changelogs) antes de criar qualquer coisa nova.
- Nunca move, renomeia ou apaga arquivos existentes: adiciona uma camada de contexto por cima do que já está lá.
- Classifica o projeto (pergunta ao usuário, nunca infere) entre "projeto/setor com trabalho técnico" ou "pasta-raiz agregadora de vários projetos", e cria só os arquivos que fazem sentido para cada caso.
- Cria/atualiza um conjunto padrão de arquivos:
  - `CLAUDE.md`: visão geral estável do projeto (arquitetura, stack, convenções).
  - `.claude/context.md`: conhecimento estável (regras de negócio, integrações, limitações).
  - `.claude/tasks.md`: Current / Next / Done, sempre atual, sempre pequeno.
  - `.claude/handoff.md`: log cronológico completo, nunca sobrescrito, com changelog datado no topo.
  - `.claude/decisions.md`: só decisões relevantes (arquiteturais, técnicas, de negócio).
  - `.claude/historico/`: arquivamento de handoffs antigos quando o log crescer demais.
  - `.claude/project-index.md` e `.claude/README.md`: navegação e explicação da própria estrutura.
- Define regras de manutenção: quando um arquivo cresce demais, como dividir e arquivar sem perder rastreabilidade de pendências.

## Onde funciona

Comandos personalizados são um recurso do Claude Code, o mesmo motor por trás de várias interfaces diferentes. Funciona em:

- **CLI** (terminal)
- **Extensão VS Code**
- **Plugin JetBrains** (IntelliJ, PyCharm, Android Studio, WebStorm, PhpStorm, GoLand)
- **App Desktop**, aba **Code** (não funciona nas abas Chat nem Cowork, que são produtos diferentes)
- **Claude Code Web** (claude.ai/code), com a ressalva de instalação abaixo

Não funciona no Claude Desktop (app de chat comum) nem no claude.ai (chat comum), que não têm acesso ao sistema de arquivos nem executam comandos personalizados.

## Instalação

### CLI, VS Code, JetBrains e Desktop (aba Code)

Essas interfaces compartilham a mesma configuração local (`CLAUDE.md`, `.claude/`, MCP, hooks, skills).

**Uso global** (disponível em todos os projetos que você abrir):

1. Baixe `iniciar-contexto.md` deste repositório.
2. Coloque em `~/.claude/commands/iniciar-contexto.md`.

**Uso em um projeto específico** (fica disponível para qualquer pessoa que abra essa pasta em qualquer uma dessas interfaces):

1. Baixe `iniciar-contexto.md` deste repositório.
2. Coloque em `<projeto>/.claude/commands/iniciar-contexto.md`.

No app Desktop, abra a aba **Code** (não Chat, não Cowork), selecione ambiente **Local** e a pasta do projeto. O comando aparece ao digitar `/` ou clicar `+` > **Slash commands**, junto dos comandos nativos.

### Claude Code Web (claude.ai/code)

Sessões na web rodam numa VM na nuvem que clona o remoto do GitHub, não o seu checkout local. Não existe pasta pessoal (`~/.claude/commands/`) nesse ambiente, então o comando só fica disponível se estiver no remoto que a VM clona:

1. Baixe `iniciar-contexto.md` deste repositório.
2. Coloque em `<projeto>/.claude/commands/iniciar-contexto.md` no repositório que você vai usar na sessão web.
3. Faça commit e **push** desse arquivo para o GitHub (a VM clona o remoto na branch atual, não o seu checkout local; commits sem push não chegam até ela).
4. Abra uma sessão em [claude.ai/code](https://claude.ai/code) apontando para esse repositório (ou rode `claude --cloud` a partir do CLI, na pasta do repositório).
5. O comando `/iniciar-contexto` aparece disponível na sessão assim que o repositório é clonado.

## Uso

Dentro do Claude Code, no diretório do projeto:

```
/iniciar-contexto
```

Ou apontando para outro caminho:

```
/iniciar-contexto caminho/do/projeto
```

O comando vai perguntar o que precisar (ex: se a pasta é um projeto específico ou uma raiz que agrupa vários) antes de criar qualquer arquivo.

## Fontes

Informações sobre onde e como o Claude Code funciona, baseadas na documentação oficial:

- [Desktop application](https://code.claude.com/docs/en/desktop) (confirma as três abas Chat/Cowork/Code)
- [Get started with the desktop app](https://code.claude.com/docs/en/desktop-quickstart)
- [Use Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web) (confirma que a VM clona o remoto do GitHub, não o checkout local)
- [Use Claude Code in VS Code](https://code.claude.com/docs/en/vs-code)
- [Slash Commands in the SDK](https://code.claude.com/docs/en/agent-sdk/slash-commands)

## Licença

Sinta-se livre para usar, adaptar e redistribuir.
