---
name: blaxel
description: Use when deploying AI agents, creating sandboxes for code execution, hosting MCP servers, running batch jobs, or managing serverless AI infrastructure. Reach for this skill when building agentic systems that need compute environments, tool servers, or orchestrated workflows.

---

# Blaxel Skill Reference

## Product summary

Blaxel is a perpetual sandbox platform built for AI agents. It provides serverless compute infrastructure including instant-launching sandboxes (resume from standby in <25ms), agent hosting, batch jobs, and MCP server deployment. Agents use the Blaxel SDK (`@blaxel/core` for TypeScript, `blaxel` for Python) to programmatically create and manage resources. The CLI (`bl` command) handles authentication, deployment, and resource management. Key files: `blaxel.toml` (deployment config), `.env` (secrets), `package.json` or `pyproject.toml` (dependencies). Primary docs: https://docs.blaxel.ai

## When to use

Deploy this skill when:
- Building AI agents that need isolated compute environments (sandboxes)
- Hosting agents as serverless HTTP endpoints with auto-scaling
- Creating MCP servers to expose tools/capabilities to agents
- Running batch processing tasks in parallel (jobs)
- Managing multi-region deployments or complex agent architectures
- Integrating with LLM APIs, databases, or external services
- Needing observability, tracing, and telemetry for agent runs
- Developing locally and deploying to production without infrastructure management

## Quick reference

### Essential CLI commands

| Command | Purpose |
|---------|---------|
| `bl login` | Authenticate to workspace |
| `bl new agent\|job\|mcp\|sandbox` | Initialize new resource |
| `bl serve [--hotreload]` | Run locally on port 1338 |
| `bl deploy [-d directory]` | Build and deploy to production |
| `bl get sandboxes\|agents\|functions\|jobs` | List resources |
| `bl logs <resource-name>` | Stream logs |
| `bl connect sandbox <name>` | Open interactive terminal |
| `bl apply -f manifest.yaml` | Deploy from YAML manifest |
| `bl chat <agent-name>` | Test agent interactively |
| `bl run job <name> --data '{...}'` | Execute batch job |

### Resource types and timeouts

| Resource | Sync timeout | Async timeout | Use case |
|----------|-------------|---------------|----------|
| Sandbox | N/A (persistent) | N/A | Agent compute runtime, code execution |
| Agent (sync) | 100 seconds | N/A | Fast HTTP API responses |
| Agent (async) | N/A | 10 minutes | Longer-running agent tasks |
| Job | N/A | 24 hours | Batch processing, parallel tasks |
| MCP Server | N/A | 10 minutes | Tool exposure via HTTP |

### blaxel.toml structure

```toml
type = "agent"  # agent, function, job, sandbox, volume-template
name = "my-resource"
public = false  # only for agents

[build]
slim = true  # automatic image slimming

[env]
MY_VAR = "value"  # non-secret config

[runtime]
memory = 4096  # MB
timeout = 900  # seconds (jobs/agents)
maxConcurrentTasks = 10  # jobs only

[[triggers]]
id = "my-trigger"
type = "http"  # http, http-async, schedule
[triggers.configuration]
path = "/webhook"
authenticationType = "public"  # functions only
```

### SDK authentication priority

1. Running on Blaxel (automatic)
2. `.env` file: `BL_WORKSPACE` and `BL_API_KEY`
3. Environment variables on machine
4. Local config from `bl login`

## Decision guidance

### When to use each compute option

| Scenario | Choose | Reason |
|----------|--------|--------|
| Agent needs OS access, file system, processes | Sandbox | Full VM with <25ms resume, persistent state |
| Agent responds to HTTP requests in <100s | Agent (sync) | Optimized for fast request-response |
| Agent processes data for minutes | Agent (async) | Maintains connection up to 10 min |
| Many parallel sub-tasks, long duration | Job | Batch processing, up to 24h per task |
| Expose reusable tools to multiple agents | MCP Server | Decoupled tool layer, <25ms boot |

### Sandbox vs Volume for persistence

| Need | Use | Trade-off |
|------|-----|-----------|
| Fast resume, state retention across sessions | Sandbox suspension | Charged for snapshot storage, not guaranteed forever |
| Critical data surviving indefinitely | Volume | ~400-600ms cold boot when recreating sandbox |
| Both speed and durability | Sandbox + Volume | Optimal: suspend for speed, volume for safety |

### Deployment: CLI vs GitHub vs Console

| Method | When | Benefit |
|--------|------|---------|
| `bl deploy` | Iterating locally | Full control, immediate feedback |
| GitHub integration | Production workflow | Auto-deploy on push to main |
| Console UI | One-off deployments | No CLI needed, visual setup |

## Workflow

### Deploy an agent

1. **Initialize**: Run `bl new agent` and choose framework (LangChain, CrewAI, Claude SDK, etc.)
2. **Develop**: Edit `src/agent.ts` or `src/agent.py` with your agent logic; use Blaxel SDK to access sandboxes, MCP servers, models
3. **Configure**: Create/edit `blaxel.toml` with resource name, memory, environment variables, triggers
4. **Test locally**: Run `bl serve --hotreload` and test at `http://localhost:1338` with POST `{"inputs": "..."}`
5. **Deploy**: Run `bl deploy` (or `bl deploy -d path/to/agent` for subdirectory)
6. **Query**: Use `bl chat agent-name` or POST to the generated endpoint
7. **Monitor**: Check logs with `bl logs agent-name`

