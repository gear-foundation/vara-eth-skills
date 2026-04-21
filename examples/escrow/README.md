# Async Order Escrow Example

This example shows a Solidity contract using a Vara.eth program as an async order state engine.

The split is intentional:

- Solidity `OrderEscrowAdapter` holds ETH and performs user-facing authorization.
- Vara.eth `OrderEngine` stores order lifecycle state and validates state transitions.
- ABI callbacks connect submitted Ethereum transactions to final Vara.eth execution results.

This avoids the common confusion where a Solidity adapter sends native value into Vara.eth and the Vara.eth program sees the adapter as the caller. In this escrow pattern, funds stay in Solidity until a Vara.eth callback confirms release, refund, or cancel.

## Flow

1. Buyer calls `createOrder(seller)` on the Solidity adapter and sends ETH.
2. Adapter stores the ETH and calls `ordersCreateOrder(true, buyer, seller, amount)` on the ABI-enabled Vara.eth program.
3. Adapter stores `messageId -> localOrderId`.
4. `replyOn_ordersCreateOrder(...)` returns `varaEthOrderId` and marks the local order as created on Vara.eth.
5. Buyer can call `release(localOrderId)`.
6. Adapter calls `ordersReleaseOrder(true, varaEthOrderId)`.
7. `replyOn_ordersReleaseOrder(...)` credits the seller's claimable ETH balance.
8. Seller calls `claim()` to withdraw.

Refund uses the same shape, but seller calls `refund(localOrderId)` and the confirmed callback credits ETH back to the buyer.

The Solidity adapter keeps both ids:

- `localOrderId` is the id visible to Solidity users.
- `varaEthOrderId` is the id returned by the Vara.eth program after the async create callback.

The `messageId` is only the correlation id for one async message. It is emitted in events and used internally to match callbacks with pending operations.

## Build

```bash
cargo build --release
```

Expected artifacts:

```text
target/wasm32-gear/release/order_escrow.opt.wasm
target/wasm32-gear/release/order_escrow.idl
```

Generate the Solidity ABI interface from the IDL before wiring the adapter to a deployed ABI contract:

```bash
cargo sails sol --idl-path target/wasm32-gear/release/order_escrow.idl
```

Check the generated function and callback names before using `contracts/OrderEscrowAdapter.sol` unchanged. The adapter intentionally names the expected generated ABI surface, but generated names are the source of truth.
