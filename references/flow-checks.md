# Vara.eth App Flow Review Checklist

Use this checklist before treating a deploy, interaction, or integration example as ready.

## Setup

- Required binaries, packages, and install commands are verified.
- The `ethexe` binary is required explicitly for CLI deploy/interact flows.
- `ethexe` installation is described as either building from `https://github.com/gear-tech/gear.git` or downloading from `https://get.gear.rs/#vara-eth`.
- Chain id, RPC endpoint, Router address, sender address, program id, code id, and ABI interface address are placeholders unless confirmed for the example.
- Private keys are never hard-coded in reusable docs or examples.
- TypeScript examples that parse IDL v2 use `SailsIdlParser` from `sails-js/parser`.
- IDL v2 examples do not use the old standalone `sails-js-parser` package.
- TypeScript examples are run from the package directory where the verified `sails-js` dependency is installed.

## Deploy

- The flow clearly states whether it uses Router, Mirror, direct Sails payloads, or a Solidity ABI interface.
- Code upload and validation happen before program creation.
- CLI examples place `--ethereum-rpc`, `--ethereum-router`, and `--sender` on `ethexe tx` before the subcommand.
- Sender setup uses `ethexe key keyring import --private-key ...` when CLI key import is needed.
- `upload --watch` is treated as the validation gate and returns a `code_id` before `create` or `create-with-abi`.
- ABI-enabled creation deploys the generated ABI interface first and passes its address to `createProgramWithAbiInterface(...)`.
- Salt, initializer, and address derivation claims are verified or left as placeholders.
- The example states whether it needs native value, wVARA, owned balance, or executable balance.
- wVARA uses 12 decimals.
- ABI-enabled mirrors still include executable balance top-up before init and message processing when required.

## Interact

- TypeScript examples use `createVaraEthApi(...)` and `getMirrorClient({ address, publicClient, signer })` where appropriate.
- Sails IDL examples use `SailsProgram`, `SailsIdlParser`, and `program.setProgramId(...)`. Use `program.setApi(gearApi)` only for SailsProgram's own `@gear-js/api`-backed calls; do not pass an `@vara-eth/api` object to `setApi`.
- Direct Mirror message examples pass hex Sails payloads to `mirror.sendMessage(...)`.
- Reply listener examples call `setupReplyListener()` and then `await waitForReply()`.
- Common reply codes are decoded: `0x00000000` for success auto reply and `0x00010000` for success manual reply.
- ABI-facing examples do not assume an Ethereum receipt means Vara.eth read-only state is already visible; they poll or repeat state reads where needed.
- Caller restrictions and failure cases are stated when relevant.
- Event names and parameters are checked from contracts or ABI artifacts.
- Ethereum address versus `ActorId` conversion is explicit when account ids are used in queries.

## Outputs

- Required inputs are listed.
- Exact commands or calls are shown.
- Expected success signals are described.
- Expected output fields are named.
- Known failure modes are included.
- Source files or public surfaces checked are listed.
