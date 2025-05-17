# 🤝 Claude Multi-Agent Communication Protocol (MCP)

This guide covers how Claude delegates tasks to other agents via MCP JSON requests.

---

## Request Format

```json
{
  "mcp_protocol_version": "1.0",
  "request_id": "req-abc",
  "service": "research_agent",
  "action": "analyze_market",
  "parameters": {
    "industry": "renewable_energy"
  },
  "context": {
    "user_objective": "investment_decision"
  },
  "response_format": {
    "type": "structured_report"
  }
}
```

---

## Example Workflow

1. User asks Claude a complex question  
2. Claude gathers basic data with tools  
3. Claude delegates specialized analysis via MCP  
4. The agent (MCP responder) performs:
   - its own tool calls  
   - return of analysis to Claude  

---

## Sample MCP Tool Call by Sub-Agent

```antml
<antml:function_calls>
  <antml:invoke name="solar_system_sizing">
    <antml:parameter name="location">Phoenix</antml:parameter>
  </antml:invoke>
</antml:function_calls>
```

---

## MCP Service Definition

```json
{
  "service_name": "content_creation_agent",
  "capabilities": ["blog_writing", "social_media_copy"],
  "input_requirements": {
    "content_type": "Required",
    "tone": "Required"
  },
  "output_formats": ["markdown", "html"]
}
```

---

## Implementation Notes

| Component         | Task                                           |
|-------------------|------------------------------------------------|
| Request Routing   | Match `service` key to appropriate sub-agent   |
| Response Handling | Wait for full result before resuming task      |
| Error Handling    | Validate MCP version, structure, completeness  |
