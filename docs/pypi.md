# uni-robot-agent

A unified harness for robot-use agents, with Codex and Claude Code runtimes,
a Python SDK, shared robot tools, and a local monitoring dashboard.

[Documentation](https://uni-robot-agent.com/docs) · [Project website](https://uni-robot-agent.com/)

## Install

Use Python 3.11+ in a virtual environment:

```bash
pip install uni-robot-agent
# or: uv pip install uni-robot-agent
uni-robot-agent --version
```

For an isolated CLI, use `uv tool install uni-robot-agent`. For the SDK in a uv
project, use `uv add uni-robot-agent`.

## New in 0.2.0

- Clean mode exposes `move`, `hold`, `capture_scene` and `finish`, with either
  calibrated RGB or RGB-only observations, compact JSON and readable image paths.
- The optional observation window limits old image/calibration context while
  preserving action history and reasoning.
- Broader Hybrid support includes dexterous manipulation, mobile manipulation and
  navigation integrations; native runtime requirements vary by environment.
- ARX ONE, additional benchmark integrations, agent/provider adapters, Code as
  Policy improvements and the offline `doctor` command expand the harness.

Upgrade CLI/planner and native services together and restart the services:
scalar environment protocol 4, scalar tool schema 8 and actor HTTP protocol v3
are incompatible with older service versions. Install `uni-robot-agent==0.2.0`
and `uni-robot-agent-runtime==0.2.0` in their **separate** environments; never
install both distributions together. Existing `--tool-profile minimal` remains
an alias for `--tool-profile clean`.

## Run a task

On a Linux GPU host with Python 3.11, prepare RoboDojo once, then run a task:

```bash
uni-robot-agent setup --integration robodojo

export UNI_ROBOT_AGENT_API_KEY='YOUR_API_KEY'
export UNI_ROBOT_AGENT_PROVIDER_URL='https://provider.example/v1'
uni-robot-agent run --integration robodojo --deploy local \
  --task stack_bowls --model YOUR_MODEL_ID \
  --action-profile direct --no-sam3 --output-dir ./runs --monitor
```

Set your vision model and Responses-compatible endpoint. Open the printed
dashboard URL to follow execution, or replay results later with
`uni-robot-agent monitor --run-dir ./runs`.

## Python SDK

Connect to an already running environment service using the same credentials:

```python
from uni_robot_agent import AgentConfig, SessionConfig, run

result = run(SessionConfig(
    integration="robodojo",
    env_url="http://127.0.0.1:18760",
    no_sam3=True,
    agent=AgentConfig(
        model="YOUR_MODEL_ID",
        provider_url="https://provider.example/v1",
    ),
    output_dir="runs",
))
print(result.agent_finished, result.task_success, result.session_dir)
```

The package includes prompts, skills, deployment recipes and dashboard assets.
Simulator runtimes, assets, GPU drivers and model weights are prepared separately;
see the documentation for host requirements. macOS supports the CLI and SDK with
remote environment services and local replay. ARX ONE provides a physical-robot
integration with separate hardware, driver and calibration requirements; see its
deployment guide before operating hardware.

Released under the MIT License. Bundled upstream components and external assets
retain their respective licenses.
