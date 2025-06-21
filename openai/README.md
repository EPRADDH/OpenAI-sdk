# OpenAI Agent SDK Learning Project

A comprehensive learning journey through OpenAI's Agent SDK, demonstrating the evolution from basic agent concepts to sophisticated multi-agent systems with real-world applications.

## 🎯 Project Overview

This project contains three progressive learning modules that showcase the power and simplicity of OpenAI's Agent SDK:

1. **First-look-AgentSDK** - Basic agent concepts and setup
2. **first-Agentic-Framework-project** - Multi-agent workflows with tools and handoffs
3. **Deep Search Query -Q&A** - Advanced research automation system

## 📚 Learning Modules

### Module 1: First Look at Agent SDK (`First-look-AgentSDK.ipynb`)

**Objective**: Introduction to basic agent concepts and OpenAI Agent SDK fundamentals.

#### Key Concepts Covered:
- **Agent Creation**: Simple agent setup with name, instructions, and model
- **Basic Execution**: Using `Runner.run()` for agent execution
- **Tracing**: Integration with OpenAI's trace system for debugging
- **Async Operations**: Understanding async/await patterns

#### Code Examples:
```python
from agents import Agent, Runner, trace

# Create a simple agent
agent = Agent(
    name="Jokester", 
    instructions="You are a joke teller", 
    model="gpt-4o-mini"
)

# Execute with tracing
with trace("Telling a joke"):
    result = await Runner.run(agent, "Tell a joke about AI")
    print(result.final_output)
```

#### Packages Used:
- `agents.Agent`: Core agent class for creating AI agents
- `agents.Runner`: Execution engine for running agents
- `agents.trace`: Tracing functionality for debugging

### Module 2: Agentic Framework Project (`first-Agentic-Framework-project.ipynb`)

**Objective**: Building sophisticated multi-agent systems with tools, handoffs, and real-world integrations.

#### Key Concepts Covered:

##### 1. **Agent Workflows**
- Multiple specialized agents working together
- Parallel agent execution for efficiency
- Agent selection and evaluation

##### 2. **Function Tools**
- Converting Python functions to agent tools using `@function_tool`
- External API integrations (SendGrid for email)
- Tool parameter validation and documentation

##### 3. **Agent Collaboration**
- Agents as tools using `agent.as_tool()`
- Handoffs for agent delegation
- Complex workflow orchestration

#### Advanced Features:

##### **Function Tools Example:**
```python
from agents import function_tool
import sendgrid

@function_tool
def send_email(body: str):
    """ Send out an email with the given body to all sales prospects """
    sg = sendgrid.SendGridAPIClient(api_key=os.environ.get('SENDGRID_API_KEY'))
    # Email sending logic
    return {"status": "success"}
```

##### **Agent as Tool:**
```python
# Convert agent to tool
tool1 = sales_agent1.as_tool(
    tool_name="sales_agent1", 
    tool_description="Write a cold sales email"
)
```

##### **Handoffs:**
```python
# Agent with handoff capability
emailer_agent = Agent(
    name="Email Manager",
    instructions="Convert email to HTML and send it",
    tools=[html_tool, send_html_email],
    handoff_description="Convert an email to HTML and send it"
)
```

#### Packages Used:
- `agents.function_tool`: Decorator for converting functions to tools
- `agents.Agent.as_tool()`: Method for converting agents to tools
- `agents.trace`: Advanced tracing with parallel execution
- `sendgrid`: Email delivery integration

### Module 3: Deep Search Query & Q&A (`Deep Search Query -Q&A.ipynb`)

**Objective**: Building a comprehensive research automation system with structured outputs and web search capabilities.

#### Key Concepts Covered:

##### 1. **OpenAI Hosted Tools**
- `WebSearchTool`: Real-time web search capabilities
- `FileSearchTool`: Vector store information retrieval
- `ComputerTool`: Computer automation tasks

##### 2. **Structured Outputs**
- Pydantic models for type-safe responses
- Field descriptions for better agent understanding
- Complex data structures with validation

##### 3. **Multi-Agent Research System**
- Planner agent for search strategy
- Search agent for web research
- Writer agent for report generation
- Email agent for delivery

#### Advanced Features:

##### **Structured Outputs Example:**
```python
from pydantic import BaseModel, Field

class WebSearchItem(BaseModel):
    reason: str = Field(description="Your reasoning for why this search is important")
    query: str = Field(description="The search term to use for the web search")

class WebSearchPlan(BaseModel):
    searches: list[WebSearchItem] = Field(description="List of web searches to perform")

planner_agent = Agent(
    name="PlannerAgent",
    instructions="Plan web searches for research queries",
    model="gpt-4o-mini",
    output_type=WebSearchPlan,  # Structured output
)
```

##### **Web Search Integration:**
```python
from agents import WebSearchTool, ModelSettings

search_agent = Agent(
    name="Search agent",
    instructions="Search the web and summarize results",
    tools=[WebSearchTool(search_context_size="low")],
    model="gpt-4o-mini",
    model_settings=ModelSettings(tool_choice="required"),
)
```

#### Packages Used:
- `agents.WebSearchTool`: OpenAI's hosted web search tool
- `agents.ModelSettings`: Advanced model configuration
- `pydantic.BaseModel`: Structured output definitions
- `asyncio`: Concurrent search execution

## 🛠️ Core Agent SDK Packages

### Essential Imports
```python
from agents import (
    Agent,           # Core agent class
    Runner,          # Execution engine
    trace,           # Tracing functionality
    function_tool,   # Function-to-tool converter
    WebSearchTool,   # Web search capability
    ModelSettings    # Advanced model configuration
)
```

