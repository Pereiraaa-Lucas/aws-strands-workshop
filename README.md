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


