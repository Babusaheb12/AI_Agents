# AI_Agents

# 🤖 AI Agents with TypeScript & OpenAI Agents SDK

A hands-on project for learning and building **AI Agents with TypeScript and the OpenAI Agents SDK**.

This project explores how modern AI agents work, how they use tools, maintain context, delegate tasks, apply guardrails, and orchestrate multiple agents to solve complex problems.

The goal is not just to build a chatbot, but to understand how to build **reliable, tool-using, production-oriented AI agent systems** with TypeScript and Node.js.

---

## 🚀 About the Project

This repository is part of my journey into **Generative AI, Agentic AI, and AI-powered backend development**.

The project focuses on building AI agents using:

- TypeScript
- Node.js
- OpenAI Agents SDK
- OpenAI models
- Tools / Function Calling
- Context
- Guardrails
- Handoffs
- Multi-Agent Workflows
- Agent Orchestration
- MCP
- Sessions / Conversation History
- AI-assisted workflows

The OpenAI Agents SDK provides a lightweight set of primitives for creating agents that can use instructions and tools, delegate work to other agents, and apply guardrails around inputs, outputs, and tool execution.

---

## 🧠 What I'm Learning

### 1. What are AI Agents?

Understanding the difference between:

- Traditional LLM applications
- Chatbots
- Tool-using LLMs
- AI Agents
- Multi-Agent Systems

An agent can reason about a task, decide which tools it needs, execute those tools, inspect their results, and continue until it can provide a final answer.

---

### 2. Creating Agents

Learning how to create agents using the OpenAI Agents SDK.

An agent typically contains:

- Name
- Instructions
- Model
- Tools
- Context
- Guardrails
- Handoffs

```ts
import { Agent } from "@openai/agents";

const agent = new Agent({
  name: "Developer Assistant",
  instructions: "You are a helpful software development assistant.",
});
```

The SDK treats an agent as an LLM configured with instructions and optional tools that it can invoke to accomplish tasks.

---

## 🛠️ Tools

One of the most important concepts in agentic AI is **tool usage**.

Instead of only generating text, an agent can call functions to perform real actions.

Examples:

- Calculator
- Weather API
- Database queries
- Search
- File operations
- API requests
- GitHub operations
- Custom business logic

Example:

```ts
import { tool } from "@openai/agents";
import { z } from "zod";

const getUser = tool({
  name: "get_user",
  description: "Get user information",
  parameters: z.object({
    userId: z.string(),
  }),
  execute: async ({ userId }) => {
    return {
      userId,
      name: "Babu",
    };
  },
});
```

The SDK supports turning TypeScript functions into tools with schema-based validation.

---

## 🧩 Context

Context allows application-specific information to be passed through an agent workflow.

Examples:

```text
User information
Conversation information
Authentication information
Database information
Application state
Business configuration
```

Context can be used by:

- Agents
- Tools
- Guardrails
- Handoffs

This allows agents to work with information from the surrounding application rather than relying only on the user's message.

---

## 🔐 Guardrails

AI systems should not blindly trust every input or output.

Guardrails can be used to validate:

- User input
- Agent output
- Tool input
- Tool output

For example:

```text
User Input
    ↓
Input Guardrail
    ↓
Agent
    ↓
Tool
    ↓
Output Guardrail
    ↓
Final Response
```

The SDK supports input, output, and tool guardrails, including mechanisms that can stop execution when a validation check fails.

---

## 🔄 Agent Handoffs

Large agent systems can be divided into specialized agents.

For example:

```text
                    ┌── Billing Agent
                    │
User → Triage Agent ├── Support Agent
                    │
                    └── Developer Agent
```

The Triage Agent can decide which specialized agent should handle the request.

Example:

```ts
const triageAgent = Agent.create({
  name: "Triage Agent",
  handoffs: [billingAgent, supportAgent, developerAgent],
});
```

Handoffs allow one agent to delegate a conversation to another specialized agent.

---

## 🤝 Multi-Agent Architecture

This project explores multi-agent workflows such as:

```text
                User
                  │
                  ▼
           Triage Agent
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
   Coding Agent  Research  Support
        │         Agent      Agent
        ▼
      Tools
```

Each agent can have a specific responsibility instead of trying to make one large agent handle everything.

---

## 🔁 Agent Loop