### Create and use a sandbox

1. **Create**: Use SDK in your agent code to create a sandbox.
2. **Execute**: Run processes, read/write files via SDK or MCP tools
3. **Persist**: Attach volumes for long-term data; rely on suspension for session state
4. **Clean up**: Set TTL expiration or manually delete when done

### Create and use a sandbox preview

1. **Create sandbox with exposed port**: Use SDK in your agent code to create a preview. Ensure the target port is declared at sandbox creation time — ports cannot be added post-creation
2. **Start a service**: Run your web server inside the sandbox, binding to `0.0.0.0` (not `localhost`) on the chosen port
3. **Create a preview URL**: Generate a public or private URL for the running service
4. **Optional — private preview**: Set `public: false`; callers must pass the preview token as a request parameter or header
5. **Optional — custom prefix**: Set `prefixUrl: "my-app"` / `prefix_url: "my-app"` for a stable, recognizable URL
6. **Hot reload caveat**: If using a bundler with WebSocket hot reload (e.g. Vite/Webpack), conditionally set `webSocketURL.port` based on environment to prevent the client from trying to connect to the local dev port instead of the preview URL

### Deploy an MCP server

1. **Initialize**: Run `bl new mcp`
2. **Implement**: Define tools in `src/server.ts` or `src/server.py` using MCP SDK
3. **Configure**: Set `type = "function"` in `blaxel.toml`, specify transport as `http-stream`
4. **Test**: Run `bl serve` or `pnpm inspect` to test locally
5. **Deploy**: Run `bl deploy`
6. **Invoke**: Call endpoint `https://run.blaxel.ai/{workspace}/functions/{name}/mcp` from agents

### Run a batch job

1. **Initialize**: Run `bl new job`
2. **Implement**: Define task logic in `src/index.ts` or `src/index.py`; accept task parameters
3. **Configure**: Set `type = "job"` in `blaxel.toml`, define `maxConcurrentTasks`, `timeout`, optional cron trigger
4. **Test**: Run `bl serve` and POST `{"tasks": [{"param": "value"}]}`
5. **Deploy**: Run `bl deploy`
6. **Execute**: Use `bl run job name --data '{...}'` or HTTP trigger

## Important notes

- **Preview URLs**: Sandbox preview URLs must be created at the time of sandbox creation. They cannot be created after the sandbox is created.
- **Server binding**: Always bind to `HOST` and `PORT` environment variables, not hardcoded `localhost` or `127.0.0.1`. Blaxel injects these; preview URLs will fail with 502 if you don't.
- **Sandbox ports**: Reserved ports (80, 443, 8080) cannot be exposed. Expose custom ports at sandbox creation time; cannot add ports post-creation.
- **Timeout confusion**: Sync agents timeout at 100s (connection open), async at 10min (connection closed). Jobs can run 24h but individual tasks timeout at configured value.
- **Volume size**: Always provision 20-30% extra space beyond template data size or volume creation fails.
- **Secrets in git**: Add `.env` to `.gitignore` before committing. Use `--env-file` flag to specify production secrets separately.
- **Sandbox lifecycle**: Don't delete sandboxes unnecessarily. Suspension is free (except snapshot storage); deletion loses instant-resume benefit. Use TTL for automatic cleanup.
- **Authentication scope**: SDK auto-authenticates on Blaxel. Locally, run `bl login` once; it persists. For remote servers, use `BL_WORKSPACE` and `BL_API_KEY` env vars.
- **Revisions**: Each `bl deploy` creates a new revision. Only last 5 revisions kept. Revisions are immutable; use git for version control.
- **Hot reload issues**: Webpack hot reload may fail if client tries to connect to local dev port instead of sandbox preview URL. Conditionally set `webSocketURL.port` based on environment.

## Verification checklist

Before submitting work:

- [ ] Server binds to `process.env.HOST` and `process.env.PORT` (not hardcoded localhost)
- [ ] `.env` file added to `.gitignore` and contains only secrets, not config
- [ ] `blaxel.toml` specifies correct `type` (agent, function, job, sandbox)
- [ ] All required environment variables documented in `blaxel.toml` `[env]` section
- [ ] Tested locally with `bl serve` and verified endpoint works
- [ ] Sandbox ports (if used) exposed at creation time, not post-creation
- [ ] Volume size provisioned with 20-30% buffer if using templates
- [ ] Timeout values appropriate for workload (100s sync, 10min async, 24h jobs)
- [ ] Logs checked with `bl logs` for errors or warnings
- [ ] Deployment region specified or confirmed default is acceptable
- [ ] Public/private access set correctly (agents default to private)

## Resources

- Deployment configuration reference: https://docs.blaxel.ai/deployment-reference
- CLI command reference: https://docs.blaxel.ai/cli-reference/introduction

Read individual SDK files for detailed explanations and code examples:

- ./references/sdk-python.md
- ./references/sdk-typescript.md

Each SDK README contains:
- An overview of the SDK
- Requirements
- Code examples for working with sandboxes, volumes, agents, batch jobs, MCP
- Additional useful information

For additional documentation, see: https://docs.blaxel.ai/llms.txt
