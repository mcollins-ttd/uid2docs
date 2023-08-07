---
title: UID2 SDK for Android
description: Reference information about the Android SDK.
hide_table_of_contents: false
sidebar_position: 12
---

# UID2 SDK for Android Reference Guide

<!-- This guide includes the following information:

- [Functionality](#functionality)
- [SDK Version](#sdk-version)
- [Features](#features)
- [GitHub Open-Source Repository/Binary](#github-open-source-repositorybinary)
- [Minimum Requirements](#minimum-requirements)
- [Installation](#installation)
  -  [Installing with Gradle ](#installing-with-gradle)
  -  [Installing with Maven ](#installing-with-maven)
- [How to Use the SDK](#how-to-use-the-sdk)
- [UID2Manager API](#uid2manager-api)
  -  [Functions](#functions)
  -  [Variables](#variables)
- [Android Initialization](#android-initialization)
- [Code Samples](#code-samples) -->

You can use the UID2 SDK for Android to facilitate the process of establishing client identity using UID2 and retrieving advertising tokens on Android devices.

>NOTE: If you want to use the UID2 SDK for Android to send UID2 tokens to Google Mobile Ads (GMA), see also [UID2 GMA Plugin for Android Integration Guide](../guides/mobile-plugin-gma-android.md).

## Functionality

This SDK simplifies integration with UID2 for any publishers who want to support UID2 for apps running on Android devices. The following table shows the functions it supports.

| Encrypt Raw UID2 to UID2 Token | Decrypt UID2 Token | Generate UID2 Token from DII | Refresh UID2 Token |
| :--- | :--- | :--- | :--- |
| No | No | No | Yes |

## SDK Version

<!-- As of 2023-07-15 -->

This documentation is for the UID2 Android SDK version 0.4.0.

## Features

The UID2 Android SDK is designed to manage UID2 identity on behalf of Android apps. It enables UID2 identity to be persisted across app lifecycles by securely storing the identity on a device via platform-native encryption tools.

By default, the SDK automatically refreshes UID2 identity based on expiration dates. However, you can disable this to allow implementing apps to manage the UID2 identity lifecycle manually.

## GitHub Open-Source Repository/Binary

This SDK is in the following open-source GitHub repository:

- [https://github.com/IABTechLab/uid2-android-sdk](https://github.com/IABTechLab/uid2-android-sdk)

The binary is published on Sonatype:

- [https://central.sonatype.com/artifact/com.uid2/uid2-android-sdk/0.4.0](https://central.sonatype.com/artifact/com.uid2/uid2-android-sdk)

## Minimum Requirements

For the latest information regarding minimum requirements and specifications, refer to the [readme in the GitHub repository](https://github.com/IABTechLab/uid2-android-sdk/blob/main/README.md#requirements).

## Installation

There are two options for installing the Android UID2 SDK:

-   [Gradle](#installing-with-gradle)

-  [ Maven](#installing-with-maven)

### Installing with Gradle

To install with Gradle, add the SDK as a dependency in the build.gradle
file:

```
implementation \'com.uid2.uid2-android-sdk:0.4.0\'
```

### Installing with Maven 

To install with Maven, add the SDK as a dependency in the `pom.xml` file:

```
dependency> 
  <groupId>com.uid2</groupId> 
  <artifactId>uid2-android-sdk</artifactId> 
  <version>0.4.0</version> 
</dependency> 
```

## How to Use the SDK

The initial UID2 Identity *must* be generated by the implementing application and then passed into the UID2 SDK. Because of security requirements, the UID2 Mobile SDKs cannot create new UID2 identities.

The UID2 Mobile SDKs can perform refreshes of UID2 identities, after an Identity is established. This is because the refresh functionality relies on the refresh tokens that are part of the UID2 Identity.

The **UID2Manager** singleton is the primary developer API for the UID2 Android and iOS SDKs. It is responsible for storing, refreshing, and retrieving UID2 Identity.

For Android, you must initialize the `UID2Manager` manually before you can use it. See [Android Initialization](#android-initialization).

## UID2Manager API

This section includes the functions and variables that are part of the UID2Manager API.

### Functions

The following functions are available as part of the UID2Manager API:
- [setIdentity()](#setidentity)
- [resetIdentity()](#resetidentity)
- [refreshIdentity()](#refreshidentity)
- [getAdvertisingToken()](#getadvertisingtoken)
- [setAutomaticRefreshEnabled()](#setautomaticrefreshenabled)

#### setIdentity()

Sets the UID2 Identity to be managed by the SDK.

#### resetIdentity()

Resets or removes the UID2 Identity currently being managed by the SDK.

#### refreshIdentity()

Manually refreshes the UID2 Identity being managed by the SDK.

#### getAdvertisingToken()

If the current UID2 Identity is valid, this function returns the UID2 token (advertising token).

#### setAutomaticRefreshEnabled()

Toggle for automatic refresh functionality.

### Variables

The following variables are available as part of the UID2Manager API:

- [identity](#identity)
- [identityStatus](#identitystatus)

#### identity

The Identity variable stores and returns the current UID2Identity data object being managed by the SDK.

#### identityStatus

The identityStatus variable stores and returns the status of the current UID2 Identity being managed by the SDK.

## Android Initialization

The Android implementation expects the singleton to be initialized before use. This does two things:

-   It allows for easier access later.

-   It allows the consuming application to potentially provide its own network instance, responsible for making requests.

The initialization can be done during the creation of the APPLICATION instance, as shown in the following example:

```java
class MyApplication : Application() {
  override fun onCreate() {
    super.onCreate()
   // Initialize the UID2Manager class. Use DefaultNetworkSession rather than providing our own
   // custom implementation. This can be done to allow wrapping something like OkHttp.
   UID2Manager.init(this.applicationContext)
```

## Code Samples

The following code samples provide examples of performing specific activities relating to managing UID2 with the UID2 Android SDK.

Set the Initial UID2 Identity:

```java
UID2Manager.getInstance().setIdentity(identity: UID2Identity)
```

Get the UID2 token (advertising token) to pass to the Advertising SDK:

```java
UID2Manager.getInstance().getAdvertisingToken()
```