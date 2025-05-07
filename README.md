# BugFlow Multi Agents - Sistema Inteligente de Triagem de Bugs e Incidentes

![Logo do Projeto](https://i.imgur.com/1QgrNNf.png)

## 📋 Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Arquitetura do Sistema](#arquitetura-do-sistema)
- [Requisitos](#requisitos)
- [Configuração Passo a Passo](#configuração-passo-a-passo)
- [Como Executar](#como-executar)
- [Componentes Detalhados](#componentes-detalhados)
- [Solução de Problemas](#solução-de-problemas)
- [Exemplos de Uso](#exemplos-de-uso)
- [FAQ](#faq)

## 📖 Sobre o Projeto

O BugFlow Multi Agents é um sistema inteligente de triagem de bugs e incidentes que utiliza múltiplos agentes de IA especializados trabalhando em conjunto para automatizar o processamento de problemas técnicos em desenvolvimento de software. O sistema é capaz de:

- **Classificar bugs** por componente afetado (Frontend, Backend, Database, DevOps, Security, Integration, UI/UX, Infrastructure)
- **Definir severidade** (Critico, Grave, Menor)
- **Realizar análise técnica** detalhada com sugestões de causas e soluções
- **Gerenciar o fluxo** de resolução e atribuição a desenvolvedores
- **Gerar documentação** completa do bug e da solução em formato Markdown e HTML com diagramas Mermaid

Os agentes trabalham de forma colaborativa, como uma equipe real de desenvolvimento, para processar e resolver bugs com eficiência e precisão, permitindo que os programadores foquem em tarefas de maior valor.

## 🏗️ Arquitetura do Sistema

### Diagrama de Fluxo

```
┌─────────────────┐     ┌──────────────────────────────────────────────────────┐
│                 │     │                                                      │
│  Banco de Dados │◄────┤                  Sistema de Agentes                  │
│   (Neon DB)     │     │                                                      │
│                 │     │  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────┐  │
└────────┬────────┘     │  │         │   │         │   │         │   │     │  │
         │              │  │  AG1:   │   │  AG2:   │   │  AG3:   │   │     │  │
         │              │  │ Classi- │   │ Classi- │   │ Análise │   │ ... │  │
         │              │  │ ficador │──►│ ficador │──►│ Técnica │──►│     │  │
         │              │  │  Setor  │   │ Priori- │   │         │   │     │  │
┌────────▼────────┐     │  │         │   │  dade   │   │         │   │     │  │
│                 │     │  └─────────┘   └─────────┘   └─────────┘   └─────┘  │
│    Pinecone     │◄────┤                                                      │
│ (Vector Store)  │     └──────────────────────────────────────────────────────┘
│                 │
└─────────────────┘
```

### Fluxo de Processamento

1. O sistema obtém bugs e incidentes abertos do banco de dados Neon
2. Cada bug passa por uma sequência de agentes especializados:
   - **AG1**: Classifica o bug por componente afetado (API, Database, UI, etc.)
   - **AG2**: Define a severidade do bug (Crítico, Grave, Menor)
   - **AG3**: Realiza análise técnica detalhada e sugere possíveis causas
   - **AG4**: Gerencia o fluxo de resolução e atribuição a desenvolvedores
   - **AG5**: Gera documentação completa do bug e da solução
3. Os resultados de cada etapa são armazenados no banco de dados
4. Os vetores de embeddings são armazenados no Pinecone para busca semântica de bugs similares

## 🛠️ Requisitos

### Serviços Externos

- Conta na [OpenAI](https://platform.openai.com) (para acesso aos modelos GPT)
- Conta no [Neon DB](https://neon.tech) (banco de dados PostgreSQL serverless)
- Conta no [Pinecone](https://www.pinecone.io) (armazenamento de vetores)

### Requisitos Técnicos

- Python 3.9+
- Ambiente virtual (venv)
- Dependências listadas em `requirements.txt`

## 🚀 Configuração Passo a Passo

### 1. Preparação do Ambiente Local


```

#### 1.2. Configure o Ambiente Virtual

**Windows:**
```powershell
# Criar ambiente virtual
python -m venv .venv

# Ativar ambiente virtual
.venv\Scripts\Activate.ps1

# Instalar dependências
uv pip install --system -r requirements.txt
```

**Linux/Mac:**
```bash
# Criar ambiente virtual
python -m venv .venv

# Ativar ambiente virtual
source .venv/bin/activate

# Instalar dependências
uv pip install --system -r requirements.txt
```

### 2. Configuração dos Serviços Externos

#### 2.1. Configuração do Neon DB

1. **Criar uma Conta no Neon DB**
   - Acesse [https://neon.tech](https://neon.tech)
   - Registre-se para uma nova conta
   - Faça login na sua conta

2. **Criar um Novo Projeto**
   - No painel do Neon, clique em "New Project"
   - Dê um nome ao projeto (ex: "ai-hackagents")
   - Selecione uma região próxima à sua localização

   ![Neon DB - Criar Projeto](https://i.imgur.com/lM8T1JD.png)

3. **Obter a URL de Conexão**
   - Após a criação do projeto, clique em "Connection Details"
   - Selecione "Connection String"
   - Copie a string de conexão completa

   ![Neon DB - Connection String](https://i.imgur.com/6rjDGLP.png)

#### 2.2. Configuração do Pinecone

1. **Criar uma Conta no Pinecone**
   - Acesse [https://www.pinecone.io](https://www.pinecone.io)
   - Registre-se para uma nova conta
   - Faça login na sua conta

2. **Criar um Novo Índice**
   - No painel do Pinecone, clique em "Create Index"
   - Preencha os campos:
     - **Nome do índice**: "bugflow"
     - **Dimensão**: 1536 (para embeddings da OpenAI)
     - **Métrica**: "cosine"
     - **Ambiente**: "us-east-1"

   ![Pinecone - Criar Índice](https://i.imgur.com/sB5XE6e.png)

3. **Obter a API Key**
   - No painel do Pinecone, vá para "API Keys"
   - Copie a API Key

   ![Pinecone - API Key](https://i.imgur.com/PRTUM58.png)

#### 2.3. Configuração da OpenAI

1. **Obter uma API Key da OpenAI**
   - Acesse [https://platform.openai.com](https://platform.openai.com)
   - Crie uma conta ou faça login
   - Vá para "API Keys"
   - Clique em "Create new secret key"
   - Dê um nome à chave e copie-a

   ![OpenAI - API Key](https://i.imgur.com/DIdQNdh.png)

### 3. Configuração do Projeto

#### 3.1. Criar o Arquivo .env

Crie um arquivo `.env` na raiz do projeto com o seguinte conteúdo:

```
OPENAI_API_KEY=sua_chave_da_openai
PINECONE_API_KEY=sua_chave_do_pinecone
NEON_DB_URL=sua_url_do_neon_db
OPENAI_API_BASE=https://api.openai.com/v1
```

Substitua os valores pelos que você obteve nas etapas anteriores.

### 4. Preparação do Banco de Dados

#### 4.1. Criar e Popular o Banco de Dados

Execute o script de criação e população do banco de dados:

```powershell
python tools/code_popular_neon.py
```

Este script irá:
- Conectar-se ao seu banco Neon DB
- Criar a tabela de chamados
- Inserir chamados de exemplo

> **Nota:** Certifique-se de que a conexão com o Neon DB está funcionando corretamente. Verifique os logs para confirmar que os chamados foram inseridos com sucesso.

#### 4.2. Criar as Tabelas Adicionais

O SQL para criar as tabelas está no arquivo `tools/neon_chamados.sql`. Você pode executar esses comandos diretamente no editor SQL do Neon DB, ou usar um cliente PostgreSQL como o psql:

```bash
psql "sua_url_do_neon_db" -f tools/neon_chamados.sql
```

Isso criará todas as tabelas necessárias para o sistema:
- `chamados`: Armazena os chamados iniciais
- `classificacao_setor`: Armazena a classificação por setor
- `classificacao_prioridade`: Armazena a classificação por prioridade
- `analise_tecnica`: Armazena a análise técnica detalhada
- `andamento_chamados`: Armazena o andamento dos chamados
- `relatorio_final`: Armazena os relatórios finais

### 5. Configuração do Pinecone

Execute o script para configurar o Pinecone:

```powershell
python tools/code_popular_pinecone.py
```

Este script irá:
- Conectar-se ao seu índice no Pinecone
- Configurar os embeddings iniciais para os chamados

## ▶️ Como Executar

Há duas maneiras principais de usar o sistema:

### Opção 1: Processar Todos os Bugs Abertos

```powershell
python agents/crewai_agents.py
```

Este comando processará todos os bugs com status "Aberto" no banco de dados e gerará relatórios em Markdown e HTML para cada um.

### Opção 2: Reprocessar um Bug Específico

```powershell
# Primeiro, reabra o bug específico (por exemplo, o bug com ID 1)
python tools/reprocessar_bug.py 1

# Em seguida, processe o bug reaberto
python agents/crewai_agents.py
```

Isso permite testar o sistema com bugs específicos sem precisar processar todos os bugs novamente.

## 🧩 Componentes Detalhados

### Agentes e suas Funções

#### AG1: Classificador de Componente
- **Função**: Analisa o bug e identifica o componente de software afetado
- **Comportamento**: Examina o conteúdo e classifica em Frontend, Backend, Database, DevOps, Security, Integration, UI/UX ou Infrastructure
- **Resultado**: Componente afetado + justificativa detalhada da classificação

#### AG2: Classificador de Severidade
- **Função**: Define a severidade do bug
- **Comportamento**: Avalia o impacto técnico e de negócio para classificar como Critico, Grave ou Menor
- **Resultado**: Nível de severidade + justificativa detalhada

#### AG3: Analista Técnico
- **Função**: Fornece análise técnica detalhada do bug
- **Comportamento**: Identifica a causa raiz, avalia o impacto técnico e propõe soluções viáveis
- **Resultado**: Análise técnica estruturada com causa raiz, impacto e soluções propostas

#### AG4: Gerenciador de Resolução
- **Função**: Coordena o processo de resolução do bug
- **Comportamento**: Define responsáveis, prazos e prioridades com base nas classificações anteriores
- **Resultado**: Plano de ação detalhado com responsável, prazo e status de andamento

#### AG5: Documentador de Bugs
- **Função**: Cria documentação estruturada do bug
- **Comportamento**: Integra todas as informações em formatos Markdown e HTML com diagramas Mermaid
- **Resultado**: Relatórios completos em múltiplos formatos salvos como arquivos locais (`bug_X_relatorio.md` e `bug_X_relatorio.html`)

### Arquitetura de Bancos de Dados

#### Banco de Dados Relacional (Neon DB)

```
┈────────────┈       ┈───────────────────┈       ┈──────────────────────┈
│             │       │                     │       │                        │
│    bugs     │──┈    │ classificacao_componente │       │ classificacao_severidade│
│             │  │    │                     │       │                        │
└─────────────┐  │    └───────────────────┐       └──────────────────────┐──┘
                 │                 ▲                              ▲
                 │                 │                              │
                 └─────────────────┴──────────────────────────────┘
                                   │
                                   │
                 ┌────────────────┐│┌────────────────┐    ┌──────────────┐
                 │                ││││                │    │              │
                 │analise_tecnica ││││andamento_chamados   │relatorio_final
                 │                ││││                │    │              │
                 └────────────────┘│└────────────────┘    └──────────────┘
                                   │                              ▲
                                   │                              │
                                   └──────────────────────────────┘
```

#### Banco de Vetores (Pinecone)
- Armazena embeddings de bugs para busca semântica
- Permite encontrar bugs similares rapidamente
- Utiliza embeddings da OpenAI para representação vetorial

### Fluxo dos Dados

```
┈────────────┈     ┈─────────┈     ┈─────────┈     ┈─────────┈
│    Bug      │────┐│          │────┐│          │────┐│          │
│  Reportado  │     │   AG1    │     │   AG2    │     │   AG3    │
│             │     │          │     │          │     │          │
└────────────┐     └─────────┐     └─────────┐     └─────────┐
                     Classificar      Definir          Análise
                     Componente      Severidade        Técnica
                         │               │                │
                         ▼               ▼                ▼
                    ┌─────────────────────────────────────────┐
                    │                                         │
                    │               Banco de                  │
                    │                Dados                    │
                    │                                         │
                    └─────────────────────────────────────────┘
                              ▲                ▲
                              │                │
                      ┈─────────┈      ┈──────────┈
                      │            │      │             │
                      │    AG4     │      │    AG5      │
                      │            │      │             │
                      └──────────┐      └───────────┐
                       Gerenciar          Documentar
                       Resolução             Bug
```

## 🔧 Solução de Problemas

### Problemas com a Conexão ao Neon DB

**Sintoma**: Erros como "could not connect to server" ou "timeout expired"

**Solução**:
1. Verifique se a string de conexão está correta no arquivo .env
2. Certifique-se de que seu IP está permitido no firewall do Neon DB
3. Verifique se o banco de dados está ativo (não em modo suspenso)

### Problemas com a API da OpenAI

**Sintoma**: Erros como "API key invalid" ou "Rate limit exceeded"

**Solução**:
1. Verifique se a API key está correta no arquivo .env
2. Verifique se há créditos disponíveis na sua conta
3. Implemente backoff exponencial para lidar com limites de taxa

### Problemas com o Pinecone

**Sintoma**: Erros ao conectar ou ao realizar operações no índice

**Solução**:
1. Verifique se a API key está correta
2. Confirme se o nome do índice está correto (deve ser "bugflow")
3. Verifique se o índice está ativo e configurado com a dimensão correta (1536)

## 📊 Exemplos de Uso

### Exemplo de Bug Processado

#### Entrada: Bug Original
```
"Erro 500 na API de autenticação após atualização. Ocorre apenas após o último deploy."
```

#### Processamento por Agentes

**AG1: Classificador de Componente**
```
Backend - O problema está relacionado à API de autenticação, que é um componente de backend do sistema.
```

**AG2: Classificador de Severidade**
```
Critico - Um erro 500 na API de autenticação impede os usuários de acessar o sistema, bloqueando funcionalidades essenciais.
```

**AG3: Analista Técnico**
```
Causa Raiz: Incompatibilidade entre a versão atualizada da API e a configuração do servidor.
Impacto: Falha na autenticação de usuários, impedindo acesso ao sistema.
Solução: Reverter para a versão anterior ou corrigir a configuração do servidor para compatibilidade.
```

**AG4: Gerenciador de Resolução**
```
Status: Em desenvolvimento
Responsável: João Silva
Prazo: 2 dias
Prioridade: Alta - Bloqueia acesso ao sistema
```

**AG5: Documentador de Bugs**
```markdown
# Bug #1234 - Erro 500 na API de Autenticação após Atualização

## Descrição
O sistema apresenta erro 500 na API de autenticação após a última atualização, impedindo que os usuários façam login no sistema.

## Componente Afetado
Backend - O problema está relacionado à API de autenticação, que é um componente de backend do sistema.

## Severidade
Critico - Este erro impede o acesso ao sistema, bloqueando completamente a utilização por parte dos usuários.

## Análise Técnica
- Causa Raiz: Incompatibilidade entre a versão atualizada da API e a configuração do servidor.
- Impacto: Falha na autenticação de usuários, impedindo acesso ao sistema.
- Solução: Reverter para a versão anterior ou corrigir a configuração do servidor para compatibilidade.

## Resolução
- Desenvolvedor: João Silva
- Prazo: 2 dias
- Status: Em desenvolvimento

## Lições Aprendidas
Implementar testes de integração mais abrangentes antes de realizar deploys em produção e estabelecer um processo de rollback automático em caso de falhas críticas.
```

Além do relatório em Markdown, o sistema também gera um relatório em HTML com um diagrama Mermaid interativo mostrando o fluxo de resolução do bug.

## ❓ FAQ

### Posso usar outros modelos além do GPT-4?

Sim, você pode configurar outros modelos no arquivo de agentes. Para isso, altere o parâmetro `model` na configuração do LLM:

```python
llm = ChatOpenAI(
    api_key=OPENAI_API_KEY,
    base_url=OPENAI_API_BASE,
    model="gpt-4o-mini",  # Altere para o modelo desejado
    temperature=0.7
)
```

### Como adicionar novos agentes ao sistema?

Para adicionar um novo agente:

1. Defina o agente no arquivo `agents/crewai_agents.py`:
```python
AG6 = Agent(
    name="Nome do Agente",
    role="Papel do Agente",
    goal="Objetivo do Agente",
    backstory="História e comportamento do agente",
    llm=llm
)
```

2. Crie uma tarefa para o agente:
```python
Task_AG6 = Task(
    description="Descrição da tarefa",
    expected_output="Formato esperado de saída",
    agent=AG6
)
```

3. Adicione o agente à equipe:
```python
crew = Crew(
    agents=[AG1, AG2, AG3, AG4, AG5, AG6],  # Adicione o novo agente
    tasks=[Task_AG1, Task_AG2, Task_AG3, Task_AG4, Task_AG5, Task_AG6],  # Adicione a nova tarefa
    verbose=True,
    max_iter=15,
    memory=True
)
```

### Como personalizar as classificações de componentes?

Para alterar os componentes disponíveis, você precisa modificar:

1. O backstory do AG1 (Classificador de Componente):

```python
backstory="""Especialista em triagem de bugs para diferentes componentes de software.
Analiso o conteúdo do bug report e classifico baseado nas seguintes regras:
- Frontend: Problemas de UI, renderização, JavaScript, CSS, responsividade
- Backend: Problemas em APIs, processamento de dados, lógica de negócio
- Database: Problemas com banco de dados, queries, inconsistências
[adicione ou modifique outros componentes conforme necessário]"""
```

2. A função de normalização de componentes:

```python
def normalizar_componente(componente):
    # Valores permitidos exatos conforme a restrição CHECK na tabela
    valores_permitidos = ['Frontend', 'Backend', 'Database', 'DevOps', 'Security', 'Integration', 'UI/UX', 'Infrastructure']
    # Adicione ou modifique os valores permitidos conforme necessário
```

3. A restrição CHECK na tabela `classificacao_setor` no banco de dados:

```sql
CREATE TABLE classificacao_setor (
    id SERIAL PRIMARY KEY,
    chamado_id INT REFERENCES bugs(id),
    setor VARCHAR(50) NOT NULL CHECK (setor IN ('Frontend', 'Backend', 'Database', 'DevOps', 'Security', 'Integration', 'UI/UX', 'Infrastructure')),
    data_classificacao TIMESTAMP DEFAULT NOW()
);
```

---

## 📜 Licença

[MIT License](LICENSE) - Sinta-se livre para usar, modificar e distribuir conforme as regras da licença.

## 🛠️ Melhorias Recentes

1. **Normalização Robusta de Dados**: Implementação de funções de normalização para garantir que os valores gerados pelos agentes estejam em conformidade com as restrições do banco de dados.

2. **Persistência Redundante**: Os relatórios agora são salvos tanto no banco de dados quanto como arquivos locais, garantindo que os dados não sejam perdidos mesmo em caso de problemas com o banco de dados.

3. **Tratamento de Erros Aprimorado**: Cada operação de banco de dados agora tem seu próprio tratamento de erros, permitindo que o sistema continue funcionando mesmo se uma parte falhar.

4. **Documentação Visual**: Adição de diagramas Mermaid interativos nos relatórios HTML para melhor visualização do fluxo de resolução de bugs.

## 📮 Contato

Para dúvidas ou sugestões, entre em contato através de [email@exemplo.com](mailto:email@exemplo.com).