A typical agent execution flow looks like:

```text
User Request
     ↓
Agent
     ↓
Model decides what to do
     ↓
Tool Call?
   ↙       ↘
 Yes        No
  ↓          ↓
Execute     Final
Tool       Response
  ↓
Tool Result
  ↓
Agent
  ↓
Continue / Final Response
```

The SDK runner manages this loop by processing model responses, executing tools, handling handoffs, and continuing until a final output is produced or the configured turn limit is reached.

---

## 🧠 Agents as Tools

Another powerful architecture is using one agent as a tool for another agent.

This allows a manager-style architecture where a main agent stays responsible for the conversation while specialized agents perform specific tasks.

Example:

```text
                    Manager Agent
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
        Researcher     Coder      Reviewer
           Agent        Agent        Agent
```

This is useful when you want specialization without completely transferring the conversation.

---

## 🔌 MCP

This project also explores the relationship between agents and **Model Context Protocol (MCP)**.

MCP allows agents to work with tools exposed by MCP servers.

Potential use cases include:

- GitHub
- Databases
- Filesystems
- Internal APIs
- Developer tools
- External services

The TypeScript Agents SDK provides MCP integrations alongside regular function tools.

---

## 💾 Sessions & Conversation History

Agents often need to remember previous interactions.

This project explores maintaining:

```text
Conversation
     ↓
User Message
     ↓
Agent
     ↓
Tool
     ↓
Result
     ↓
Next User Message
     ↓
Previous Context
```

The SDK provides session/conversation mechanisms for maintaining working context across agent runs.

---

## 👨‍💻 Human in the Loop

Not every action should happen automatically.

For sensitive operations, an agent may need human approval before executing an action.

Example:

```text
User
 ↓
Agent
 ↓
Sensitive Tool
 ↓
Human Approval
 ↓
Tool Execution
 ↓
Result
```

Potential examples:

- Sending an email
- Deleting data
- Making a payment
- Updating production systems
- Publishing content

---

## 📊 Tracing & Debugging

Agent workflows can become difficult to understand because one request may involve:

```text
Agent
 → Tool
 → Agent
 → Handoff
 → Tool
 → Final Response
```

Tracing helps visualize and debug these workflows.

The Agents SDK includes built-in tracing capabilities for observing agent runs and debugging workflows.

---

# 🏗️ Project Structure

```text
openai-agent-sdk-typescript/
│
├── src/
│   ├── agents/
│   │   ├── developer.agent.ts
│   │   ├── research.agent.ts
│   │   └── triage.agent.ts
│   │
│   ├── tools/
│   │   ├── calculator.tool.ts
│   │   ├── search.tool.ts
│   │   └── user.tool.ts
│   │
│   ├── guardrails/
│   │   ├── input.guardrail.ts
│   │   └── output.guardrail.ts
│   │
│   ├── context/
│   │   └── agent.context.ts
│   │
│   ├── workflows/
│   │   └── agent.workflow.ts
│   │
│   └── index.ts
│
├── .env
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

> The exact folder structure may evolve as the project grows.

---

# ⚙️ Tech Stack

| Technology            | Purpose                 |
| --------------------- | ----------------------- |
| **TypeScript**        | Application development |
| **Node.js**           | Runtime                 |
| **OpenAI Agents SDK** | Agent orchestration     |
| **OpenAI API**        | LLM capabilities        |
| **Zod**               | Schema validation       |
| **MCP**               | Tool integration        |
| **Git/GitHub**        | Version control         |

The current TypeScript Agents SDK documentation recommends installing `@openai/agents` with `zod`; the SDK currently supports Node.js 22+ according to the official repository.

---

# 📦 Installation

Clone the repository:

```bash
git clone <your-repository-url>
```

Move into the project:

```bash
cd openai-agent-sdk-typescript
```

Install dependencies:

```bash
npm install
```

The official SDK installation is:

```bash
npm install @openai/agents zod
```

---

# 🔑 Environment Variables

Create a `.env` file:

```env
OPENAI_API_KEY=your_openai_api_key
```

Never commit your API key to GitHub.

Add `.env` to `.gitignore`:

```gitignore
node_modules
.env
dist
```

---

# ▶️ Running the Project

Start the development server:

```bash
npm run dev
```

Build the project:

```bash
npm run build
```

Run the production build:

```bash
npm start
```

> Update these commands according to the scripts defined in `package.json`.

---

# 🧪 Learning Roadmap

This project is being developed step-by-step.

### Phase 1 — Fundamentals

- [x] What are AI Agents?
- [x] Basic Agent
- [x] Instructions
- [x] Agent Runner
- [ ] Agent Context
- [ ] Tool Calling

### Phase 2 — Tools

- [ ] Function Tools
- [ ] Tool Schemas
- [ ] API Tools
- [ ] Database Tools
- [ ] Custom Developer Tools

### Phase 3 — Reliability

- [ ] Input Guardrails
- [ ] Output Guardrails
- [ ] Tool Guardrails
- [ ] Error Handling
- [ ] Human Approval

### Phase 4 — Multi-Agent Systems

- [ ] Agent Handoffs
- [ ] Agents as Tools
- [ ] Triage Agent
- [ ] Specialized Agents
- [ ] Multi-Agent Orchestration

### Phase 5 — Advanced Agents

- [ ] Sessions
- [ ] Conversation History
- [ ] MCP
- [ ] Tracing
- [ ] Evaluation
- [ ] Production Architecture

### Phase 6 — Real-World Project

Build a complete AI Developer Assistant capable of:

```text
User
 ↓
