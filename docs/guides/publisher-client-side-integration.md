---
title: Client-Side Integration
sidebar_label: TODO
pagination_label: TODO
description: TODO
hide_table_of_contents: false
sidebar_position: 02
---

# UID2 Client-Side Integration

<!-- TODO: This sentence is copied from publisher-client-side.md -->
This guide is intended for publishers with web assets who want to generate identity tokens using UID2 for the RTB bid stream, while integrating directly with UID2 rather than UID2-enabled single-sign-on or identity providers.

Publishers can integrate with UID2 using either a client-side integration or a [server-side integration](TODO). This guide describes client-side integrations and compares them to server-side integrations so that publishers can choose which type of integration is right for them.

## Introduction

_Client-side integration_ refers to a UID2 integration in which all API calls to the UID2 service are made directly from the user's browser. _Server-side integration_ refers to a UID2 integration in which some API calls to the UID2 service are made from a server that is operated by the publisher.

Due to the lower technical resources required for client-side integrations, a publisher can integrate with UID2 in a relatively short amount of time when compared to server-side integrations.

For a publisher, choosing between a client-side integration and a server-side integration is an important step when onboarding with UID2.

## Integration Steps

<!-- TODO: If we don't have a "Bid Using UID2 Tokens" section in the diagram, remove the part about integrating with the RTB bid stream. -->
The following diagram outlines the steps required for a user to establish a UID2 token and how the UID2 token integrates with the RTB bid stream. This diagram is specific to client-side integrations.

<!-- TODO: Fill out or remove "Bid Using UID2 Tokens" section -->

