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

## Instalação

**Uso global** (disponível em todos os projetos que você abrir):

1. Baixe `iniciar-contexto.md` deste repositório.
2. Coloque em `~/.claude/commands/iniciar-contexto.md`.

**Uso em um projeto específico** (fica disponível para qualquer pessoa que abra essa pasta no Claude Code):

1. Baixe `iniciar-contexto.md` deste repositório.
2. Coloque em `<projeto>/.claude/commands/iniciar-contexto.md`.

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

## Licença

Sinta-se livre para usar, adaptar e redistribuir.
