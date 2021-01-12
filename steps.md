## A- Substrate node installation

```
git clone -b v2.0.0 --depth 1 https://github.com/substrate-developer-hub/substrate-node-template
cd substrate-node-template
make init
make build
```
it compiles successfuly.

## B- Frontier tutorial
### 1- Install the EVM pallet

https://substrate.dev/frontier-workshop/#/main-content/pallet-evm

Those steps followed.

Result:

```
error[E0063]: missing field `pallet_evm` in initializer of `GenesisConfig`
   --> node/src/chain_spec.rs:136:2
    |
136 |     GenesisConfig {
    |     ^^^^^^^^^^^^^ missing `pallet_evm`
```

It was an expected error so step 1 is successful.

### 2- Pallet EVM in the Genesis Config
