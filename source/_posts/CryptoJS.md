---
title: CryptoJS
tags: Encryption
abbrlink: 331a7bb4
date: 2021-01-25 20:22:12
---

# CryptoJS

**It is a powerful encryption library in JavaScript**

## Install

```javascript
yarn add crypto-js
npm install crypto-js
```

## Usage

```javascript
// Return a object (WordArray)
CryptoJS.MD5("smallstars");

// Return string (default Hex)
CryptoJS.MD5("smallstars").toString(CryptoJS.enc.Hex);

// Return the object before conversion
CryptoJS.enc.Hex.parse(CryptoJS.MD5("smallstars").toString(CryptoJS.enc.Hex));
```

## Simple Sample

```javascript
import CryptoJS from "crypto-js";

// The hexadecimal characters of sixteen length as the key
const key = CryptoJS.enc.Utf8.parse("1234123412ABCDEF");
// The hexadecimal characters of sixteen length as the offset of key
const iv = CryptoJS.enc.Utf8.parse("ABCDEF1234123412");

// encrypt
function encrypt(message) {
  let srcs = CryptoJS.enc.Utf8.parse(message);
  let encrypted = CryptoJS.AES.encrypt(srcs, key, {
    iv: iv,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  return encrypted.ciphertext.toString().toUpperCase();
}

// decrypt
function decrypt(message) {
  let encryptedHexStr = CryptoJS.enc.Hex.parse(message);
  let srcs = CryptoJS.enc.Base64.stringify(encryptedHexStr);
  let decrypt = CryptoJS.AES.decrypt(srcs, key, {
    iv: iv,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  let decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);
  return decryptedStr.toString();
}
```

## References

- [Official Website](http://cryptojs.altervista.org/)
