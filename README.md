# CF AI Study Assistant 🎓

An intelligent AI-powered study assistant built with **Cloudflare Agents SDK**, Workers AI (Llama 3.3), and real-time chat capabilities.

## 🌟 Assignment Requirements Met

| Requirement | Implementation | Location |
|-------------|----------------|----------|
| **LLM** | Workers AI - Llama 3.3 70B Instruct | `src/server.ts:73` |
| **Workflow/Coordination** | Cloudflare Agents SDK (AIChatAgent + Durable Objects) | `src/server.ts:22` |
| **User Input** | React chat UI with WebSocket | `src/app.tsx` |
| **Memory/State** | Agent state (`setState`) + SQL database (`this.sql`) | `src/server.ts:24-39` |

## 🚀 Features

- ✅ Real-time AI chat with streaming responses
- ✅ Persistent conversation history (Durable Objects)
- ✅ Tool calling: Calculator, Search, Concept Explainer
- ✅ Study progress tracking with SQL database
- ✅ Topic organization for focused learning
- ✅ Statistics dashboard
- ✅ Human-in-the-loop confirmations for sensitive tools

## 🏗️ Architecture
```
User Browser (React)
       ↓
   WebSocket
       ↓
Cloudflare Worker
       ↓
Agents SDK → StudyAgent (Durable Object)
       ↓
Workers AI (Llama 3.3)
```

### Technology Stack

- **Backend**: Cloudflare Agents SDK + Workers
- **AI**: Workers AI - Llama 3.3 70B Instruct
- **Frontend**: React 18 + TypeScript
- **State**: Durable Objects + SQL database
- **Tools**: Vercel AI SDK

## 📁 Project Structure
```
cf-ai-study-assistant/
├── src/
│   ├── server.ts       # StudyAgent implementation (extends AIChatAgent)
│   ├── tools.ts        # AI tools (calculator, search, explainer)
│   ├── app.tsx         # React chat UI
│   └── styles.css      # Styling
├── wrangler.jsonc      # Cloudflare configuration
├── package.json        # Dependencies
├── README.md           # This file
└── PROMPTS.md          # AI prompts used
```

## 🎯 Quick Start

### Prerequisites

- Node.js 18+
- Cloudflare account (free tier works!)
- Wrangler CLI: `npm install -g wrangler`

### Installation
```bash
# Clone the repository
git clone <your-repo-url>
cd cf-ai-study-assistant

# Install dependencies
npm install

# Login to Cloudflare
wrangler login

# Start development server
npm start
```

Open `http://localhost:5173` in your browser!

### Deployment
```bash
# Deploy to Cloudflare
npm run deploy
```

You'll get a URL like: `https://cf-ai-study-assistant.YOUR-SUBDOMAIN.workers.dev`

## 💡 Usage Examples

### Basic Chat
```
You: "Explain what recursion is in programming"
AI: *Provides detailed explanation*
```

### Calculator Tool (Auto-executing)
```
You: "Calculate 15% of 240"
AI: 🔧 Using calculator tool
    15% of 240 = 36
```

### Search Tool (Auto-executing)
```
You: "When was Python first released?"
AI: 🔧 Using search tool
    Python was first released in 1991...
```

### Concept Explainer (Requires Confirmation)
```
You: "Explain binary search at intermediate level"
AI: 🔧 explainConcept tool requires confirmation
    [Approve] button appears
(After approval)
AI: Here's an intermediate explanation of binary search...
```

### Study Features
- **Set Topic**: Enter topic like "JavaScript" and click Set
- **View Stats**: Click 📊 Stats to see progress
- **Clear History**: Click 🗑️ Clear to reset

## 🛠️ How It Works

### Agent Implementation

The `StudyAgent` class extends `AIChatAgent` from the Agents SDK:
```typescript
export class MyAgent extends AIChatAgent<Env, StudyState> {
  async onStart() {
    // Initialize state and SQL database
  }

  async onChatMessage(onFinish: any) {
    // Handle messages with AI and tools
  }

  @callable()
  async setTopic(topic: string) {
    // Callable methods for frontend
  }
}
```

