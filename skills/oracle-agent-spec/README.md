# Oracle Open Agent Specification

You are an expert in Oracle's Open Agent Specification for portable, framework-agnostic AI agent definitions.

## When to Use
- Defining agents that work across multiple platforms
- Creating portable agent specifications
- Implementing the AGENT Blueprint framework
- Enterprise agent standardization

## Overview

The Open Agent Specification is a **framework-agnostic** standard for defining AI agents. Agents defined in this format can be deployed on Oracle ADK, LangChain, AutoGen, or custom frameworks.

## Agent Definition Structure

```yaml
name: "Marketing Agent"
version: "1.0.0"
description: "Strategic brand management and campaign optimization"

llm_config:
  model_id: "llama-4-maverick"
  temperature: 0.7
  max_tokens: 2048

system_prompt: |
  You are an expert Marketing AI agent specializing in...

tools:
  - name: analyze_campaign
    description: Analyze marketing campaign metrics
    inputs:
      - campaign_id: string
      - date_range: string
    outputs:
      - roi: number
      - recommendations: list
```

## The AGENT Blueprint

| Letter | Component | Purpose |
|--------|-----------|---------|
| **A** | Architecture | System prompt, capabilities, communication style |
| **G** | Governance | Decision authority levels (full/collaborative/advisory) |
| **E** | Execution | Tools — single-purpose, composable |
| **N** | Network | Multi-agent orchestration pattern |
| **T** | Transformation | KPIs — efficiency, quality, business impact |

## Python SDK

```python
from pyagentspec.agent import Agent
from pyagentspec.llms import VllmConfig
from pyagentspec.tools import ServerTool
from pyagentspec.serialization import AgentSpecSerializer

agent = Agent(
    name="Finance Agent",
    llm_config=VllmConfig(
        name="finance-llm",
        model_id="llama-4-maverick",
        url="http://your-llm-endpoint"
    ),
    system_prompt="...",
    tools=[...],
)

# Export to JSON/YAML
serializer = AgentSpecSerializer()
agent_yaml = serializer.to_yaml(agent)
```

## Resources
- [Oracle Agent Spec GitHub](https://github.com/oracle/agent-spec)
- [pyagentspec](https://pypi.org/project/pyagentspec/)
