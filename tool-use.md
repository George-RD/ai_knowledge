# 🛠 Claude Tool Use Guide

This guide explains how Anthropic Claude uses tools via the `<antml:function_calls>` structure.

---

## Basic Format

```antml
<antml:function_calls>
  <antml:invoke name="weather_lookup">
    <antml:parameter name="location">Chicago</antml:parameter>
    <antml:parameter name="unit">fahrenheit</antml:parameter>
  </antml:invoke>
</antml:function_calls>
```

Tool responses are JSON-encoded, like:

```json
{
  "temperature": 72,
  "condition": "Partly Cloudy"
}
```

---

## Chained Tool Use

Claude can sequence multiple tools. Example: `flight_search` → `hotel_search` → `car_rental_search`. Each uses its own `<antml:function_calls>` block and receives structured JSON responses.

---

## Complex Parameters

Supports arrays and nested objects:

```antml
<antml:function_calls>
  <antml:invoke name="data_analysis">
    <antml:parameter name="filters">{"region":"NE","product_categories":["A","B"]}</antml:parameter>
  </antml:invoke>
</antml:function_calls>
```

---

## Tool Schema Definition

Each tool is defined in the system context:

```json
{
  "name": "flight_search",
  "parameters": {
    "type": "object",
    "properties": {
      "origin": { "type": "string" },
      "destination": { "type": "string" }
    },
    "required": ["origin", "destination"]
  }
}
```

---

## Implementation Notes

| Task                | Description                                  |
|---------------------|----------------------------------------------|
| Detect tool blocks  | Parse `<antml:function_calls>`               |
| Validate params     | Match against schema before backend call     |
| Route to service    | Trigger API or internal logic                 |
| Append results      | Store JSON back in AI conversation history   |
