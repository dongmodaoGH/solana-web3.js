# @solana/keys

This package contains utilities for validating, generating, and manipulating addresses and key material. It can be used standalone, but it is also exported as part of the Solana JavaScript SDK [`@solana/web3.js@experimental`](https://github.com/solana-labs/solana-web3.js/tree/master/packages/library).

## Types

### `Base58EncodedAddress`

This type represents a string that validates as a Solana address or public key. Functions that require well-formed addresses should specify their inputs in terms of this type.

Whenever you need to validate an arbitrary string as a base58-encoded address, use the `assertIsBase58EncodedAddress()` function in this package.

## Functions

### `assertIsBase58EncodedAddress()`

Client applications primarily deal with addresses and public keys in the form of base58-encoded strings. Addresses and public keys returned from the RPC API conform to the type `Base58EncodedAddress`. You can use a value of that type wherever a base58-encoded address or key is expected.

From time to time you might acquire a string, that you expect to validate as an address, from an untrusted network API or user input. To assert that such an arbitrary string is a base58-encoded address, use the `assertIsBase58EncodedAddress` function.

```ts
import { assertIsBase58EncodedAddress } from '@solana/web3.js`;

// Imagine a function that fetches an account's balance when a user submits a form.
function handleSubmit() {
    // We know only that what the user typed conforms to the `string` type.
    const address: string = accountAddressInput.value;
    try {
        // If this type assertion function doesn't throw, then
        // Typescript will upcast `address` to `Base58EncodedAddress`.
        assertIsBase58EncodedAddress(address);
        // At this point, `address` is a `Base58EncodedAddress` that can be used with the RPC.
        const balanceInLamports = await rpc.getBalance(address);
    } catch (e) {
        // `address` turned out not to be a base58-encoded address
    }
}
```