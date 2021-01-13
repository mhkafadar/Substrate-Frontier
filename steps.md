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

https://substrate.dev/frontier-workshop/#/main-content/evm-genesis

Those steps followed.

It compiled successfully.

### 3- Install the Ethereum Pallet

https://substrate.dev/frontier-workshop/#/main-content/pallet-ethereum

Those steps followed. 

It does not compile. It gives an error:

```
error[E0063]: missing field `pallet_ethereum` in initializer of `GenesisConfig`
   --> node/src/chain_spec.rs:138:2
    |
138 |     GenesisConfig {
    |     ^^^^^^^^^^^^^ missing `pallet_ethereum`
```

Extra step: pallet_ethereum added to cargo.toml

It compiles now.

### 4- The Ethereum genesis configuration

https://substrate.dev/frontier-workshop/#/main-content/ethereum-genesis

Those steps followed.

It compiled successfully.

### 5- Wrapping transactions

https://substrate.dev/frontier-workshop/#/main-content/wrapping-transactions

Those steps followed.

It compiled successfully.

### 6- Installing the Runtime API

https://substrate.dev/frontier-workshop/#/main-content/runtime-api

Those steps followed.

It does not compile:

```
 error[E0405]: cannot find trait `EthereumRuntimeRPCApi` in crate `frontier_rpc_primitives`
     --> /Users/zaara/Code/Workshop/workshop/substrate3/substrate-node-template/runtime/src/lib.rs:147:31
      |
  147 | impl frontier_rpc_primitives::EthereumRuntimeRPCApi<Block> for Runtime {
      |                               ^^^^^^^^^^^^^^^^^^^^^ not found in `frontier_rpc_primitives`
      |
  help: consider importing this trait
      |
  9   | use frontier_rpc_primitives::runtime_decl_for_EthereumRuntimeRPCApi::EthereumRuntimeRPCApi;
```
It compiles now after following change:

```
frontier_rpc_primitives::runtime_decl_for_EthereumRuntimeRPCApi::EthereumRuntimeRPCApi<Block>
```

### 7- Installing the FrontierBlockImport

https://substrate.dev/frontier-workshop/#/main-content/block-import?id=install-the-frontier-block-import

Those steps followed.

It compiled successfully.

### 8- Installing the RPC endpoint

https://substrate.dev/frontier-workshop/#/main-content/rpc

Those steps followed.

It does not compile:


```
error[E0277]: the trait bound `RuntimeApiImpl<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>, sc_service::client::client::Client<sc_client_db::Backend<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>, sc_service::client::call_executor::LocalCallExecutor<sc_client_db::Backend<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>, NativeExecutor<service::Executor>>, sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>, node_template_runtime::RuntimeApi>>: frontier_rpc_primitives::EthereumRuntimeRPCApi<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>` is not satisfied
   --> node/src/service.rs:152:13
    |
152 |             crate::rpc::create_full(deps)
    |             ^^^^^^^^^^^^^^^^^^^^^^^ the trait `frontier_rpc_primitives::EthereumRuntimeRPCApi<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>` is not implemented for `RuntimeApiImpl<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>, sc_service::client::client::Client<sc_client_db::Backend<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>, sc_service::client::call_executor::LocalCallExecutor<sc_client_db::Backend<sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>>, NativeExecutor<service::Executor>>, sp_runtime::generic::Block<sp_runtime::generic::Header<u32, BlakeTwo256>, OpaqueExtrinsic>, node_template_runtime::RuntimeApi>>`
    |
   ::: node/src/rpc.rs:36:8
    |
36  | pub fn create_full<C, P, BE>(
    |        ----------- required by a bound in this
...
49  |     C::Api: frontier_rpc_primitives::EthereumRuntimeRPCApi<Block>,
    |             ----------------------------------------------------- required by this bound in `create_full`
```

I tried this line:

`C::Api: frontier_rpc_primitives::runtime_decl_for_EthereumRuntimeRPCApi::EthereumRuntimeRPCApi<Block>`

instead of this line:

`C::Api: frontier_rpc_primitives::EthereumRuntimeRPCApi<Block>,`

at line 49 of node/src/rpc.rs file.

But it did not help.
