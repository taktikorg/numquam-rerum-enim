# OTP io

> Typed library to work 2fa via Google Authenticator/Time-based TOTP/Hmac-based HOTP

[![Test Status](https://github.com/taktikorg/numquam-rerum-enim/actions/workflows/test.yml/badge.svg)](https://github.com/taktikorg/numquam-rerum-enim)
[![Downloads](https://img.shields.io/npm/dt/@taktikorg/numquam-rerum-enim.svg)](https://npmjs.com/package/@taktikorg/numquam-rerum-enim)
[![last commit](https://img.shields.io/github/last-commit/taktikorg/numquam-rerum-enim.svg)](https://github.com/taktikorg/numquam-rerum-enim)
[![codecov](https://img.shields.io/codecov/c/github/taktikorg/numquam-rerum-enim/main.svg)](https://codecov.io/gh/taktikorg/numquam-rerum-enim)
[![GitHub](https://img.shields.io/github/stars/taktikorg/numquam-rerum-enim.svg)](https://github.com/taktikorg/numquam-rerum-enim)
[![@taktikorg/numquam-rerum-enim](https://snyk.io/advisor/npm-package/@taktikorg/numquam-rerum-enim/badge.svg)](https://snyk.io/advisor/npm-package/@taktikorg/numquam-rerum-enim)
[![Known Vulnerabilities](https://snyk.io/test/npm/@taktikorg/numquam-rerum-enim/badge.svg)](https://snyk.io/test/npm/@taktikorg/numquam-rerum-enim)
[![Quality](https://img.shields.io/npms-io/quality-score/@taktikorg/numquam-rerum-enim.svg?label=quality%20%28npms.io%29&)](https://npms.io/search?q=@taktikorg/numquam-rerum-enim)
[![npm](https://img.shields.io/npm/v/@taktikorg/numquam-rerum-enim.svg)](https://npmjs.com/package/@taktikorg/numquam-rerum-enim)
[![license MIT](https://img.shields.io/npm/l/@taktikorg/numquam-rerum-enim.svg)](https://github.com/taktikorg/numquam-rerum-enim/blob/main/LICENSE.txt)
[![Size](https://img.shields.io/bundlephobia/minzip/@taktikorg/numquam-rerum-enim)](https://bundlephobia.com/package/@taktikorg/numquam-rerum-enim)

[Example](#how-it-works) &bull; [API Reference](./docs/api/README.md)

## Why use this lib?

- **Small.** Tree-shakable, 0 dependencies
- **Tested.** Compatibility with [Google Authenticator](https://github.com/google/google-authenticator/wiki/Key-Uri-Format) and with [RFC4226 (HOTP)](https://www.ietf.org/rfc/rfc4226.txt) and [RFC6238 (TOTP)](https://www.ietf.org/rfc/rfc6238.txt)

## Install

- `npm`
  ```bash
  npm i @taktikorg/numquam-rerum-enim
  ```
- `Yarn`
  ```bash
  yarn add @taktikorg/numquam-rerum-enim
  ```

## What is this?

- `HOTP` - HMAC-based One Time Password generation method. Uses incrementing with each login `counter` and `secret` to generate unique 6-8 digit codes.
- `TOTP` - Time-based, uses `current time` modulo `period` (seconds) as counter in `HOTP`,
- `Google Authenticator` - uses simplified version of `TOTP` to generate codes. Differences:
  - Only `SHA-1` hash support
  - Only 6 digit codes
  - Keys should not be padded
  - TOTP period is 30 seconds

Google Authenticator limits are defaults for this library.

## How it works?

```typescript
// 1. Import library - use totp (code changes with time)
import { totp, generateKey, getKeyUri } from "@taktikorg/numquam-rerum-enim";
// 2. Import crypto adapter. Either `crypto-node` or `crypto-web` - API is identical
import { hmac, randomBytes } from "@taktikorg/numquam-rerum-enim/crypto-node";

// 3. Get key from somewhere. Or generate it
const key = generateKey(randomBytes, /* bytes: */ 20); // 5-20 good for Google Authenticator

// 4. Get key import url
const url = getKeyUri({
  type: "totp",
  secret,
  name: "User's Username",
  issuer: "Your Site Name"
});

// 5. Show it to user as QR code - send it back to client
// Get 6-digit code back from him, as confirmation of saving secret key

const input = "...";

const code = await totp(hmac, { secret });

if (code === input) {
  // 6. Done. User configured your key
}
```

## Api Reference

[API Reference](./docs/api/modules.md)