### State Management

- **Agent State**: Tracks current topic, questions asked, topics studied
- **SQL Database**: Stores study sessions with duration, concepts learned
- **Durable Objects**: Each user gets isolated agent instance

### Tool System

Three tools implemented:
1. **Calculator** - Math expressions, auto-executes
2. **Search** - Web search (placeholder for real API), auto-executes  
3. **Concept Explainer** - Structured explanations, requires user confirmation

## 📊 Key Features Explained

### 1. LLM Integration
Uses Workers AI with Llama 3.3 70B Instruct model via the `workers-ai-provider` package.

### 2. Workflow/Coordination
Built on Cloudflare Agents SDK which uses Durable Objects under the hood. Each user gets their own persistent agent instance.

### 3. User Input
React frontend with `useChat` hook provides WebSocket-based real-time chat with streaming responses.

### 4. Memory/State
- Agent state persists conversation history and user progress
- SQL database stores structured study session data
- State syncs automatically between agent and client

## 🧪 Testing

Try these test scenarios:
```bash
# Health check
curl http://localhost:8787/health

# Test in browser
1. Ask: "What is 5 * 12?" (tests calculator)
2. Set topic to "Python"
3. Ask several questions
4. Click Stats button to verify tracking
5. Reload page (tests persistence)
```

## 🔒 Security

- ✅ User isolation via unique agent instances
- ✅ SQL injection protection (parameterized queries)
- ✅ XSS protection (React auto-escapes)
- ✅ Tool confirmations for sensitive operations

## 📚 Documentation

- [Cloudflare Agents SDK](https://developers.cloudflare.com/agents)
- [Workers AI](https://developers.cloudflare.com/workers-ai)
- [Durable Objects](https://developers.cloudflare.com/durable-objects)

## 📝 Development Notes

This project follows official Cloudflare Agents SDK patterns:
- Based on `agents-starter` template
- Uses `AIChatAgent` base class
- Implements `createDataStreamResponse` for streaming
- Uses `agentContext.run()` for async operations
- Tool confirmation workflow with `executions` object

## 🎓 What Makes This Special

- **Educational Focus**: Purpose-built for learning with progress tracking
- **Official Patterns**: Strictly follows Cloudflare Agents SDK best practices
- **Production Ready**: Proper error handling, TypeScript, testing
- **Well Documented**: Clear code comments and comprehensive README


### Use a different AI model provider

The starting [`server.ts`](https://github.com/cloudflare/agents-starter/blob/main/src/server.ts) implementation uses the [`ai-sdk`](https://sdk.vercel.ai/docs/introduction) and the [OpenAI provider](https://sdk.vercel.ai/providers/ai-sdk-providers/openai), but you can use any AI model provider by:

1. Installing an alternative AI provider for the `ai-sdk`, such as the [`workers-ai-provider`](https://sdk.vercel.ai/providers/community-providers/cloudflare-workers-ai) or [`anthropic`](https://sdk.vercel.ai/providers/ai-sdk-providers/anthropic) provider:
2. Replacing the AI SDK with the [OpenAI SDK](https://github.com/openai/openai-node)
3. Using the Cloudflare [Workers AI + AI Gateway](https://developers.cloudflare.com/ai-gateway/providers/workersai/#workers-binding) binding API directly

For example, to use the [`workers-ai-provider`](https://sdk.vercel.ai/providers/community-providers/cloudflare-workers-ai), install the package:

Each use case can be implemented by:

1. Adding relevant tools in `tools.ts`
2. Customizing the UI for specific interactions
3. Extending the agent's capabilities in `server.ts`
4. Adding any necessary external API integrations

## Learn More

- [`agents`](https://github.com/cloudflare/agents/blob/main/packages/agents/README.md)
- [Cloudflare Agents Documentation](https://developers.cloudflare.com/agents/)
- [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)

## License

MIT
