# rlar _(rlar-flow)_

RLAR as an hmz flow: an actor works in one session until a fresh reviewer, reading the repository each round, says the task is done.

An actor works on the task in one session. After each of its turns a fresh reviewer is told the
task and what the actor said, reads the repository, and either says the task is done or writes
the review that becomes the actor's next prompt, word for word. It is the `rlar` that
[FlowBench](https://humanfia.ai/flowbench/) scores.

## Table of Contents

- [Install](#install)
- [Usage](#usage)
  - [What ends it](#what-ends-it)
  - [Picking it up](#picking-it-up)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

## Install

You need [hmz](https://github.com/humanfia/humanize). In hmz, open `/flow`, go to
**Flowverses → official → rlar** and **Install** it.

To run a release without installing it, name it by its git ref:

```sh
hmz exec -f git+https://github.com/humanfia/rlar-flow@v0.1.0#rlar ...
```

## Usage

```text
❯ $rlar the task
```

```sh
hmz exec -f rlar -a actor=claude/claude-opus-5:high \
    -a reviewer=codex/gpt-5.6-sol:high \
    -p budget.cost=20 "the task"
```

| Role | Needs | |
| --- | --- | --- |
| `actor` | nothing | Does the work, in one session. |
| `reviewer` | nothing | Reads each round in a fresh session. Carries the flow's `review-notes` skill. |

The flow takes no params of its own.

### What ends it

The reviewer saying the task is completely and correctly done, or the budget being spent. A turn
that fails ends the run with that failure. The run finishes on the last review: what was built
and how it was checked.

### Picking it up

The last review is kept, so `--resume` hands a fresh actor that review to pick up from.

Another flow can call it by ref, `load("rlar")`; see
[A flow that calls a flow](https://humanfia.ai/humanize/weaver/calling-flows).

## Maintainers

[@futrime](https://github.com/futrime), as listed in [`.github/CODEOWNERS`](.github/CODEOWNERS).

## Contributing

Issues and pull requests are welcome. The flow is the `rlar/` directory; its tests are in
`tests/`. With [uv](https://docs.astral.sh/uv/):

```sh
uv sync
uv run ruff check
uv run ruff format --check
uv run pytest
```

CI runs the same on every pull request. A release is a `vX.Y.Z` tag on `main`; it reaches hmz
users once a `flows/rlar/X.Y.Z/flow.yaml` pointing at it is merged into
[humanfia/flowverse](https://github.com/humanfia/flowverse).

## License

[Apache-2.0](LICENSE) © Humanfia