AI Developer Assistant
 ↓
Understand Request
 ↓
Choose Agent
 ↓
Use Tools
 ↓
Inspect Results
 ↓
Validate Output
 ↓
Return Solution
```

---

# 💡 Planned Real-World Features

The final project can evolve into an **AI Developer Assistant** with capabilities such as:

- Explain code
- Debug errors
- Generate code
- Refactor code
- Review pull requests
- Generate tests
- Search documentation
- Analyze project files
- Work with Git repositories
- Run developer tools
- Delegate tasks to specialized agents
- Validate generated results
- Ask for human approval before sensitive operations

---

# 🎯 Project Goals

The main goals of this project are:

1. Understand how AI Agents work internally.
2. Learn the OpenAI Agents SDK with TypeScript.
3. Build tool-using agents.
4. Understand context and sessions.
5. Implement guardrails.
6. Learn multi-agent orchestration.
7. Understand agent handoffs.
8. Integrate MCP-based tools.
9. Build reliable AI workflows.
10. Create a production-oriented AI application.

---

# 📚 Official Resources

- **OpenAI Agents SDK – TypeScript Documentation**
  https://openai.github.io/openai-agents-js/

- **OpenAI Agents SDK – GitHub**
  https://github.com/openai/openai-agents-js

- **OpenAI Agents SDK – Agents Guide**
  https://openai.github.io/openai-agents-js/guides/agents/

- **Guardrails Guide**
  https://openai.github.io/openai-agents-js/guides/guardrails/

- **Handoffs Guide**
  https://openai.github.io/openai-agents-js/guides/handoffs/

- **Running Agents Guide**
  https://openai.github.io/openai-agents-js/guides/running-agents/

---

# 🎥 Learning Resources

This project is inspired by the **Building AI Agents with TypeScript and OpenAI Agent SDK** learning series by Piyush Garg.

Topics covered include:

- What are AI Agents?
- How to build AI Agents
- Building AI Agents using TypeScript
- OpenAI Agents SDK
- Google Agent Development Kit
- Agent tools
- Context
- Guardrails
- Agent orchestration

---

# 👨‍💻 Author

**Babu Saheb**

Software Developer | Flutter | iOS | Node.js | AI

- Portfolio: https://babusaheb.vercel.app/
- GitHub: https://github.com/BabuSaheb12
- LinkedIn: https://linkedin.com/in/babu-saheb-608155239

---

# ⭐ Why This Project?

I am building this project to move beyond traditional API development and understand how **agentic AI systems** are designed, implemented, orchestrated, and made reliable.

The goal is to combine my existing **Node.js/backend development experience** with modern **Generative AI and Agentic AI technologies** and eventually build production-oriented AI applications.

---

## 📌 Status

🚧 **Currently in Development**

This repository is continuously evolving as I learn and implement new concepts from the OpenAI Agents SDK.

More agents, tools, workflows, and real-world use cases will be added over time.

---

## 📄 License

This project is intended for learning and experimentation.
