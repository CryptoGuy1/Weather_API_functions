# 🌤️ On-Chain Weather App using Chainlink Functions

This is a decentralized weather query application that fetches real-time temperature data from a Web2 API (`wttr.in`) and stores the response on-chain using [Chainlink Functions](https://docs.chain.link/chainlink-functions/introduction). It demonstrates how to bridge **off-chain APIs** with **smart contracts** using secure, decentralized computation.

> Deployed on **Sepolia Testnet** using the Chainlink Functions Playground.

---

## 🧠 How It Works

- Users call `getTemperature(city)` from the smart contract.
- Chainlink Functions executes JavaScript code that queries `https://wttr.in/{city}?format=3&m`.
- The temperature string is encoded and returned on-chain.
- The contract stores the weather result, sender, and timestamp in a persistent struct.
- Users can query the results or list all cities queried on-chain.

---

## 🛠 Tech Stack

| Tool / Network     | Purpose                                      |
|--------------------|----------------------------------------------|
| **Solidity 0.8.19**| Smart contract logic                         |
| **Chainlink Functions** | Off-chain Web2 API integration          |
| **Sepolia Testnet**| Deployment & interaction network             |
| **wttr.in**        | Free weather API                             |

---

## 🔗 Contract Summary

### `WeatherFunctions.sol`

- ✅ Fetches real-time weather for any city.
- ✅ Stores responses in a mapping and array (`CityStruct[]`).
- ✅ Emits a `Response` event on each successful fulfillment.
- ✅ Enables querying weather results via `getCity(city)` or `listCities(...)`.

---

## ⚙️ Key Features

- **Chainlink Functions Integration** with JavaScript inline source code
- **Off-chain Weather API** call using `Functions.makeHttpRequest`
- **Response Logging** via on-chain event `Response(...)`
- **Persistent Storage** of past queries and their results
- **Public View Functions**:
  - `getCity(city)`
  - `listAllCities()`
  - `listCities(start, end)`

---

## 🧪 Example Usage

### 1. Call `getTemperature("New York")`

Triggers a Chainlink Functions request to fetch New York’s weather.

### 2. Wait for the Fulfillment

After Chainlink oracle fulfills the request, view the result:

```solidity
getCity("New York")
```

Or listen for:

```solidity
event Response(bytes32 requestId, string temperature, bytes response, bytes err);
```

### 3. List Past Requests

```solidity
listAllCities();
listCities(0, 4);
```

---

## 📜 Chainlink Functions JavaScript Code

This JS code is injected inline and executed by Chainlink Functions:

```javascript
const city = args[0];
const apiResponse = await Functions.makeHttpRequest({
  url: `https://wttr.in/${city}?format=3&m`,
  responseType: 'text'
});
if (apiResponse.error) {
  throw Error('Request failed');
}
const { data } = apiResponse;
return Functions.encodeString(data);
```

---

## 📦 Deployment & Configuration

### Requirements

- Chainlink Functions Playground or [functions-tool](https://github.com/smartcontractkit/functions-hardhat-starter-kit)
- Funded subscription ID on Sepolia (with LINK)

### Constructor

```solidity
constructor(uint64 functionsSubscriptionId)
```

Pass your Chainlink Functions subscription ID during deployment.

---

## 🧰 Developer Tools

| Function                     | Description                                  |
|------------------------------|----------------------------------------------|
| `getTemperature(city)`       | Initiates the off-chain API query            |
| `fulfillRequest(...)`        | Handles Chainlink callback (internal)        |
| `getCity(city)`              | Returns the latest response and sender info |
| `listAllCities()`            | Returns all queried cities                   |
| `listCities(start, end)`     | Paged query for city list                    |

---

## 🧠 Learn More

- [Chainlink Functions Docs](https://docs.chain.link/chainlink-functions/introduction)
- [wttr.in API](https://wttr.in/:help)
- [Chainlink Playground](https://functions.chain.link/)
- [Chainlink Blog – Calling Web APIs from Smart Contracts](https://blog.chain.link/calling-any-api-from-your-smart-contract/)

---

## 👨‍💻 Author

Built with ❤️ by a Chainlink Functions enthusiast exploring on-chain Web2 integrations.

---

## 📜 License

MIT © 2025

