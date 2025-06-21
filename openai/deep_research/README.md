# Deep Research Project

A sophisticated multi-agent research system that automates the entire research process from query planning to report generation and email delivery. Built using OpenAI's Agent SDK and Gradio for a user-friendly interface.

## 🏗️ Architecture Overview

The Deep Research project implements a multi-agent architecture where specialized AI agents work together to perform comprehensive research tasks:

```
Query Input → Planner Agent → Search Agent → Writer Agent → Email Agent
     ↓              ↓              ↓             ↓            ↓
  Gradio UI    Search Plan    Web Results   Final Report   Email Delivery
```

## 🤖 Agent Components

### 1. **Planner Agent** (`planner_agent.py`)
- **Purpose**: Analyzes research queries and creates strategic search plans
- **Functionality**: 
  - Generates 5 targeted search terms for comprehensive coverage
  - Provides reasoning for each search query
  - Ensures diverse research angles
- **Output**: `WebSearchPlan` with structured search items

### 2. **Search Agent** (`search_agent.py`)
- **Purpose**: Performs web searches and summarizes findings
- **Functionality**:
  - Uses OpenAI's WebSearchTool for real-time web searches
  - Creates concise 2-3 paragraph summaries (under 300 words)
  - Focuses on essential information for report synthesis
- **Tools**: WebSearchTool with low context size for efficiency

### 3. **Writer Agent** (`writer_agent.py`)
- **Purpose**: Synthesizes research findings into comprehensive reports
- **Functionality**:
  - Creates detailed 5-10 page reports (1000+ words)
  - Generates structured outlines before writing
  - Provides short summaries and follow-up questions
- **Output**: `ReportData` with markdown report, summary, and follow-up questions

### 4. **Email Agent** (`email_agent.py`)
- **Purpose**: Delivers research reports via email
- **Functionality**:
  - Converts markdown reports to HTML format
  - Sends professionally formatted emails via SendGrid
  - Provides delivery status confirmation
- **Dependencies**: SendGrid API for email delivery

## 🎯 Core Components

### Research Manager (`research_manager.py`)
The orchestrator that coordinates all agents and manages the research workflow:

- **Async Processing**: Handles concurrent search operations for efficiency
- **Progress Tracking**: Provides real-time status updates
- **Error Handling**: Gracefully manages failed searches
- **Trace Integration**: Links to OpenAI's trace system for debugging

### Web Interface (`deep_research.py`)
A Gradio-based user interface that provides:
- Clean, modern UI with sky-themed styling
- Real-time streaming of research progress
- Easy query input and report display
- Automatic browser launch

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- OpenAI API key
- SendGrid API key (for email functionality)
- Required Python packages (see dependencies below)

### Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd openai/deep_research
   ```

2. **Install dependencies**:
   ```bash
   pip install openai gradio python-dotenv sendgrid pydantic
   ```

3. **Set up environment variables**:
   Create a `.env` file with:
   ```
   OPENAI_API_KEY=your_openai_api_key_here
   SENDGRID_API_KEY=your_sendgrid_api_key_here
   ```

4. **Configure email settings** (in `email_agent.py`):
   - Update sender email: `from_email = Email("your-email@domain.com")`
   - Update recipient email: `to_email = To("recipient@domain.com")`

### Running the Application

```bash
python deep_research.py
```

The application will:
1. Launch a Gradio web interface
2. Automatically open in your default browser
3. Provide a text input for research queries
4. Display real-time progress updates
5. Show the final research report

## 📋 Usage Workflow

1. **Query Input**: Enter your research topic in the text box
2. **Planning Phase**: The planner agent creates 5 strategic search queries
3. **Search Phase**: Multiple concurrent web searches are performed
4. **Analysis Phase**: Search results are synthesized and analyzed
5. **Report Generation**: A comprehensive report is created with:
   - Detailed analysis (5-10 pages)
   - Executive summary
   - Follow-up research questions
6. **Email Delivery**: Report is automatically sent via email
7. **Display**: Final report is shown in the web interface

## 🔧 Configuration Options

### Search Configuration
- **Number of searches**: Modify `HOW_MANY_SEARCHES` in `planner_agent.py`
- **Search context size**: Adjust in `search_agent.py` WebSearchTool settings
- **Model selection**: Change model parameters in each agent

### Report Configuration
- **Report length**: Modify instructions in `writer_agent.py`
- **Summary length**: Adjust field descriptions in ReportData model

### Email Configuration
- **Email service**: Currently uses SendGrid (can be modified)
- **Email formatting**: HTML conversion handled by email agent
- **Recipient settings**: Configure in `email_agent.py`

## 🛠️ Technical Details

### Data Models
- **WebSearchItem**: Individual search query with reasoning
- **WebSearchPlan**: Collection of planned searches
- **ReportData**: Final report with summary and follow-up questions

### Async Processing
- Concurrent search execution for improved performance
- Real-time progress updates via async generators
- Error handling for failed searches

### Integration Points
- **OpenAI Agent SDK**: Core agent functionality
- **Gradio**: Web interface framework
- **SendGrid**: Email delivery service
- **Pydantic**: Data validation and serialization

## 🔍 Monitoring and Debugging

### Trace System
The application integrates with OpenAI's trace system:
- Each research session generates a unique trace ID
- View detailed execution traces at: `https://platform.openai.com/traces/trace?trace_id={trace_id}`
- Monitor agent interactions and tool usage

### Logging
- Console output for each phase of the research process
- Progress indicators for search completion
- Error messages for failed operations

## 🎨 Customization

### Adding New Agents
1. Create a new agent file following the existing pattern
2. Define instructions and output models
3. Integrate with ResearchManager workflow

### Modifying Search Strategy
- Adjust planner agent instructions for different search approaches
- Modify search agent for different content types
- Change search tool parameters for different contexts

### Custom Report Formats
- Modify writer agent instructions for different report styles
- Add new output fields to ReportData model
- Customize email formatting in email agent

## 📊 Performance Considerations

- **Concurrent Searches**: Multiple searches run simultaneously
- **Context Management**: Optimized for efficient token usage
- **Error Resilience**: Failed searches don't stop the entire process
- **Streaming Updates**: Real-time progress feedback

## 🔒 Security and Privacy

- API keys stored in environment variables
- No sensitive data logged or stored
- Email addresses configurable for privacy
- Secure API communication via OpenAI SDK

## 🤝 Contributing

To contribute to this project:
1. Follow the existing code structure and patterns
2. Maintain async/await patterns for consistency
3. Add appropriate error handling
4. Update documentation for new features
5. Test with various query types

## 📄 License

This project is part of the OpenAI SDK examples and follows the same licensing terms.

---

**Note**: This is a demonstration project showcasing multi-agent AI systems. For production use, consider additional error handling, rate limiting, and security measures. 