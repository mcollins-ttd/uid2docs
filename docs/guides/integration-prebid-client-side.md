---
title: Prebid Integration
sidebar_label: Prebid
pagination_label: Prebid Integration
description: Information about integrating with Prebid as part of your UID2 implementation.
hide_table_of_contents: false
sidebar_position: 04
---

# Prebid Integration Guide

This guide is for publishers who want to integrate with UID2 and generate [UID2 tokens](../ref-info/glossary-uid.md#gl-uid2-token) (advertising tokens) to be passed by Prebid in the RTB bid stream.

It outlines the basic steps to consider if you're building a direct integration with UID2 and you use Prebid for header bidding. 

## Introduction

If you're a publisher using Prebid for header bidding, there are a few extra steps to take so that your Prebid header bidding implementation also supports UID2.

<!-- TODO -->
In addition, if you don't already have one, you must set up a UID2 account: see [Account Setup](../getting-started/gs-account-setup.md).

## Integration Steps

At a high level, to integrate with UID2 using Prebid you will need to complete the following steps:

1. Complete UID2 Account Setup.
<!-- TODO: link -->
1. Add Prebid to your page.
2. Configure the UID2 User ID submodule.

Prebid will take care of generating 

### Complete UID2 Account Setup

<!-- TODO -->

### Add Prebid to your page

<!-- TODO -->

### Configure the UID2 User ID submodule

<!-- To configure the module to use this mode, you must: -->

To configure the UID2 User ID submodule, you will need the **public key** and **subscription ID** that you obtained during account setup, as well as the user's DII.

<!-- Set parmas.serverPublicKey and params.subscriptionId (please reach out to the UID2 team to obtain these values) -->

#### Example

```js
pbjs.setConfig({
  userSync: {
    userIds: [{
      name: 'uid2',
      params: {
        serverPublicKey: '...server public key...',
        subscriptionId: '...subcription id...',
        email: 'user@example.com',
      }
    }]
  }
});
```

#### Example

```js
pbjs.setConfig({
  userSync: {
    userIds: [{
      name: 'uid2',
      params: {
        serverPublicKey: '...server public key...',
        subscriptionId: '...subcription id...',
        // TODO
        phone: 'user@example.com',
      }
    }]
  }
});
```

#### Example

```js
pbjs.setConfig({
  userSync: {
    userIds: [{
      name: 'uid2',
      params: {
        serverPublicKey: '...server public key...',
        subscriptionId: '...subcription id...',
        // TODO
        emailHash: 'user@example.com',
      }
    }]
  }
});
```

#### Example

```js
pbjs.setConfig({
  userSync: {
    userIds: [{
      name: 'uid2',
      params: {
        serverPublicKey: '...server public key...',
        subscriptionId: '...subcription id...',
        // TODO
        phoneHash: 'user@example.com',
      }
    }]
  }
});
```

Provide ONLY ONE DII by setting ONLY ONE of params.email/params.phone/params.emailHash/params.phoneHash
Below is a table that provides guidance on when to use each directly identifying information (DII) parameter, along with information on whether normalization and hashing are required by the publisher for each parameter.

UID2 will provide the publisher with the following values required to use the client-side token generation feature:

A subscription ID, and
A public key.
There will be one set of these values for the publisher's testing environment(s), and a separate set for the publisher's production environment.

An example of such configuration which will be passed into the token generation method mentioned further down is:
