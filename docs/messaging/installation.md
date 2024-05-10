---
sidebar_position: 1
---

# Installation

Installing and configuring **RTM Server** on server-side and **RTM Client** on application side:

## Requirements & Dependencies

- [Node.js](https://nodejs.org/en/download/) version 18.0 or above.
- (Optional) [Redis 7](https://redis.io/download/) instance running locally or on a server.

## Licensing

To get get access to the Real-Time Messaging SDK, you need to obtain a license. [Visit our product page](https://store.zexware.com/buy/767cf43b-8d3e-487a-ad60-6476fa8d98f6) to subscribe and obtain your license.

After subscribing, please wait until you are granted access to the private NPM packages before proceeding.

The license offers:

✅ Access to Zexcore RTM SDK, including `@zexcore/rtm-server` and `@zexcore/rtm-client`.

✅ Included any upcoming and under-development features.

❌ Does not include Zexcore Analytics. See Analytics page for details.

## Accessing Private Packages

Once you have obtained your license, you will get access to the private packages and the repository, with source code and examples. Following are the steps to needed to access the private packages, which are hosted on our GitHub npm package registry.

1. Obtain a GitHub Personal Access Token with `packages:read` permission.
2. Add the token to your environment variables, name it something like `NPM_ZEXCORE_TOKEN`.
3. Create an `.npmrc` file in your project root with the following content (remember to set the environment variable's name accordingly!):

```
//npm.pkg.github.com/:_authToken=${NPM_ZEXCORE_TOKEN}
@zexcore:registry=https://npm.pkg.github.com/
```

## Installation

Make sure you have added the `.npmrc` file to your project with your GitHub Personal Access Token.

### RTM Server

Open your terminal and navigate to the project root directory. Run the following command to install the Zexcore RTM Server SDK in your project:

```bash
npm install @zexcore/rtm-server
```

### RTM Client

Run the following command in your client-application's project root directory to install the Zexcore RTM Client in your project:

```bash
npm install @zexcore/rtm-client
```
