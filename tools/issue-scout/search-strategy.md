# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

Fifteen queries: five project families from the profile, each run once
per label (`good first issue`, `help wanted`, `bug`). `gh` joins repeated
`--label` flags with AND and has no OR form, and GitHub rejects 16 or more
owners in one query, so families stay separate. Each query lists owners
(orgs) rather than keywords: keyword searches like "llm" or "agent" mostly
returned tiny personal repos, and results carry no star count to screen
them out later. The `bug` queries are there because scan 1 showed fresh
newcomer-labelled issues get raced within hours; a confirmed bug without a
newcomer label draws far fewer drive-by PRs, and a bug with a clear
reproduction is what the profile ranks highest. `--sort updated` makes the
100 results the most recently active ones; recency itself is filtered
below, not in the query, so no date goes stale. `--language python` traces
to "I work in Python, so I want Python repos" and is in every query. The
trailing `-- -linked:pr` drops issues that already have a pull request
linked in GitHub's Development sidebar (open, closed, or merged): result
fields carry no PR data, so this is the only free stage that can see it.
The `--` must come last, or `gh` reads every flag after it as search text.
Run the queries one at a time with a few seconds between them: fifteen
searches fired back to back can hit GitHub's secondary rate limit (HTTP
403). The named projects are examples, not limits; the org lists are where
the scout looks first.

Eval and observability, from: "LLM eval and observability frameworks (inspect_ai, TruLens, DeepEval, Ragas, Phoenix/OpenInference, MLflow, Langfuse, Opik, lighteval, Guardrails)"
gh search issues --owner explodinggradients,confident-ai,UKGovernmentBEIS,mlflow,Arize-ai,comet-ml,langfuse,truera,guardrails-ai,huggingface --label "good first issue" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Eval and observability, same sentence, `help wanted`:
gh search issues --owner explodinggradients,confident-ai,UKGovernmentBEIS,mlflow,Arize-ai,comet-ml,langfuse,truera,guardrails-ai,huggingface --label "help wanted" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Eval and observability, same sentence, `bug`:
gh search issues --owner explodinggradients,confident-ai,UKGovernmentBEIS,mlflow,Arize-ai,comet-ml,langfuse,truera,guardrails-ai,huggingface --label "bug" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Agent and LLM-systems, from: "agent and LLM-application frameworks (LangChain/LangGraph, LlamaIndex, DSPy, CrewAI, Pydantic AI, Haystack, LiteLLM, Instructor, OpenAI Agents SDK, Agno, Letta, Mem0)"
gh search issues --owner langchain-ai,run-llama,stanfordnlp,pydantic,crewAIInc,deepset-ai,BerriAI,567-labs,openai,agno-agi,letta-ai,mem0ai --label "good first issue" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Agent and LLM-systems, same sentence, `help wanted`:
gh search issues --owner langchain-ai,run-llama,stanfordnlp,pydantic,crewAIInc,deepset-ai,BerriAI,567-labs,openai,agno-agi,letta-ai,mem0ai --label "help wanted" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Agent and LLM-systems, same sentence, `bug`:
gh search issues --owner langchain-ai,run-llama,stanfordnlp,pydantic,crewAIInc,deepset-ai,BerriAI,567-labs,openai,agno-agi,letta-ai,mem0ai --label "bug" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Wider ML ecosystem, from: "the wider ML ecosystem of libraries, training and serving infrastructure, and MLOps tools (scikit-learn, PyTorch, Lightning, Ray, vLLM, Weights & Biases, Optuna, BentoML, ZenML, Kedro, Feast, DVC, sktime, Docling, Unstructured)"
gh search issues --owner scikit-learn,pytorch,Lightning-AI,ray-project,vllm-project,wandb,optuna,bentoml,zenml-io,kedro-org,feast-dev,iterative,sktime,docling-project,Unstructured-IO --label "good first issue" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Wider ML ecosystem, same sentence, `help wanted`:
gh search issues --owner scikit-learn,pytorch,Lightning-AI,ray-project,vllm-project,wandb,optuna,bentoml,zenml-io,kedro-org,feast-dev,iterative,sktime,docling-project,Unstructured-IO --label "help wanted" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Wider ML ecosystem, same sentence, `bug`:
gh search issues --owner scikit-learn,pytorch,Lightning-AI,ray-project,vllm-project,wandb,optuna,bentoml,zenml-io,kedro-org,feast-dev,iterative,sktime,docling-project,Unstructured-IO --label "bug" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Data and Python infrastructure, from: "the data and Python infrastructure ML runs on, which I use at work (Airflow and the rest of Apache's Python projects, Polars, DuckDB, pandas, Dask, xarray, Jupyter, marimo, Dagster, Prefect, Great Expectations, Ibis, FastAPI/Starlette, SQLAlchemy)"
gh search issues --owner apache,pola-rs,duckdb,pandas-dev,dask,pydata,jupyter,marimo-team,dagster-io,PrefectHQ,great-expectations,ibis-project,fastapi,encode,sqlalchemy --label "good first issue" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Data and Python infrastructure, same sentence, `help wanted`:
gh search issues --owner apache,pola-rs,duckdb,pandas-dev,dask,pydata,jupyter,marimo-team,dagster-io,PrefectHQ,great-expectations,ibis-project,fastapi,encode,sqlalchemy --label "help wanted" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Data and Python infrastructure, same sentence, `bug`:
gh search issues --owner apache,pola-rs,duckdb,pandas-dev,dask,pydata,jupyter,marimo-team,dagster-io,PrefectHQ,great-expectations,ibis-project,fastapi,encode,sqlalchemy --label "bug" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Mid-size LLM tooling, from: "mid-size LLM tooling, where a newcomer is not one of hundreds racing for each issue (OpenLLMetry, Evidently, Giskard, Deepchecks, Outlines, Guidance, Chroma, LanceDB, Qdrant, Weaviate, AgentOps, OpenLIT, the MCP Python SDK, CAMEL, browser-use)"
gh search issues --owner traceloop,evidentlyai,Giskard-AI,deepchecks,dottxt-ai,guidance-ai,chroma-core,lancedb,qdrant,weaviate,AgentOps-AI,openlit,modelcontextprotocol,camel-ai,browser-use --label "good first issue" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Mid-size LLM tooling, same sentence, `help wanted`:
gh search issues --owner traceloop,evidentlyai,Giskard-AI,deepchecks,dottxt-ai,guidance-ai,chroma-core,lancedb,qdrant,weaviate,AgentOps-AI,openlit,modelcontextprotocol,camel-ai,browser-use --label "help wanted" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

