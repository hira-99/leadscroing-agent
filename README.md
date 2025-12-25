# Lead Scoring Agent

This is a **learning project** focused on exploring **LangGraph** - a framework for building stateful, multi-actor applications with LLMs. The project demonstrates how to create an AI-powered workflow for lead enrichment, scoring, and automated proposal generation.

## 🎯 Objective

The main objective of this project is to:
- Learn and practice building complex AI workflows using LangGraph
- Understand state management in multi-step AI applications
- Implement conditional routing and decision-making in agentic workflows
- Integrate multiple tools (browser automation, web search, email) into a cohesive system
- Build a practical lead scoring system that automates the sales development process

## 🔄 What's Happening

This project implements an **automated lead enrichment and scoring workflow** that:

1. **Takes Input**: Accepts a company name and website URL

2. **Website Research** (`website_research_node`):
   - Uses Playwright browser tools to navigate and explore the company's website
   - Visits key pages (homepage, about, jobs, news, team) to understand the company
   - Extracts relevant information about what the company does, their needs, and potential pain points

3. **News Research** (`news_research_node`):
   - Uses Google Serper API to search the web for latest news and events about the company
   - Visits news websites using browser tools to gather comprehensive information
   - Identifies buying signals like recent funding, hiring, or strategic initiatives

4. **Lead Scoring** (`lead_scoring_node`):
   - Analyzes the gathered research data against ByteMage's Ideal Customer Profile (ICP)
   - Scores the lead on two dimensions:
     - **Lead Score (0-5)**: How desirable the company is based on ICP alignment
     - **Likelihood Score (0-5)**: How likely it is to win this client
   - Categorizes the lead as DISQUALIFIED, COLD, WARM, or HOT
   - Suggests 2-3 relevant services the company might need

5. **Decision Routing**:
   - If lead score ≥ 3: Proceeds to send a proposal
   - If lead score < 3: Ends the workflow (lead not qualified)

6. **Proposal Generation & Sending** (`send_proposal_node`):
   - Crafts a tailored business proposal based on the research findings
   - Uses the suggested services and company insights to personalize the proposal
   - Automatically sends the proposal via email using Resend API

## 🏗️ Architecture

The workflow is built using **LangGraph's StateGraph**, which allows for:
- **State Management**: Centralized state using TypedDict that tracks messages, research results, and scoring outputs
- **Parallel Execution**: Website research and news research run in parallel for efficiency
- **Conditional Routing**: Dynamic decision-making based on lead scores
- **Tool Integration**: Seamless integration of browser tools, search tools, and email tools
- **Structured Outputs**: Using Pydantic models for type-safe lead scoring results

## 🛠️ Technologies Used

- **LangGraph**: For building the stateful workflow graph
- **LangChain**: For LLM integration and tool management
- **OpenAI GPT-4o-mini**: As the underlying LLM
- **Playwright**: For browser automation and website scraping
- **Google Serper API**: For web search capabilities
- **Resend API**: For sending emails
- **Pydantic**: For structured data validation and output

## 📊 Workflow Graph

The workflow follows this structure:
```
START
  ├─> website_research_node ──> [tools?] ──> website_research_tool_node ──┐
  │                                                                         │
  └─> news_research_node ──> [tools?] ──> news_research_tool_node ────────┼─> lead_scoring_node
                                                                             │
                                                                             ├─> [score ≥ 3?]
                                                                             │
                                                                             ├─> send_proposal_node ──> [tools?] ──> proposal_tool_node
                                                                             │
                                                                             └─> END
```

## 🎓 Learning Outcomes

Through this project, you'll learn:
- How to structure complex AI workflows with LangGraph
- State management patterns in multi-step AI applications
- Implementing conditional logic and routing in agentic systems
- Tool integration and orchestration
- Building production-ready AI agents with proper error handling and structured outputs
