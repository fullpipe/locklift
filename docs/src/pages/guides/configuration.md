---
title: Configuration
outline: deep
---

# Configuration

Locklift uses a configuration file named `locklift.config.ts` by default. This file is crucial for setting up your Locklift environment and specifying how your project will interact with different networks.

## Basic Layout

Here's an expanded layout of a `locklift.config.ts` file with more detailed configuration options:

```typescript
import { LockliftConfig } from 'locklift';
import { FactorySource } from './build/factorySource';

declare global {
  const locklift: import('locklift').Locklift<FactorySource>;
}

const LOCAL_NETWORK_ENDPOINT =
  process.env.NETWORK_ENDPOINT || 'http://localhost/graphql';
const DEV_NET_NETWORK_ENDPOINT =
  process.env.DEV_NET_NETWORK_ENDPOINT ||
  'https://devnet-sandbox.evercloud.dev/graphql';

const VENOM_TESTNET_ENDPOINT =
  process.env.VENOM_TESTNET_ENDPOINT ||
  'https://jrpc-devnet.venom.foundation/';
const VENOM_TESTNET_TRACE_ENDPOINT =
  process.env.VENOM_TESTNET_TRACE_ENDPOINT ||
  'https://gql-devnet.venom.network/graphql';

// Create your own link on https://dashboard.evercloud.dev/
const MAIN_NET_NETWORK_ENDPOINT =
  process.env.MAIN_NET_NETWORK_ENDPOINT ||
  'https://mainnet.evercloud.dev/XXX/graphql';

const config: LockliftConfig = {
  compiler: {
    // Specify path to your TON-Solidity-Compiler
    // path: "/mnt/o/projects/broxus/TON-Solidity-Compiler/build/solc/solc",

    // Or specify version of compiler
    version: '0.62.0',

    // Specify config for external contracts as in example
    // externalContracts: {
    //   "node_modules/broxus-ton-tokens-contracts/build": ['TokenRoot', 'TokenWallet']
    // }
  },
  linker: {
    // Specify path to your stdlib
    // lib: "/mnt/o/projects/broxus/TON-Solidity-Compiler/lib/stdlib_sol.tvm",
    // // Specify path to your Linker
    // path: "/mnt/o/projects/broxus/TVM-linker/target/release/tvm_linker",

    // Or specify version of linker
    version: '0.15.48',
  },
  networks: {
    local: {
      // Specify connection settings for https://github.com/broxus/everscale-standalone-client/
      connection: {
        id: 1,
        group: 'localnet',
        type: 'graphql',
        data: {
          endpoints: [LOCAL_NETWORK_ENDPOINT],
          latencyDetectionInterval: 1000,
          local: true,
        },
      },
      // This giver is default local-node giverV2
      giver: {
        // Check if you need provide custom giver
        address:
          '0:ece57bcc6c530283becbbd8a3b24d3c5987cdddc3c8b7b33be6e4a6312490415',
        key: '172af540e43a524763dd53b26a066d472a97c4de37d5498170564510608250c3',
      },
      tracing: {
        endpoint: LOCAL_NETWORK_ENDPOINT,
      },
      keys: {
        // Use everdev to generate your phrase
        // !!! Never commit it in your repos !!!
        phrase:
          'action inject penalty envelope rabbit element slim tornado dinner pizza off blood',
        amount: 20,
      },
    },
    // Other networks...
  },
  mocha: {
    timeout: 2000000,
  },
};
```

### Compiler

The `compiler` field in the configuration is used to set the version of the compiler that you're using. You can also specify a path to your TON-Solidity-Compiler, or specify config for external contracts.

```typescript
compiler: {
  // Specify path to your TON-Solidity-Compiler
  // path: "/mnt/o/projects/broxus/TON-Solidity-Compiler/build/solc/solc",

  // Or specify version of compiler
  version: "0.62.0",

  // Specify config for external contracts as in example
  // externalContracts: {
  //   "node_modules/broxus-ton-tokens-contracts/build": ['TokenRoot', 'TokenWallet']
  // }
},
```

### Linker

The `linker` field is used to specify the version of the linker. You can also specify a path to your stdlib or Linker.

```typescript
linker: {
  // Specify path to your stdlib
  // lib: "/mnt/o/projects/broxus/TON-Solidity-Compiler/lib/stdlib_sol.tvm",
  // // Specify path to your Linker
  // path: "/mnt/o/projects/broxus/TVM-linker/target/release/tvm_linker",

  // Or specify version of linker
  version: "0.15.48",
},
```

### Networks

The `networks` field allows you to specify the different networks that you'll be connecting to. Each network that you're using must be defined separately, with its own connection, giver, tracing, and keys.

```typescript
networks: {
  local: {
    connection: {
      id: 1,
      group: "localnet",
      type: "graphql",
      data: {
        endpoints: [LOCAL_NETWORK_ENDPOINT],
        latencyDetectionInterval: 1000,
        local: true,
      },
    },
    giver: {
      address: "0:ece57bcc6c530283becbbd8a3b24d3c5987cdddc3c8b7b33be6e4a6312490415",
      key: "172af540e43a524763dd53b26a066d472a97c4de37d5498170564510608250c3",
    },
    tracing: {
      endpoint: LOCAL_NETWORK_ENDPOINT,
    },
    keys: {
      phrase: "action inject penalty envelope rabbit element slim tornado dinner pizza off blood",
      amount: 20,
    },
  },
  // Other networks
  test: {
    connection: {
      id: 1,
      type: "graphql",
      group: "dev",
      data: {
        endpoints: [DEV_NET_NETWORK_ENDPOINT],
        latencyDetectionInterval: 1000,
        local: false,
      },
    },
    giver: {
      address: "0:0000000000000000000000000000000000000000000000000000000000000000",
      key: "secret key",
    },
    tracing: {
      endpoint: DEV_NET_NETWORK_ENDPOINT,
    },
    keys: {
      phrase: "action inject penalty envelope rabbit element slim tornado dinner pizza off blood",
      amount: 20,
    },
  },
  venom_testnet: {
    connection: {
      id: 1000,
      type: "jrpc",
      group: "dev",
      data: {
        endpoint: VENOM_TESTNET_ENDPOINT,
      },
    },
    giver: {
      address: "0:0000000000000000000000000000000000000000000000000000000000000000",
      phrase: "phrase",
      accountId: 0,
    },
    tracing: {
      endpoint: VENOM_TESTNET_TRACE_ENDPOINT,
    },
    keys: {
      amount: 20,
    },
  },
  main: {
    connection: {
      id: 1,
      type: "graphql",
      group: "main",
      data: {
        endpoints: [MAIN_NET_NETWORK_ENDPOINT],
        latencyDetectionInterval: 1000,
        local: false,
      },
    },
    giver: {
      address: "0:0000000000000000000000000000000000000000000000000000000000000000",
      key: "secret key",
    },
    tracing: {
      endpoint: MAIN_NET_NETWORK_ENDPOINT,
    },
    keys: {
      amount: 20,
    },
  },
},
```

#### Note about Giver settings

For `networks.giver`, all known wallets and givers such as `EverWallet` or `GiverV2` (from local node) will be detected automatically. You only need to provide giver credentials - (address, secret key, or phrase with account id). If you want to use something custom you will need to provide a `giverFactory` callback for `networks.giver.giverFactory` that callback should return something that implements the `Giver` interface.

### Mocha

The `mocha` field allows you to specify settings for the Mocha testing framework.

```typescript
mocha: {
  timeout: 2000000,
},
```

This example sets the timeout for Mocha tests to 2000000 milliseconds.