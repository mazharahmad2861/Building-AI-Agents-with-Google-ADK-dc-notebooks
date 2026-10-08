# Google ADK 

A beginner-friendly learning guide to the core concepts and components of **Google Agent Development Kit (ADK)**.

The goal of this README is to understand **how ADK works**.

---

## 1. What is Google ADK?

**Google ADK (Agent Development Kit)** is a framework for building AI agents and multi-agent applications.

An agent can:

- Understand user requests
- Generate responses using an LLM
- Use tools
- Access external information
- Maintain conversation context
- Delegate tasks to other agents

Basic idea:

```text
User → Agent → Model / Tools / Sub-agents → Response
```

---

# 2. Core ADK Components

## `LlmAgent`

The main agent class used to create an LLM-powered agent.

Important parameters:

| Parameter | Purpose |
|---|---|
| `name` | Unique name of the agent |
| `model` | LLM used by the agent |
| `description` | Describes the agent's capability |
| `instruction` | Defines the agent's behavior |
| `tools` | Tools available to the agent |
| `sub_agents` | Other agents it can delegate to |

**Remember:**

> `instruction` = how the agent behaves  
> `description` = what the agent does

---

## Model

The LLM responsible for understanding and generating responses.

The model is the **brain**, while the agent provides the surrounding instructions, tools and context.

---

## `FunctionTool`

Converts a Python function into a tool that an agent can use.

For example, a function that searches an FAQ database can be exposed through `FunctionTool`.

**Remember:**

> Tool = capability the agent can use.

---

## Tools

Tools allow agents to interact with things outside the LLM.

Examples:

- APIs
- Databases
- Search
- Calculators
- Files
- Knowledge bases

The important concept is:

```text
Agent decides → Tool executes → Result returns to Agent
```

---

## `types.Content` and `types.Part`

ADK uses structured objects to represent messages.

- `Content` → represents a message/content object
- `Part` → represents individual pieces of content, such as text

You don't need to memorize their implementation initially. Just understand that ADK uses **structured message objects**.

---

# 3. Sessions & Runtime

## `InMemorySessionService()`

A session service manages conversation sessions and their context.

`InMemorySessionService` stores session information **in memory**, making it useful for learning and experimentation.

Think:

```text
User conversation
       ↓
Session
       ↓
Session Service
```

For production systems, persistent storage may be required.

---

## Session

A session represents a particular conversation between a user and the agent.

Important concepts:

- `app_name` → identifies the application
- `user_id` → identifies the user
- `session_id` → identifies a particular conversation

---

## `Runner`

The `Runner` is responsible for **executing the agent**.

Important parameters:

- `app_name`
- `agent`
- `session_service`

Mental model:

```text
User Input
    ↓
  Runner
    ↓
  Agent
    ↓
Model / Tools / Sub-agents
    ↓
Response
```

---

## Events

Agent execution can produce multiple **events**.

An event can represent:

- Agent activity
- Tool calls
- Tool results
- Intermediate responses
- Final response

`event.is_final_response()` is useful for identifying the final response.

---

# 4. Guardrails & Safety

## Guardrails

Guardrails control the agent's behavior.

Example:

> Only answer customer-support questions.

They can help with:

- Scope control
- Safety
- Output restrictions
- Tool restrictions

**Important:** Instructions can provide behavioral guardrails, but critical rules may require programmatic validation.

---

## `types.SafetySetting(...)`

Used to configure safety behavior for model generation.

It is related to controlling how the model handles potentially unsafe content.

Think:

> **SafetySetting = model safety rules**

---

## `types.GenerateContentConfig(...)`

Controls how the model generates content.

Important parameters include:

| Parameter | Purpose |
|---|---|
| `temperature` | Controls randomness |
| `max_output_tokens` | Limits output length |
| `top_p` | Controls token probability selection |
| `safety_settings` | Configures safety behavior |

Think:

> **GenerateContentConfig = model generation configuration**

---

# 5. Knowledge Base

A knowledge base contains information the agent can use.

Examples:

- FAQs
- Company policies
- Product information
- Documents

Important distinction:

```text
Knowledge Base = Information
Tool = Access to information
Agent = Decides when to use it
```

A simple FAQ dictionary is a basic knowledge source.

It is **not necessarily RAG**.

---

# 6. Multi-Agent Systems

A complex application can contain multiple specialized agents.

Example:

```text
Root Agent
 ├── Greeting Agent
 ├── Account Agent
 └── FAQ Agent
```

Each agent should have a clear responsibility.

---

## Root Agent

The main/entry agent.

It can:

- Understand the request
- Decide which agent should handle it
- Delegate tasks
- Handle fallback cases

Think of it as the **manager/router**.

---

## Sub-Agent

A specialized agent controlled by another agent.

For example:

- Greeting Agent → greetings
- Account Agent → account problems
- FAQ Agent → FAQ questions

---

## Delegation

Delegation means passing a task from one agent to another.

```text
User
 ↓
Root Agent
 ↓
Specialist Agent
 ↓
Response
```

### Delegation vs Tool

**Tool:**  
> "Use this capability."

**Delegation:**  
> "Let another agent handle this task."

---

# 7. Important Components — Quick Reference

| Component | Purpose |
|---|---|
| `LlmAgent` | Creates an LLM-powered agent |
| `model` | LLM used by the agent |
| `instruction` | Defines agent behavior |
| `description` | Describes agent capability |
| `FunctionTool` | Converts a function into an agent tool |
| `types.Content` | Represents message content |
| `types.Part` | Represents a piece of content |
| `InMemorySessionService` | Manages sessions in memory |
| `Runner` | Executes the agent |
| `Event` | Represents execution activity |
| `SafetySetting` | Controls model safety behavior |
| `GenerateContentConfig` | Controls model generation |
| `sub_agents` | Defines child/specialist agents |
| `tools` | Defines available agent capabilities |

---

# 8. Things to Remember

### Start simple

Learn in this order:

```text
LlmAgent
 ↓
Model + Instructions
 ↓
Tools
 ↓
Sessions
 ↓
Runner + Events
 ↓
Guardrails
 ↓
Multi-Agent / Delegation
```

### Keep agent responsibilities clear

Don't create one giant agent that does everything.

### Don't add tools unnecessarily

A tool should provide a real capability.

### Don't use multi-agent architecture unnecessarily

Multiple agents increase complexity. Use them when specialization provides a real benefit.

### Understand the architecture 

The most important mental model is:

```text
Agent = Decision maker
Model = Intelligence
Tool = Capability
Session = Context
Runner = Execution
Guardrail = Control
Sub-agent = Specialist
Root Agent = Coordinator
```

---

## Final Takeaway

Google ADK provides the components needed to build AI agents:

**Agents + Models + Tools + Sessions + Runtime + Safety + Multi-Agent orchestration**

Focus first on understanding **what each component does and why it exists**. The API and code become much easier once the architecture is clear.