### Key Package Features

#### 1. **Agent Class**
- **Purpose**: Create AI agents with specific roles and capabilities
- **Key Parameters**:
  - `name`: Agent identifier
  - `instructions`: Role and behavior definition
  - `model`: OpenAI model to use
  - `tools`: List of available tools
  - `handoffs`: Other agents this agent can delegate to
  - `output_type`: Pydantic model for structured outputs

#### 2. **Runner Class**
- **Purpose**: Execute agents and manage their lifecycle
- **Methods**:
  - `Runner.run()`: Standard execution
  - `Runner.run_streamed()`: Streaming execution for real-time output
  - `asyncio.gather()`: Parallel execution of multiple agents

#### 3. **Function Tools**
- **Purpose**: Convert Python functions into agent-usable tools
- **Features**:
  - Automatic JSON schema generation
  - Parameter validation
  - Documentation extraction
  - Error handling

#### 4. **Tracing System**
- **Purpose**: Debug and monitor agent execution
- **Features**:
  - Unique trace IDs for each session
  - Detailed execution logs
  - OpenAI platform integration
  - Performance monitoring

## 🚀 Project Setup

### Prerequisites
- Python 3.8+
- OpenAI API key
- SendGrid API key (for email functionality)
- Required packages (see installation below)

### Installation

1. **Install Dependencies**:
   ```bash
   pip install openai gradio python-dotenv sendgrid pydantic asyncio
   ```

2. **Environment Setup**:
   Create a `.env` file:
   ```
   OPENAI_API_KEY=your_openai_api_key_here
   SENDGRID_API_KEY=your_sendgrid_api_key_here
   ```

3. **SendGrid Configuration**:
   - Create SendGrid account at https://sendgrid.com/
   - Generate API key in Settings → API Keys
   - Verify sender email in Settings → Sender Authentication

### Running the Modules

1. **Start with Module 1**:
   ```bash
   jupyter notebook First-look-AgentSDK.ipynb
   ```

2. **Progress to Module 2**:
   ```bash
   jupyter notebook first-Agentic-Framework-project.ipynb
   ```

3. **Complete with Module 3**:
   ```bash
   jupyter notebook "Deep Search Query -Q&A.ipynb"
   ```

## 📊 Learning Progression

### Beginner Level (Module 1)
- ✅ Basic agent creation and execution
- ✅ Understanding async/await patterns
- ✅ Introduction to tracing system
- ✅ Simple agent interactions

### Intermediate Level (Module 2)
- ✅ Multi-agent workflows
- ✅ Function tool creation
- ✅ Agent collaboration patterns
- ✅ External API integration
- ✅ Handoffs and delegation

### Advanced Level (Module 3)
- ✅ Structured outputs with Pydantic
- ✅ Web search integration
- ✅ Complex multi-agent systems
- ✅ Concurrent execution
- ✅ Real-world automation

## 🔧 Configuration Options

### Model Selection
```python
# Different models for different use cases
agent = Agent(
    model="gpt-4o-mini",      # Fast, cost-effective
    # model="gpt-4o",         # More capable, higher cost
    # model="gpt-3.5-turbo",  # Legacy, still effective
)
```

### Tool Configuration
```python
# Web search with different context sizes
WebSearchTool(search_context_size="low")    # Cost-effective
WebSearchTool(search_context_size="medium") # Balanced
WebSearchTool(search_context_size="high")   # Comprehensive
```

### Model Settings
```python
from agents.model_settings import ModelSettings

agent = Agent(
    model_settings=ModelSettings(
        tool_choice="required",  # Force tool usage
        temperature=0.7,         # Creativity level
        max_tokens=1000,         # Response length
    )
)
```

## 🎯 Real-World Applications

### Sales Automation
- Cold email generation and sending
- Lead qualification and follow-up
- Multi-channel outreach campaigns

### Research Automation
- Market research and analysis
- Competitive intelligence gathering
- Technical documentation research

### Content Creation
- Multi-step content generation
- Research-backed article writing
- Automated report creation

### Customer Support
- Multi-agent support workflows
- Email response automation
- Issue escalation and resolution

## 🔍 Monitoring and Debugging

### Trace System
- View detailed execution traces at: https://platform.openai.com/traces
- Monitor agent interactions and tool usage
- Debug complex multi-agent workflows

### Best Practices
1. **Start Simple**: Begin with basic agents before complex workflows
2. **Use Tracing**: Always enable tracing for debugging
3. **Error Handling**: Implement proper error handling for production use
4. **Rate Limiting**: Be mindful of API rate limits
5. **Cost Management**: Monitor usage and optimize for cost efficiency

## 🚨 Important Notes

### Cost Considerations
- WebSearchTool costs ~$0.025 per call
- Monitor usage to avoid unexpected charges
- Consider using alternative search tools for high-volume applications

### Rate Limits
- OpenAI has rate limits on API calls
- Implement retry logic for production applications
- Consider using different models to distribute load

### Security
- Store API keys in environment variables
- Never commit sensitive credentials to version control
- Use verified sender emails for SendGrid

## 🤝 Contributing

To extend this project:
1. Follow the existing code patterns and structure
2. Maintain async/await patterns for consistency
3. Add appropriate error handling and logging
4. Update documentation for new features
5. Test with various scenarios and edge cases

## 📄 License

This project is part of the OpenAI SDK examples and follows the same licensing terms.

---

**Note**: This is a learning project demonstrating OpenAI Agent SDK capabilities. For production use, implement additional security, error handling, and monitoring measures. 