![Publisher Flow](https://mermaid.ink/svg/pako:eNq1VMFu2zAM_RVCl21AamBNTj70MGSHANtqLPHNF0WiHWGO5Elyuq7ov4-S7DWJk2LDsFwiU-R7fCTFJyaMRJYzh9971AKXijeW7ysN9Ou49UqojmsPJXAHpUM7vSrCVdFvW-V2l-7L1fI2Rof_NdqDEjj1Wq-LSifzF-MRzAEtlDMy5_A-g4_O88gAK4naK_-YfMubu7vgccMz2OwQekoRDsop74BDN2ZF_A59lmK48OrAiaNI3wVhlAFjmzBeovBH13KlHXgyH3jbI5nEjuuGEqyj1XSoQWmPVqMHriXYUEsX-VvTKD2wSjzmPcukPFMjjtTwnnhIsiBHFxmCxVj1E1NiwiL3yuiQEo9lHihfQIMx4MojXIdaRgBloVitwJuI5s03UtSgRptQXWrZVEY5lUE0AymdxrJiIr0GTAXzvdXDgEQvd4EsQl8vW2SqE9MREHCL4LyxKKlLUWAQ_8bB1poHOr0iazKHtxl8UJKegdJNItkkkreb--X9u6tx8wy-Ym2RpjcFnDdmfjK-qTF8HKTQGDuE-zN1fUwlWH97hIvsD_oyH8f9JHLSFI0P_9yY-TjP_6sxiyzuJvhkGtP7V9_W4qTU9EAdUAjU1uz_dmEsxgoOiathM4FokVt38d2zGduj3XMlaes-BY-KkfY9ViynY031cL5ilX4mT3rnZv2oBcu97XHG-k4SzLCjWV7z1pEVpaIyfk6LPO7z51-PXOXu)

## Choosing between client-side and server-side integration

 The following table shows some key differences between client-side and server-side integrations that a publisher should consider when choosing between the two.

| | Client-side integration | Server-side integration |
| - | - | - |
| Expected time to integrate | ✅ Low | ❌ High |
| Technical resources required | ✅ Low (front-end, JavaScript) | ❌ High (back-end, technology of publisher's choice) |
| Runs in the user's browser | ✅ Yes | ❌ No |
| End-to-end encryption | ✅ Yes | ✅ Yes |
| Requires an API key | ❌ No | ✅ Yes |
| Requires an API secret | ❌ No | ✅ Yes |

<!-- This guide outlines the [basic steps](#integration-steps) that you need to consider if you are building an integration without using an SDK. For example, you need to decide how to implement user login and logout, how to manage UID2 identity information and use it for targeted advertising, and how to refresh tokens, deal with missing identities, and handle user opt-outs. See also [FAQs](#faqs).

To facilitate the process of establishing client identity using UID2 and retrieving advertising tokens, the web integration steps provided in this guide rely on the UID2 SDK for JavaScript. Here's an [example application](https://example-jssdk-integ.uidapi.com/) that illustrates the integration steps described in this guide and the usage of the SDK (currently only for email addresses). For the application documentation, see [UID2 SDK Integration Example](https://github.com/IABTechLab/uid2-examples/blob/main/publisher/standard/README.md).

:::warning
The SDK currently stores tokens in first-party cookies. Since implementation details like this may change in the future, to avoid potential issues, be sure to rely on the [Client-Side JavaScript SDK](../sdks/client-side-identity.md#api-reference) for your identity management.
:::

For integration scenarios for publishers that do not use the UID2 SDK for JavaScript, see [Publisher Integration Guide, Server-Only](custom-publisher-integration.md). 

:::note
If you are using Google Ad Manager and want to use the secure signals feature, first follow the steps in this guide and then follow the additional steps in the [Google Ad Manager Secure Signals Integration Guide](google-ss-integration.md).
:::

## Integration Steps  -->


<!-- TODO: This might be its own page? -->

## Client-side integration using the UID2 SDK for JavaScript and Prebid.js

### Summary

This section describes how publishers can create a client-side integraiton with UID2 using the UID2 SDK for JavaScript and Prebid.js. Because this is a client-side integration all API calls to the UID2 server are made directly from the user's browser. When choosing a client-side integration, the publisher does not need to make any server-side changes.

At a high level the steps are:

1. Add the UID2 SDK for JavaScript (SDK) to your page.
2. Pass the directly identifying information (DII) to the SDK in order to generate UID2 tokens.
3. Pass the UID2 advertising token to Prebid.js

#### Requirements:

- Basic knowledge of HTML.
- Basic knowledge of JavaScript.

### UID2 SDK for JavaScript

You must use version 3.2.0 or higher of the UID2 SDK for JavaScript.

In the following code examples, we will use the placeholder `{{ UID2_JS_SDK_URL }}` to refer to the URL of the SDK.

Add the SDK to your page using the following code:

```html
<script defer src="{{ UID2_JS_SDK_URL }}"></script>
```

The following code snippet illustrates the different SDK events that we will respond to in order to generate and refresh tokens. The code snippet will be fleshed out in the rest of this guide.

```js
// When the UID2 SDK is executed, it will look for these callbacks and invoke them.
window.__uid2 = window.__uid2 || {};
window.__uid2.callbacks = window.__uid2.callbacks || [];
window.__uid2.callbacks.push((eventType, payload) => {
  switch (eventType) {
    case "SdkLoaded":
      // The SdkLoaded event occurs just once.
      __uid2.init({});
      break;
 
    case "InitCompleted":
      // The InitCompleted event occurs just once.
      //
      // If there was a valid UID2 token, it will be in payload.identity.
      break;
 
    case "IdentityUpdated":
      // The IdentityUpdated event will happen when a UID2 token was generated or refreshed.
      break;
  }
});
```

Further documentation for the SDK can be found at [TODO](TODO).

### UID2 Base Url

By default, the SDK is configured to work with the UID2 production environment. If you are targeting the UID2 integration environment instead, provide the URL for the integration environment in your call to init:

```js
__uid2.init({
  baseUrl: "https://operator-integ.uidapi.com",
});
```

:::note
The default value for `baseUrl` points to servers in the USA. Publishers should take into account where their readers are based, and consider using a different value in order to reduce latency for their users.

For example, a publisher with an audience in Singapore can set the value to https://sg.prod.uidapi.com. This URL points to the UID2 production environment, but the servers are located in Singapore.

A publisher with a geographically distributed audience might want to set the base URL to `https://global.prod.uidapi.com`, which will direct readers to a server geographically close to them.

Further information can be found at [TODO](TODO).
:::

### Client-side configuration values

During onboarding the publisher will be provided with the following values required for the client-side integration:

- A subscription ID, and
- A public key.

There will be one set of these values for the publisher's testing environment(s), and a separate set for the publisher's production environment.

To pass these values to the SDK, create a JavaScript variable like the following, where `{{ SUBSCRIPTION_ID }}` and `{{ PUBLIC_KEY }}` are placeholders for the real values:

```js
const clientSideConfig = {
  subscriptionId: "{{ SUBSCRIPTION_ID }}",
  serverPublicKey: "{{ PUBLIC_KEY }}",
};
```

### Generate UID2 tokens

Generating UID2 tokens requires the user's directly identifying information (DII) and the client-side configuration values that are provided to the publisher.

The UID2 SDK for JavaScript provides different methods for generating tokens, depending on the nature of the DII available on the publisher's asset.

The methods are:

| Method | When to use | Normalization required by publisher? | Hashing required by publisher? |
| -- | -- | -- | -- |
| `setIdentityFromEmail` | When the user's email address is available. | ❌ No. | ❌ No. |
| `setIdentityFromEmailHash` | When the user's phone number is available. | ✅ Yes. | ❌ No. |
| `setIdentityFromPhone` | When the user's hashed, normalized email address is available. | ✅ Yes. | ✅ Yes. |
| `setIdentityFromPhoneHash` | When the user's hashed, normalized phone number is available. | ✅ Yes. | ✅ Yes. |