Mid-size LLM tooling, same sentence, `bug`:
gh search issues --owner traceloop,evidentlyai,Giskard-AI,deepchecks,dottxt-ai,guidance-ai,chroma-core,lancedb,qdrant,weaviate,AgentOps-AI,openlit,modelcontextprotocol,camel-ai,browser-use --label "bug" --language python --state open --sort updated --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- -linked:pr

## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

Apply in this order to the combined, de-duplicated results. "Scan date"
is the day the scan runs.

1. Drop any result with one or more assignees.
2. Drop any result whose `updatedAt` is more than 60 days before the scan
   date.
3. Drop any result whose title contains, as a whole word and ignoring
   case, `Windows`, `JavaScript`, `TypeScript`, `JS`, `Node.js`, `npm`,
   `CUDA`, `GPU`, `NPU`, `ROCm`, `Ascend`, `TPU`, `XPU`, `NCCL`, `AMD`, or
   `NVIDIA`. From "I develop on macOS without a GPU, so I want to avoid
   issues that only reproduce on Windows or need special hardware ..., and
   I'd rather avoid JavaScript/TypeScript or frontend work." (`--language`
   matches the repo's main language, not the issue, so a Python repo's
   JS-package or GPU-only bugs get through without this.)
4. Drop any result with more than 5 comments: a busy thread usually means
   someone is already on it or the scope is contested.
5. Order what is left by age from `createdAt`: first issues opened 30 to
   365 days before the scan date, then issues under 30 days old, then
   issues over a year old. Fresh issues are where drive-by PRs race; very
   old ones are often stale; the band between is where an unclaimed issue
   is most likely to still be open to a newcomer.
6. Within each age band: results with a label containing `bug` first, then
   `good first issue`, then the rest; then fewest comments first; ties
   broken by most recent `updatedAt` first.
7. Walking that order, take at most 2 results from any one repository;
   skip a third and take the next result instead, so one busy repo cannot
   fill the whole top N.

Repo size cannot be ranked here (results carry no star count). The
preference for mid-size repos lives in the fifth family's org list instead.

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
