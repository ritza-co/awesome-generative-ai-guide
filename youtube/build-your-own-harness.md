# Build your own company harness

**Release planned:** October 29, 2026.
**Code:** [NS Work notebook and application](code/ns-work-harness/README.md) · [Download the source ZIP](code/ns-work-harness.zip)

Build NS Work, a harness around an AI model for Northstar's sales, engineering, and finance teams. The example starts with Deep Agents and adds Context, Actions, Boundaries, and Persistence through one cumulative notebook.

## Start with the code

1. Download the ZIP or clone this repository.
2. Open `youtube/code/ns-work-harness/` and follow its [setup instructions](code/ns-work-harness/README.md#set-up).
3. Open [00_NS_Work.ipynb](code/ns-work-harness/00_NS_Work.ipynb) in VS Code or Jupyter and select your Python 3.12 environment.
4. Run each chapter's configuration cells, then use its interface link to submit a request.

The source includes the recorded notebook, the shared harness configuration, all 4 company tools, company instructions, shared skills, a local web interface, and an offline verification script. The ZIP contains the same source as the code folder.

## The 4 responsibilities

| Responsibility | What you configure | Example |
|---|---|---|
| Context | Company guidance and reusable skills | Prepare proposals using Northstar's conventions |
| Actions | Company tools, planning, and a specialist | Look up Acme and ask a reviewer to check a commitment |
| Boundaries | Role permissions, approval, protected files, and call limits | Finance can read revenue; Sales needs approval before recording a proposal send |
| Persistence | Working files, checkpoints, and context management | Save a proposal and return to its task later |

This is a local teaching application with synthetic company records. Proposal sends are local outbox records. The [code README](code/ns-work-harness/README.md#execution-scope) explains deployment boundaries, storage, and model usage.

## Library references

- [Deep Agents customization](https://docs.langchain.com/oss/python/deepagents/customization)
- [Skills](https://docs.langchain.com/oss/python/deepagents/skills)
- [Human approval](https://docs.langchain.com/oss/python/deepagents/human-in-the-loop)
- [Filesystem backends](https://docs.langchain.com/oss/python/deepagents/backends)

The example pins the library versions used in the recording. Follow its `requirements.txt` to reproduce the configuration.
