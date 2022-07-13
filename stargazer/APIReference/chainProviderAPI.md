---
title: Chain Provider API
sidebar_label: Chain Provider API
sidebar_position: 2
hide_table_of_contents: false
---

import DocsCard from '@components/global/DocsCard';
import DocsCards from '@components/global/DocsCards';
import TOCInline from '@theme/TOCInline';

<head>
  <meta
    name="description"
    content="Lorem ipsum"
  />
</head>

<intro-end />

The `ChainProvider` handles access to the chain RPC methods and underlying events. It complies with the [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) standard. Besides methods from the standard it adds a couple of utility methods related to [provider activation](../Guide/providerActivation.md).

## Properties

### `version`

Contains the current wallet version.

##### Type

`String` - Version string.

##### Example

```typescript title="TypeScript"
provider.version;
// "3.5.2"
```

### `chain`

Contains the chain name of the provider.

##### Type

`"constellation"` | `"ethereum"` - Chain name.

##### Example

```typescript title="TypeScript"
provider.chain;
// "constellation"
```

### `activated`

Contains the status of [activation](../Guide/providerActivation.md).

##### Type

`Boolean` - Indicating if the provider is activated for the current [origin](https://datatracker.ietf.org/doc/html/rfc6454).

##### Example

```typescript title="TypeScript"
provider.version;
// true
```

### `async activate(title?): boolean`

Sends an [activation](../Guide/providerActivation.md) request for the user to accept. If the user has already given authorization for the current [origin](https://datatracker.ietf.org/doc/html/rfc6454) this method just configures the underlying communication channel.

##### Parameters

| Name   | Type     | Description |
| ------ | -------- | ----------- |
| title? | `String` | App name.  |

##### Return Type

`Boolean` - Indicating the result of the activation request.

##### Example

```typescript title="TypeScript"
await provider.activate();
// true
```

### `async request(request): any`

**[EIP-1193 request()](https://eips.ethereum.org/EIPS/eip-1193#request)**

Invokes the selected RPC method. Depeding on the provider chain it will invoke a [Constellation RPC method](./constellationRPCAPI.md) or an [Ethereum RPC method](./ethereumRPCAPI.md).

##### Parameters

| Name    | Type      | Description                    |
| ------- | --------- | ------------------------------ |
| request | `Request` | Request method and parameters. |

```typescript title="Request"
type Request = {
  readonly method: string;
  readonly params?: any[];
};
```

##### Return Type

`Any` - Data returned from the request, depends on the invoked RPC method.

##### Example

```typescript title="TypeScript"
await provider.request({ method: "dag_accounts", params: [] });
// ["DAG88C9WDSKH451sisyEP3hAkgCKn5DN72fuwjfX"]
```

### `on(eventName, listener): void`

**[EIP-1193 events](https://eips.ethereum.org/EIPS/eip-1193#events)**

Registers the listener function as callback of the selected RPC event.

:::caution Warning
This method will always return, even if there are errors while adding the listener, for controlling errors generated during the registration process use [async onAsync(eventName, listener)](#async-onasynceventname-listener-void).
:::

##### Parameters

| Name      | Type      | Description                                                                                                                                                 |
| --------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventName | `String`  | Event to listen. Depeding on the provider chain one of [Constellation RPC event](./constellationRPCAPI.md) or an [Ethereum RPC event](./ethereumRPCAPI.md). |
| listener  | `()=>any` | Callback function.                                                                                                                                          |

##### Return Type

`void`

##### Example

```typescript title="TypeScript"
provider.on("accountsChanged", () => {
  // Do something when accounts changed
});
```

### `removeListener(eventName, listener): void`

**[EIP-1193 events](https://eips.ethereum.org/EIPS/eip-1193#events)**

Removes the listener function as callback of the selected RPC event.

:::caution Warning
This method will always return, even if there are errors while removing the listener, for controlling errors generated during the deregistration process use [async removeListenerAsync(eventName, listener)](#async-removelistenerasynceventname-listener-void).
:::

##### Parameters

| Name      | Type      | Description                                                                                                                                                |
| --------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventName | `String`  | Event listened. Depeding on the provider chain one of [Constellation RPC event](./constellationRPCAPI.md) or an [Ethereum RPC event](./ethereumRPCAPI.md). |
| listener  | `()=>any` | Callback function.                                                                                                                                         |

##### Return Type

`void`

##### Example

```typescript title="TypeScript"
provider.removeListener("accountsChanged", listener);
```

### `async onAsync(eventName, listener): void`

Registers the listener function as callback of the selected RPC event.

##### Parameters

| Name      | Type      | Description                                                                                                                                                 |
| --------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventName | `String`  | Event to listen. Depeding on the provider chain one of [Constellation RPC event](./constellationRPCAPI.md) or an [Ethereum RPC event](./ethereumRPCAPI.md). |
| listener  | `()=>any` | Callback function.                                                                                                                                          |

##### Return Type

`void`

##### Example

```typescript title="TypeScript"
await provider.onAsync("accountsChanged", () => {
  // Do something when accounts changed
});
```

### `async removeListenerAsync(eventName, listener): void`

Removes the listener function as callback of the selected RPC event.

##### Parameters

| Name      | Type      | Description                                                                                                                                                |
| --------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventName | `String`  | Event listened. Depeding on the provider chain one of [Constellation RPC event](./constellationRPCAPI.md) or an [Ethereum RPC event](./ethereumRPCAPI.md). |
| listener  | `()=>any` | Callback function.                                                                                                                                         |

##### Return Type

`void`

##### Example

```typescript title="TypeScript"
await provider.removeListenerAsync("accountsChanged", listener);
```