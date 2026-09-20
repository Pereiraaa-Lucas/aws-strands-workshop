# Construa um Agente de IA de Produção: Workshop Prático de Strands Agents

Construa um agente de IA de atendimento ao cliente pronto para produção do zero com o SDK [Strands Agents](https://strandsagents.com/latest/?trk=87c4c426-cddf-4799-a299-273337552ad8&sc_channel=el) — o **SDK de infraestrutura de agentes** de código aberto — adicionando ferramentas, proteções (*guardrails*), memória, delegação multiagente, avaliações e implantação no [Amazon Bedrock AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/?trk=87c4c426-cddf-4799-a299-273337552ad8&sc_channel=el) um módulo de cada vez.

![Strands Agents](https://img.shields.io/badge/Strands_Agents-SDK-FF9900?logo=amazonaws&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Amazon Bedrock](https://img.shields.io/badge/Amazon_Bedrock-AgentCore-232F3E?logo=amazonaws&logoColor=white)
![License MIT-0](https://img.shields.io/badge/License-MIT--0-green.svg)

> Este exemplo funciona com Strands Agents e Amazon Bedrock AgentCore. O código neste repositório é fornecido "como está" e não é oficialmente suportado pela Amazon.

---

## O que você vai construir

Um único agente de atendimento ao cliente que cresce ao longo de 7 módulos. Cada módulo é um notebook independente (12–15 min) que adiciona uma capacidade de produção — começando a partir de um loop de agente básico e terminando com um agente implantado no AgentCore Runtime. Tempo total: cerca de 90 minutos.

## O que é uma infraestrutura de agente (*agent harness*)?

Uma **infraestrutura de agente** (*agent harness*) é o sistema que permite que um agente realmente funcione: o loop de orquestração que chama o modelo, decide qual ferramenta invocar, retorna os resultados, gerencia a janela de contexto e lida com falhas — além da infraestrutura subjacente (processamento, sandbox de código, conexões de ferramentas seguras, armazenamento persistente, memória, identidade e observabilidade).

**Strands Agents é o SDK de infraestrutura de agente de código aberto** — você não escreve apenas um prompt, você constrói e controla toda a infraestrutura (o loop, ferramentas, ganchos, memória, proteções) de ponta a ponta. Este workshop constrói isso camada por camada e, em seguida, implanta um agente no [Amazon Bedrock AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html) como a camada de hospedagem:

| Estágio | O que você faz | Com |
| :--- | :--- | :--- |
| Construir a infraestrutura | Montar o loop, ferramentas, ganchos, habilidades e memória — controlando cada camada | Strands Agents (Módulos 1–4) |
| Executar a infraestrutura | Operar a mesma infraestrutura em produção — processamento gerenciado, memória, identidade, observabilidade | Amazon Bedrock AgentCore Runtime (Módulo 5) |
| Ir mais longe (opcional) | Adicionar delegação multiagente e avaliações automatizadas sobre o agente implantado | Strands Agents (Módulos 6–7) |

Como a infraestrutura é orientada por configuração, testar um modelo diferente ou adicionar uma ferramenta é uma alteração de configuração, não uma reescrita de código.

## Módulos

| # | Módulo | Tempo | O que você vai construir |
| :--- | :--- | :--- | :--- |
| 1 | [Loop de Agente + Ferramentas](./samples/01-agent-loop-tools/) | 12 min | Agente de atendimento ao cliente com ferramentas de consulta, pedidos e reembolso |
| 2 | [Ganchos (*Hooks*)](./samples/02-hooks/) | 10 min | Limitador de taxa (*rate limiter*) que limita chamadas excessivas de ferramentas com código determinístico |
| 3 | [Habilidades + Direcionamento (*Skills + Steering*)](./samples/03-skills-steering/) | 15 min | Habilidades de fluxo de trabalho, aplicação de reembolso e proteção de tom |
| 4 | [Gerenciadores de Sessão](./samples/04-session-managers/) | 10 min | Memória persistente que sobrevive a reinicializações |
| 5 | [Implantação (*Deploy*)](./samples/05-deploy/) | 15 min | Implantação no Amazon Bedrock AgentCore Runtime |
| 6 | [Multiagente](./samples/06-multi-agent/) *(opcional)* | 15 min | Delegação para um agente especialista em suporte técnico |
| 7 | [Avaliações (*Evals*)](./samples/07-evals/) *(opcional)* | 13 min | Teste automatizado de qualidade com LLM como juiz (*LLM-as-judge*) |

Ferramentas simuladas compartilhadas usadas em todos os módulos estão em [`samples/shared/`](./samples/shared/).

---

## Como começar?

O caminho mais rápido é abrir o Módulo 1 e executar as células do notebook de cima a baixo. O README de cada módulo explica o conceito e fornece links para o próximo.

```bash
# Clonar o repositório
git clone [https://github.com/aws-samples/sample-strands-agents-hands-on-workshop.git](https://github.com/aws-samples/sample-strands-agents-hands-on-workshop.git)
cd sample-strands-agents-hands-on-workshop

Em seguida, abra samples/01-agent-loop-tools/ no VS Code ou JupyterLab e execute o notebook.
Como configurar o ambiente?
Este workshop é executado em um ambiente hospedado do VS Code com dependências pré-instaladas. Para executar localmente:
# Criar e ativar um ambiente virtual
python3 -m venv .venv
source .venv/bin/activate

# Instalar dependências (ou usar o requirements.txt de cada módulo)
pip install strands-agents strands-agents-evals bedrock-agentcore

# Configurar credenciais da AWS (Strands usa o Amazon Bedrock por padrão)
aws configure

Cada módulo também possui seu próprio requirements.txt, permitindo instalar apenas o que aquele módulo precisa.
O Módulo 5 (Deploy) também precisa da CLI do AgentCore (Node.js 20+). Uma instalação global precisa de privilégios de root, portanto use sudo:
sudo npm install -g @aws/agentcore --ignore-scripts
agentcore --version

> --ignore-scripts ignora o script postinstall do pacote, que apenas emite um aviso sobre o antigo bedrock-agentcore-starter-toolkit. Sem ele, em algumas instalações do npm 10.8.x, esse script pode abortar a instalação e deixar o comando agentcore quebrado. Sempre confirme com agentcore --version - se exibir bash: agentcore: command not found, reexecute o comando de instalação acima.
> 
> Se você instalou anteriormente o bedrock-agentcore-starter-toolkit, desinstale-o (pip uninstall bedrock-agentcore-starter-toolkit) - ele fornece uma CLI agentcore mais antiga que entra em conflito com esta.
> 
Quais são os pré-requisitos?
| Requisito | Detalhe |
|---|---|
| Python | 3.10 ou superior |
| Credenciais da AWS | Acesso ao modelo Amazon Bedrock para o Claude Sonnet 5 |
| Módulo de Deploy (Módulo 5) | Node.js 20+, CLI @aws/agentcore, uv e AWS CDK; provisiona um AgentCore Runtime + empacotamento no Amazon S3 via CloudFormation |
Como funciona o loop do agente?
O loop do agente alterna entre o LLM e suas ferramentas até que o modelo tenha informações suficientes para responder: Usuário → LLM → Chamada de Ferramenta → Resultado da Ferramenta → LLM → Resposta. As ferramentas são funções simples em Python decoradas com @tool, e o LLM lê cada docstring para decidir quando chamá-las — sem necessidade de roteamento manual.
from strands import Agent, tool

@tool
def lookup_customer(customer_id: str) -> str:
    """Busca um cliente pelo seu ID."""
    ...

agent = Agent(tools=[lookup_customer], system_prompt=SYSTEM_PROMPT)
agent("Sou o C-1001. Quais são os meus pedidos recentes?")

Consulte o Módulo 1 para ver o passo a passo completo e uma inspeção do loop em ação.
Perguntas frequentes
Preciso concluir os módulos em ordem?
Trabalhe nos Módulos 1–5 em ordem — cada um se baseia no anterior, terminando com o agente implantado no AgentCore Runtime. Os Módulos 6 (Multi-Agent) e 7 (Evals) são extensões opcionais no mesmo agente; faça-os em qualquer ordem ou ignore-os.
Qual modelo Claude isto usa?
Os módulos usam o Claude Sonnet 5 por padrão via Amazon Bedrock. Você precisa ter o acesso ao modelo Bedrock habilitado em sua conta da AWS.
Posso executar isto localmente sem credenciais da AWS, usando o Ollama?
Sim. Instancie o Ollama, baixe um modelo que suporte o uso de ferramentas, instale o complemento Ollama do Strands e passe OllamaModel para Agent(...):
# 1. Instalar o Ollama (macOS: baixar DMG de ollama.com; Linux: curl abaixo; Windows: instalador de [ollama.com/download/windows](https://ollama.com/download/windows))
curl -fsSL [https://ollama.com/install.sh](https://ollama.com/install.sh) | sh   # Apenas Linux

# 2. Baixar um modelo com suporte a chamadas de ferramentas/funções
ollama pull llama3.1   # recomendado — amplamente testado com uso de ferramentas

# 3. Instalar o complemento Strands Ollama
pip install strands-agents[ollama]

from strands import Agent
from strands.models import OllamaModel

model = OllamaModel(host="http://localhost:11434", model_id="llama3.1")
agent = Agent(model=model, tools=[...], system_prompt=...)

Outros modelos com suporte a ferramentas: llama3.2, qwen2.5, qwen3, mistral. Consulte a biblioteca de modelos do Ollama para ver a lista completa. Cada notebook e o arquivo chat.py incluem um exemplo comentado.
Estou usando créditos fornecidos pela AWS de um evento patrocinado — como posso usá-los?
Os créditos da AWS emitidos para hackathons e workshops cobrem apenas modelos Amazon Nova, não o Claude. Para alternar qualquer agente para o Nova, importe BedrockModel e passe-o para Agent(...):
from strands.models import BedrockModel

model = BedrockModel(model_id="amazon.nova-pro-v1:0")
agent = Agent(model=model, tools=[...], system_prompt=...)

IDs de modelos Nova disponíveis — consulte os cartões de modelo do Amazon Bedrock para a lista completa:
| ID do Modelo | Descrição |
|---|---|
| amazon.nova-micro-v1:0 | Mais rápido, apenas texto, menor custo |
| amazon.nova-lite-v1:0 | Baixo custo, multimodal (texto, imagem, vídeo) |
| amazon.nova-pro-v1:0 | Precisão/velocidade equilibradas, multimodal (recomendado) |
Cada notebook e arquivo chat.py inclui um exemplo comentado mostrando exatamente onde fazer essa alteração.
Qual é a diferença entre um framework de agentes e uma infraestrutura de agentes (agent harness)?
Um framework fornece o loop de orquestração (chamadas de modelo, seleção de ferramentas, contexto). Uma infraestrutura é o sistema completo que permite que o agente seja executado: o loop mais o processamento, sandbox de código, conexões de ferramentas, memória, identidade e observabilidade.
Posso usar um framework diferente do Strands Agents?
Os padrões mostrados aqui — uso de ferramentas, ganchos, memória de sessão, transferência multiagente e avaliações com LLM como juiz — podem ser aplicados a qualquer infraestrutura de agentes. Este workshop implementa os padrões com o SDK Strands Agents.
Quanto tempo dura o workshop completo?
Cerca de 90 minutos para todos os 7 módulos. Cada módulo é independente e leva de 10 a 15 minutos.
Preciso de recursos da AWS para executar os primeiros módulos?
Você precisa de acesso ao Amazon Bedrock a partir do Módulo 1. Serviços adicionais (Amazon Bedrock AgentCore Runtime, Amazon S3) são necessários apenas para o módulo de implantação (Módulo 5).
Recursos
 * Documentação do Strands Agents
 * SDK do Strands Agents no GitHub
 * Documentação do Amazon Bedrock AgentCore
 * Infraestrutura AgentCore (infraestrutura de agente gerenciada)
 * Curso em Vídeo Completo — Aprofundamentos em todos os tópicos cobertos aqui
Contribuindo
Contribuições são bem-vindas! Consulte CONTRIBUTING para obter mais informações.
Segurança
Se você descobrir um potencial problema de segurança neste projeto, notifique a Segurança da AWS/Amazon por meio da página de relatório de vulnerabilidades. Por favor, não crie um problema público no GitHub.
Licença
Esta biblioteca está licenciada sob a Licença MIT-0. Consulte o arquivo LICENSE para obter detalhes.



