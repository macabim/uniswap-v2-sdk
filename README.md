# Custom Uniswap V2 SDK

[![Unit Tests](https://github.com/gndplayground/uniswap-v2-sdk/actions/workflows/unit-tests.yml/badge.svg)](https://github.com/gndplayground/uniswap-v2-sdk/actions/workflows/unit-tests.yml)
[![Lint](https://github.com/gndplayground/uniswap-v2-sdk/actions/workflows/lint.yml/badge.svg)](https://github.com/gndplayground/uniswap-v2-sdk/actions/workflows/lint.yml)
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![npm version](https://img.shields.io/npm/v/custom-uniswap-v2-sdk/latest.svg)](https://www.npmjs.com/package/custom-uniswap-v2-sdk/v/latest)
[![npm bundle size (scoped version)](https://img.shields.io/bundlephobia/minzip/custom-uniswap-v2-sdk/latest.svg)](https://bundlephobia.com/result?p=@custom-uniswap-v2-sdk)

In-depth documentation on original SDK is available at [uniswap.org](https://uniswap.org/docs/v2/SDK/getting-started/).

Most blockchain with EVM compatibility will use Uniswap solutions to build a decentralized crypto exchange. However,
the original Uniswap SDK has a factory address contract and the init code hash (used for `getCreate2Address` etherjs method) is locked in the source code.

The result is that everyone is cloning and creates their own version. And as a result, their own SDK is outdated with
Uniswap latest fixes, upgrade and hard to check what changes with the original Uniswap SDK.

I forked and changes some code, so we can reuse Uniswap SDK without cloning and create our own.

##[Demo online](https://gndplayground.github.io/custom-uniswap-v2-sdk-example/)

## Install

```shell
npm i custom-uniswap-v2-sdk
```

or

```shell
yarn add custom-uniswap-v2-sdk
```

## Changes

- The forked SDK won't export `FACTORY_ADDRESS` and `INIT_CODE_HASH` anymore.

- Exported `computePairAddress` method will require factory address, and init code hash.

```ts
// Example interface
interface ComputePairAddressArg {
  factoryAddress: string
  initCodeHash: string
  tokenA: Token
  tokenB: Token
}

export function computePairAddress(arg: ComputePairAddressArg) {
  // Example
}
```

- Exported class `Pair` constructor will require factory address, and init code hash.

```ts
new Pair(currencyAmountA, tokenAmountB, factoryAddress, initCodeHash)
```

- Exported class `Pair` static method `getAddress` will require factory address, and init code hash.

```ts
 public static getAddress(tokenA: Token, tokenB: Token, factoryAddress: string, initCodeHash: string): string {
    //
}
```

- Exported class `Pair` constructor now support custom fees numerator and fees denominator `(v3.1.0)`

In the Uniswap SDK , `feesNumerator` and `feesDenominator` are constant `JSBI.BigInt(997)` and `JSBI.BigInt(1000)`

```ts
export const _997 = JSBI.BigInt(997)

export const _1000 = JSBI.BigInt(1000)
```

In `PancakeSwap` they change this to support different fee

```ts
export const FEES_NUMERATOR = JSBI.BigInt(9975)

export const FEES_DENOMINATOR = JSBI.BigInt(10000)
```

So in the custom SDK, The`Pair` class now can change the fees. If you do not add to the constructor, default fees will be used.

```ts
new Pair(currencyAmountA, tokenAmountB, factoryAddress, initCodeHash, feesNumerator, feesDenominator)
```
