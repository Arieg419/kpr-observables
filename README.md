# KPR Observables

KPR Observables allow you to interact with accounts and smart contracts during development, testing, and simulations. Getting started is easy! 🚀 🚀

## Overview

1. Add Observable Data as a Javascript object `{k: v}`
2. Deploy Config Object to KPR Cloud
3. View Observable Widgets on `https://kprlabs.io`
4. Profit 🎉🎉🥳🥳

## Quick Start

`npm install @kpr-labs/observables`

**Initialize KPR Config Object**

```js
import { initKPR, addConfigObject, deployConfig } from "@kpr-labs/observables";

const API_URL = "https://kpr-server.herokuapp.com";
const SPECTRAL_ENDPOINT = "/cloud/server/spectral/config";

// KPR Init
const configData = {};
initKPR({ API_URL, API_ENDPOINT: SPECTRAL_ENDPOINT, API_KEY: "<YOUR_KEY>" });

// Add Observable Data
configData["POOL_ADMIN"] = poolAdmin;
configData["EMERGENCY_ADMIN"] = emergencyAdmin;
addConfigObject(configData);

// Deploy Observable Data
await deployConfig();
```

# Interactables

Interactables allow you to define contracts or accounts that you'd like to interact with during simulations. Interactables supports the following types:

```js
// Valid Interactable Types
const INTERACTABLE_TYPES = {
  CONTRACT: "CONTRACT",
  HISTOGRAM: "HISTOGRAM",
  PIE_CHART: "PIE_CHART",
  TIME_SERIES: "TIME_SERIES",
};
```

## Contract

Defining an interactable as a contract will allow you to view and invoke it's methods through the KPR dashboard. This is similar to what you can do with Etherscan. Let's define an interactable contract together.

![Contract Interactable]("https://github.com/Arieg419/kpr-observables/blob/master/img/InteractableContract.png)

```js
import { initKPR, addConfigObject, deployConfig } from "@kpr-labs/observables";

const API_URL = "https://kpr-server.herokuapp.com";
const SPECTRAL_ENDPOINT = "/cloud/server/spectral/config";

// KPR Init
initKPR({ API_URL, API_ENDPOINT: SPECTRAL_ENDPOINT, API_KEY: "<YOUR_KEY>" });

const exampleJson = require("../src/artifacts/contracts/example/Example.sol/Example.json");
addInteractable({
  name: exampleJson.contractName,
  abi: exampleJson.abi,
  address: exampleContract.address,
  description:
    "Omer - This is a very cool interactable, say hello and Lorem Ipsum, I appreciate the help.",
  type: INTERACTABLE_TYPES.CONTRACT,
});

const importantJson = require("../src/artifacts/contracts/important-contract/ImportantContract.sol/ImportantContract.json");
addInteractable({
  name: importantJson.contractName,
  abi: importantJson.abi,
  address: importantContract.address,
  description:
    "Yonatan - This is a very important interactable, say hello and Lorem Ipsum, I appreciate the help.",
  type: INTERACTABLE_TYPES.CONTRACT,
});

await deployInteractables();
```

## Histogram

Defining a histogram (bar chart) requires you to define the histogram `name`, as well as a payload describing how to fetch the data for each bucket in the histogram.

![Histogram]("https://github.com/Arieg419/kpr-observables/blob/master/img/InteractableHistogram.png")

```js
import { initKPR, addHistogram, deployHistograms } from "@kpr-labs/observables";

// KPR Init
initKPR({ API_URL, API_ENDPOINT: SPECTRAL_ENDPOINT, API_KEY: "<YOUR_KEY>" });

addHistogram([
  {
    title: "USERS PER TICK",
    buckets: [
    {
        contractAddress,
        abi,
        method: "getReserveData",
        title: "Tick 3",
        returnKey: "1",
        params: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", 3]
    },
    {
        contractAddress,
        abi,
        method: "getReserveData",
        title: "Tick 4",
        returnKey: "2",
        params: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", 4]
    },
    {
        contractAddress,
        abi,
        method: "getReserveData",
        title: "Tick 5",
        returnKey: "3",
        params: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", 5]
    },
    {
        contractAddress,
        abi,
        method: "getReserveData",
        title: "Tick 6",
        returnKey: "4",
        params: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", 6]
    },
    {
        contractAddress,
        abi,
        method: "getReserveData",
        title: "Tick 7",
        returnKey: "5",
        params: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", 7]
    },
  }
]);
await deployHistograms();
```

# Hardhat Network Config

In order to run and view your simulation results on KPRCloud make sure that your hardhat network config is pointing at the correct node. When signing up for KPR you should receive an API key. See the `networks` payload in the example below.

```js
const hardhatConfig: HardhatUserConfig = {
  solidity: {
    version: "0.6.12",
    settings: {
      optimizer: { enabled: true, runs: 200 },
      evmVersion: "istanbul",
    },
  },
  typechain: {
    outDir: "types",
    target: "ethers-v5",
  },
  etherscan: {
    apiKey: ETHERSCAN_KEY,
  },
  defaultNetwork: "localhost",
  networks: {
    hardhat: {
      forking: {
        url: "https://eth-mainnet.alchemyapi.io/v2/<YOUR_KEY_HERE>",
      },
    },
  },
};
```

## Additional support

Please reach out to omer@kprlabs.io :)
