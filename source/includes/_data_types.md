# Data Types

There are some data types and attributes which Switcheo Exchange uses that have specific meanings and formats.

Here is a list of important data types:

Data Type                           |
------------------------------------|
[Addresses](#addresses)             |
[Amounts](#amounts)                 |
[Contract Hashes](#contract-hashes) |
[Private Keys](#private-keys)       |
[Signature](#signatures)            |
[Timestamp](#timestamp)             |

## Addresses
An `address` in Switcheo is always represented by a hex string, and is prefixed by a `0x` only if it is
an Ethereum address. Addresses should always be in a **lowercase** hex string representation.

### NEO Addresses

> Getting the user's address

```js
const { wallet } = require('@cityofzion/neon-js')
wallet.getScriptHashFromPublicKey('031c37f6cce9627dc635d026deddd1200013c1b78dac767cdb507339a831183fd9')
// Result: 87cf67daa0c1e9b6caa1443cf5555b09cb3f8e5f

// you can also retrieve the script hash from a wallet address directly with:
wallet.getScriptHashFromAddress('AQV8FNNi2o7EtMNn4etWBYx1cqBREAifgE')
// Result: 87cf67daa0c1e9b6caa1443cf5555b09cb3f8e5f
```

For NEO, an `address` refers to the [RIPEMD160](https://en.wikipedia.org/wiki/RIPEMD) hash of the the user's ECDSA public key.

An example implemention to derive an `address` from a user's public key can be found in the
[neon-js javascript library](https://github.com/CityOfZion/neon-js/blob/5d61c31a5d6e5e2e29095e08c70d23449810b509/src/wallet/core.js#L92).

Please note the difference in terminology between Switcheo and neon-js. Switcheo's `address` is equivalent to neon-js's `scriptHash`.

### Ethereum Addresses

For Ethereum, an `addresses` refers to the [Keccak](https://en.wikipedia.org/wiki/SHA-3) hash of the right-most 160-bits of the user's ECDSA public key, as specified in the Ethereum [Yellow Paper](http://gavwood.com/paper.pdf).

Ethereum addresses are sometimes displayed with uppercase letters for verification purposes. When submitting addresses through the Switcheo API however, the address should always be **fully lowercased**.

## Amounts

When specifying amounts as request parameters, the amounts should first be converted by following these steps:

1. Multiply the original amount by 10 to the power of the precision allowed by the given token.
2. Drop any decimals, and convert the resulting integer to a string

For example, if the token's precision is `8`, and the original amount is `9.12345678`:

1. Following the above steps: `9.12345678 * (10 ^ 8)` = `912345678.00`
2. Drop any decimals to get: `912345678`
3. Convert the result to a string to get `"912345678"`

The list of supported assets and their corresponding precisions can be found in the [Tokens](#get-tokens) section.


## Private Keys

Please check the documentation of the respective blockchains for signing with private keys.

For NEO, there is both a WIF and a private key.

Example WIF:
<code style=" hyphens: none;">
L479CJz4ZyxHiumy2viAjBFk8nixKeMY45DCQa5p4n4AT6M6WsWq
</code>

Example private key:
<code style=" hyphens: none;">
cd7b887c29a110e0ce53e81d6dd02805fc7b912718ff8b6659d8da42887342bd
</code>

The private key should be used for signing, not the WIF.


## Contract Hashes

Use [Get Contracts](#get-contracts) to get the correct blockchain smart contract you are referring to. There may be more than one version for a given blockchain, due to upgrades, as smart contracts are immutable.  The correct contract hash must be used depending on whether you are using the TestNet or MainNet endpoints.

## Signatures

Take note that not all parameters should be included to generate the signature, excluded parameters are specified in their respective endpoint's documentation. See [signing request parameters](#signing-request-parameters) for more information on how to sign each request based on the required blcokchain.

## Timestamps

If a timestamp parameter is required, then it should first be fetched from the server using [Get Timestamp](#get-timstamp). If the timestamp is not within an acceptable range of the exchange's timestamp then an invalid signature error will be returned.
