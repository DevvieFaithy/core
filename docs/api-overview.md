# Sorokit Core API Reference

Sorokit Core is a framework-independent TypeScript SDK for Stellar wallets,
accounts, transactions, and Soroban contracts.

Start with {@link createSorokitClient} to create a client. Browse its
{@link SorokitClient} interface for wallet, account, transaction, Soroban, and
network operations. Public methods return {@link SorokitResult} values.

```ts
import { createSorokitClient } from "sorokit-core";

const result = createSorokitClient({ network: "testnet" });
if (result.status === "ok") {
  const client = result.data;
  console.log(client.network.getConfig());
}
```

## Amount Validation and Precision Boundaries

Sorokit Core enforces Stellar's exact 7-decimal fixed-point arithmetic rules across all amount operations:

- **Minimum positive amount**: `0.0000001` XLM (1 stroop).
- **Maximum amount**: `922337203685.4775807` XLM (`2^63 - 1` stroops).
- **Scale limit**: At most 7 decimal places. Inputs exceeding 7 decimal places are rejected with `SorokitErrorCode.VALIDATION`.
- **Formats**: Plain decimal strings without exponential notation (e.g. `"10"`, `"0.5"`, `"100.25"`). Zero and negative amounts are rejected for payments.
- **Invariants**: Round-trip conversions between stroops and XLM decimal strings are verified via property-based testing (`src/tests/amounts.property.test.ts`).

```ts
import { validateAmount } from "sorokit-core/shared";

const amountResult = validateAmount("10.50");
if (amountResult.status === "ok") {
  console.log("Validated amount:", amountResult.data);
}
```

Use the search box or navigation to find functions, interfaces, and examples.

- [Getting started and SDK usage](https://github.com/TochukwuJustice/core#readme)
- [Task-oriented workflows](https://github.com/TochukwuJustice/core/blob/main/docs/workflows.md)
- [Architecture guide](https://github.com/TochukwuJustice/core/blob/main/docs/architecture.md)

To regenerate this reference locally, run `npm ci --legacy-peer-deps` and
`npm run docs`, then open `docs/api/index.html`. Generated files are not committed.
