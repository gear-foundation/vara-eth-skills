# AGENTS.md

## Purpose

This repository contains shared AI playbooks and agent skills for Vara.eth work.

## Rules

- Keep canonical task guidance in `playbooks/`.
- Keep reusable factual material in `references/`.
- Keep self-contained agent-facing task instructions in `skills/`.
- Do not make the repository depend on one agent runtime or one local installation path.
- Update shared playbooks first when behavior changes, then update skills that depend on them.
- Keep examples implementation-backed and avoid speculative claims.
- When source priority changes, keep all playbooks and skills aligned.
- Keep Rust and Sails contract-writing guidance consistent across the contract-writer and app-builder materials.

## Vara.eth Source Of Truth

Primary implementation repository:

```text
https://github.com/gear-tech/gear.git
```

Use the `ethexe/` subtree as the primary implementation source for Vara.eth behavior.

Canonical implementation areas:

- Contracts: `ethexe/contracts/src`
- ABIs/artifacts: `ethexe/ethereum/abi`
- CLI: `ethexe/cli/src`
- RPC server: `ethexe/rpc/src`
- Rust SDK wrappers: `ethexe/sdk/src`
- Runtime and processing: `ethexe/runtime`, `ethexe/processor`, `ethexe/service`

## Validation

Prefer task-specific validation:

- contract changes: `cargo build --release` and `cargo test --release`
- TypeScript examples: `npm run check` from the example folder
- deploy/interact flows: verify against the current `ethexe` binary and live output when a network is involved
- documentation changes: verify claims against the implementation source above
