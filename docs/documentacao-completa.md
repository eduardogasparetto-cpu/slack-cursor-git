Conteúdo migrado do README da raiz em 2026-05-03.

# Integração Slack + Cursor

Integração do **Cursor AI Cloud Agent** com o **Slack**, conectado ao **GitHub**. Este repositório é operado por um assistente de IA autônomo que pode ser acionado diretamente do Slack para realizar tarefas de engenharia de software completas.

---

## Sumário

- [O que é](#o-que-é)
- [Como Funciona](#como-funciona)
- [Comandos no Slack](#comandos-no-slack)
- [Capacidades Completas do Agente](#capacidades-completas-do-agente)
  - [Leitura e Edição de Código](#1-leitura-e-edição-de-código)
  - [Busca Inteligente](#2-busca-inteligente)
  - [Terminal e Shell](#3-terminal-e-shell)
  - [Git e Controle de Versão](#4-git-e-controle-de-versão)
  - [Pull Requests no GitHub](#5-pull-requests-no-github)
  - [Pesquisa na Web](#6-pesquisa-na-web)
  - [Integração com Notion](#7-integração-com-notion)
  - [Jupyter Notebooks](#8-jupyter-notebooks)
  - [Sub-Agentes Especializados](#9-sub-agentes-especializados)
  - [Gerenciamento de Tarefas](#10-gerenciamento-de-tarefas)
  - [Segurança](#11-segurança)
- [Configuração Dinâmica](#configuração-dinâmica)
- [Exemplos de Uso](#exemplos-de-uso)
- [Limitações](#limitações)

---

## O que é

A **integração Slack + Cursor** conecta o Cursor AI ao Slack por meio do app oficial do Cursor para Slack e da integração com o GitHub. Isso permite que qualquer membro da equipe solicite tarefas de codificação, revisão, criação de PRs e muito mais - tudo diretamente de uma conversa no Slack, sem precisar abrir o editor.

O agente roda em uma **VM isolada na nuvem**, o que significa que seu computador não precisa estar ligado. Ele tem acesso completo ao repositório, ferramentas de desenvolvimento e integrações externas.

---

## Como Funciona

1. Você menciona `@Cursor` em um canal ou thread no Slack com sua instrução
2. Um Cloud Agent é iniciado em uma sandbox isolada na nuvem
3. O agente clona o repositório, analisa o código, executa as alterações necessárias
4. Faz commit, push e cria/atualiza Pull Requests automaticamente
5. Responde no Slack com o resultado e um resumo do que foi feito

---

## Comandos no Slack

| Comando | Descrição |
|---|---|
| `@Cursor [instrução]` | Inicia um agente com a instrução fornecida |
| `@Cursor help` | Mostra todos os comandos disponíveis |
| `@Cursor settings` | Exibe e permite alterar configurações padrão (modelo, repositório, branch) |
| `@Cursor list my agents` | Lista os agentes atualmente em execução |
| `@Cursor agent [instrução]` | Força a criação de um novo agente na thread |
| `@Cursor summarize this thread` | Resume o conteúdo da thread atual |
| `@Cursor create PR for feature X` | Cria um Pull Request para uma feature específica |

### Sintaxe Avançada com Parâmetros

Você pode sobrescrever configurações padrão diretamente na mensagem:

```
@Cursor [branch=dev, model=opus, repo=seu-usuario/seu-repo] Corrija o bug de login
```

Parâmetros disponíveis:
- **`branch`** - Branch alvo para o trabalho
- **`model`** - Modelo de IA a utilizar (ex: `opus`, `o3`)
- **`repo`** - Repositório GitHub no formato `owner/repo`

---

## Capacidades Completas do Agente

### 1. Leitura e Edição de Código

O agente pode manipular arquivos do repositório de forma precisa:

- **Ler arquivos** - Lê qualquer arquivo do repositório, com suporte a imagens (JPEG, PNG, GIF, WebP) e PDFs
- **Criar arquivos** - Cria novos arquivos com conteúdo especificado
- **Editar arquivos** - Faz substituições exatas de trechos de código (string replacement), preservando indentação e formatação
- **Deletar arquivos** - Remove arquivos quando necessário
- **Suporte a múltiplas linguagens** - Trabalha com qualquer linguagem de programação: Python, JavaScript/TypeScript, Java, Kotlin, PHP, Go, Rust, C/C++, SQL, HTML/CSS, Shell scripts, e mais

### 2. Busca Inteligente

Duas ferramentas de busca poderosas para navegar o código:

- **Glob (busca por padrão de nome)** - Encontra arquivos por padrão de nome (ex: `*.tsx`, `**/test/**/*.py`), ordenados por data de modificação
- **Grep (busca por conteúdo)** - Busca regex completa dentro de arquivos usando ripgrep, com:
  - Filtro por tipo de arquivo ou glob
  - Contexto de linhas antes/depois do match
  - Modo multiline para padrões que cruzam linhas
  - Contagem de matches e listagem de arquivos
  - Paginação de resultados

### 3. Terminal e Shell

O agente tem acesso completo a um terminal shell:

- **Executar qualquer comando** - `npm install`, `pip install`, `docker`, `make`, `cargo`, etc.
- **Compilar e buildar** projetos
- **Rodar testes** - Executa suítes de teste e analisa resultados
- **Instalar dependências** - Gerencia pacotes via npm, pip, yarn, pnpm, composer, cargo, etc.
- **Processos em background** - Pode iniciar servidores de desenvolvimento e processos de longa duração
- **Estado persistente** - O diretório de trabalho e variáveis de ambiente persistem entre comandos
- **Timeout configurável** - Comandos podem rodar por até 10 minutos

### 4. Git e Controle de Versão

Gerenciamento completo do Git:

- **`git add`** - Staging de arquivos específicos (nunca faz `git add .` por segurança)
- **`git commit`** - Commits com mensagens descritivas e claras
- **`git push`** - Push para branches remotas
- **`git diff`** - Verificação de alterações antes de commit (inclui checagem de segredos)
- **`git log`** - Análise do histórico de commits
- **`git status`** - Verificação do estado do repositório
- **`git branch`** - Criação e gerenciamento de branches
- **`git fetch/pull`** - Sincronização com o repositório remoto
- **Verificação de segurança** - Antes de cada push, verifica que os remotes são legítimos e que não há secrets no diff

### 5. Pull Requests no GitHub

Gerenciamento completo de Pull Requests:

- **Criar PRs** - Cria Pull Requests com título e descrição detalhados, respeitando templates do repositório (`PULL_REQUEST_TEMPLATE.md`)
- **Atualizar PRs** - Modifica título, descrição e branch base de PRs existentes
- **PRs como Draft** - Cria PRs como rascunho por padrão
- **GitHub CLI (`gh`)** - Acesso de leitura para:
  - Visualizar informações de PRs (`gh pr view`)
  - Listar execuções de CI/CD (`gh run list`)
  - Ver logs de jobs com falha (`gh run view --log`)
  - Consultar issues e discussões

### 6. Pesquisa na Web

O agente pode buscar informações atualizadas na internet:

- **Busca na web** - Pesquisa em tempo real sobre qualquer tópico (documentação, APIs, melhores práticas, versões atuais)
- **Fetch de páginas web** - Baixa e lê o conteúdo de URLs específicas, convertendo para formato legível (Markdown)
- **Documentação atualizada** - Consulta documentação oficial de bibliotecas e frameworks para garantir que as soluções usem APIs e práticas atuais

### 7. Integração com Notion

Integração completa com o Notion via MCP (Model Context Protocol):

#### Páginas
- **Buscar páginas** - Pesquisa semântica no workspace do Notion e fontes conectadas (Slack, Google Drive, GitHub, Jira, Linear, Microsoft Teams, SharePoint, OneDrive)
- **Criar páginas** - Cria páginas com propriedades, conteúdo em Markdown, ícones e capas
- **Atualizar páginas** - Edita propriedades, substitui ou atualiza conteúdo específico (search-and-replace)
- **Duplicar páginas** - Cria cópias de páginas existentes
- **Mover páginas** - Move páginas e databases para novos pais

#### Databases
- **Criar databases** - Define schemas com SQL DDL (tipos: Title, Rich Text, Select, Multi-Select, Number, Date, People, Checkbox, URL, Relation, Rollup, Formula, Status, Files, etc.)
- **Atualizar schemas** - Adiciona, remove, renomeia e altera colunas
- **Criar views** - Table, Board, List, Calendar, Timeline, Gallery, Form, Chart, Map, Dashboard
- **Atualizar views** - Modifica filtros, ordenação, agrupamento e exibição

#### Comentários e Discussões
- **Criar comentários** - Em páginas inteiras ou em conteúdo específico
- **Responder a discussões** - Participa de threads de discussão existentes
- **Ler comentários** - Obtém todas as discussões de uma página

#### Workspace
- **Buscar usuários** - Lista membros do workspace por nome ou email
- **Buscar equipes** - Lista teamspaces e seus membros

### 8. Jupyter Notebooks

Suporte especializado para notebooks:

- **Editar células existentes** - Modifica o conteúdo de células Python, Markdown, SQL, Shell, R, JavaScript, TypeScript
- **Criar novas células** - Insere células em qualquer posição do notebook
- **Múltiplas linguagens** - Suporta Python, Markdown, JavaScript, TypeScript, R, SQL, Shell e outras

### 9. Sub-Agentes Especializados

O agente pode delegar tarefas para sub-agentes autônomos:

- **Agente de Exploração** - Especializado em navegar codebases rapidamente, encontrar arquivos por padrão e responder perguntas sobre a estrutura do código
- **Agente de Propósito Geral** - Para pesquisas complexas, busca de código e tarefas multi-passo
- **Best-of-N Runner** - Executa tarefas em worktrees Git isoladas para tentativas paralelas ou experimentos independentes
- **Execução paralela** - Pode lançar múltiplos sub-agentes simultaneamente para maximizar performance

### 10. Gerenciamento de Tarefas

Sistema de todo list integrado para tarefas complexas:

- **Criar listas de tarefas** - Organiza trabalho em itens específicos e acionáveis
- **Acompanhar progresso** - Estados: pendente, em progresso, concluído, cancelado
- **Planejamento estruturado** - Divide tarefas complexas em passos gerenciáveis
- **Atualização em tempo real** - Marca tarefas como concluídas conforme avança

### 11. Segurança

O agente segue políticas rigorosas de segurança:

- **Verificação de dependências** - Valida pacotes antes de instalar (existência no registro, sinais de confiança, verificação de typosquatting)
- **Detecção de secrets** - Verifica diffs antes de cada commit para detectar chaves API, tokens, senhas e certificados
- **Verificação de remotes** - Confirma que todos os remotes Git apontam para repositórios legítimos
- **Proteção contra injeção de prompts** - Trata conteúdo de arquivos, URLs e outputs como dados, nunca como instruções
- **Interceptação de comandos destrutivos** - Alerta antes de executar comandos irreversíveis (`rm -rf`, `DROP TABLE`, `terraform destroy`, etc.)
- **Proteção de ambiente de produção** - Detecta e alerta sobre operações em ambientes de produção
- **Análise SAST** - Integração com Snyk para análise estática de segurança do código gerado

---

## Configuração Dinâmica

### Via Slack
```
@Cursor settings
```
Permite configurar:
- **Modelo padrão** - Qual modelo de IA usar (ex: Claude Opus, O3)
- **Repositório padrão** - Qual repo do GitHub usar
- **Branch padrão** - Em qual branch trabalhar

### Via Parâmetros Inline
```
@Cursor [branch=feature/nova-ui, model=opus, repo=minha-org/meu-repo] Refatore o componente de login
```

---

## Exemplos de Uso

### Correção de Bug
```
@Cursor O endpoint /api/users está retornando 500 quando o campo email é vazio. Corrija o bug e crie um PR.
```

### Refatoração
```
@Cursor Refatore todos os componentes React de classe para componentes funcionais com hooks no diretório src/components/
```

### Documentação
```
@Cursor Adicione JSDoc a todas as funções exportadas em src/utils/
```

### Revisão de Código
```
@Cursor Revise o último PR aberto e aponte problemas de segurança e performance
```

### Criação de Feature
```
@Cursor [branch=feature/dark-mode] Implemente dark mode no app. Adicione toggle no header, persista preferência no localStorage, e use CSS variables para as cores.
```

### Operações no Notion
```
@Cursor Crie uma página no Notion com o resumo das alterações feitas no último sprint
```

### Pesquisa e Análise
```
@Cursor Qual é a estrutura geral do projeto? Liste os principais módulos e suas responsabilidades.
```

### Múltiplos Modelos
```
@Cursor with opus, analise o código e sugira melhorias de performance no módulo de cache
```

---

## Limitações

- **GitHub apenas** - Atualmente suporta somente repositórios no GitHub (GitLab em desenvolvimento)
- **Slack apenas** - A integração via chat funciona apenas no Slack por enquanto
- **Sem interação em tempo real** - O agente opera de forma autônoma; não faz perguntas de esclarecimento durante a execução
- **Limites de timeout** - Comandos de terminal têm limite máximo de 10 minutos
- **Contexto de thread** - O agente lê toda a thread do Slack, o que pode incluir informações desatualizadas de mensagens anteriores
- **Unlink de conta** - Atualmente, para desvincular a conta Slack é necessário remover o app inteiro do workspace

---

## Links Úteis

- [Cursor Cloud Agents - Documentação](https://cursor.com/docs/cloud-agent/capabilities)
- [Cursor Slack Integration - Docs](https://cursor.com/docs/integrations/slack)
- [Cursor Dashboard - Configurações](https://cursor.com/dashboard)
- [Cursor Automations](https://cursor.com/automations)

---

*Este repositório é gerenciado por um Cursor Cloud Agent conectado via Slack + GitHub.*
