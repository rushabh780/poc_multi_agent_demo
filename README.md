# Minimal POC Spec: LangChain Multi-Agent Demo (Python, Local-Only)



## 1. Objective
Build a very small multi-agent LangChain project in Python having below capabilities:
- Agent orchestration
- Tool usage by agents
- Clear separation of specialist responsibilities

This POC must run fully on a laptop without live databases, vector databases, or external APIs.

## 2. Friendly Demo Theme
Use a neutral and friendly context: `Cafe Assistant Demo`.

The assistant helps with:
- Cafe order follow-up
- Cafe handbook lookup
- Coffee machine troubleshooting

## 3. Scope and Constraints
- Language: Python
- Framework: LangChain
- Execution mode: CLI only (not FastAPI)
- Data source: local JSON/MD files only
- No MongoDB, Azure Search, external ticket systems, or live enterprise endpoints
- Keep implementation small enough to finish in 2-4 hours

## 4. Agent Roles (Generic Names)
- `orchestrator_agent`: decides which specialist to call
- `orders_agent`: works with order records
- `handbook_agent`: searches handbook text
- `equipment_agent`: handles troubleshooting rules

## 5. Required Local Tools
Implement as LangChain tools (simple Python functions):
- `get_order_details(order_id: str) -> str`
- `search_handbook(query: str) -> str`
- `find_equipment_fix(error_code: str, symptom: str) -> str`
- `get_current_datetime() -> str`

Optional:
- `create_markdown_report(summary: str, filename: str) -> str`

## 6. Fixed Questions for Demo Validation
Only validate these questions first:
1. `Summarize order CF-1002 and suggest the next action.`
2. `How should staff clean the espresso machine at closing time?`
3. `Error E-17 with symptom "low steam pressure". What should I check first?`
4. `Use order CF-1003 and handbook guidance to provide a short action plan.`
5. `What is the return policy for damaged takeaway mugs?`
6. `What opening checks should be done before serving customers?`

## 7. Folder Structure

```text
src/poc_multi_agent_demo/
  main.py
  router.py
  agents/
    orders_agent.py
    handbook_agent.py
    equipment_agent.py
  tools/
    order_tools.py
    handbook_tools.py
    equipment_tools.py
    utility_tools.py
  data/
    cafe_orders.json
    cafe_handbook.md
    equipment_rules.json
  tests/
    test_smoke.py
```

## 8. Data Files (Already Added)
Use these files directly in tools:
- `src/poc_multi_agent_demo/data/cafe_orders.json`
- `src/poc_multi_agent_demo/data/cafe_handbook.md`
- `src/poc_multi_agent_demo/data/equipment_rules.json`

Developers should read only these local files via tool functions.

## 9. Implementation Rules
- Keep each specialist agent domain-specific
- Router should delegate, not parse files directly
- Tool logic should be deterministic and small
- Prompt instructions should be short (3-6 lines)
- Output should be concise and structured

## 10. LLM Mode
Use one of these:
- Preferred default: `FakeListChatModel` (offline, no API key)
- Optional: `ChatOpenAI` (if a dev chooses to test with API key)

Default path must remain offline-friendly.

## 11. Definition of Done
- One command can answer all 6 fixed demo questions from local files
- Console output shows which specialist/tool was used
- No external DB/API calls occur
- At least one smoke test passes
- New joiner can run setup to success in under 15 minutes

## 12. New Joiner Runbook
1. Create venv and install dependencies: `uv sync`
2. Run demo query:
   `uv run python -m src.poc_multi_agent_demo.main --query "<question>"`
3. Run smoke tests:
   `uv run pytest src/poc_multi_agent_demo/tests -q`

## 13. Out of Scope
- Authentication/authorization
- FastAPI endpoints
- Session persistence
- Production observability integrations
- PDF generation/styling

---

This POC is intentionally minimal and focused on agentic concepts, not production features.
