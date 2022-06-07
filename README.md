# DiscordDataPackage

DiscordDataPackage is a .NET library that allows you to easily consume the data in a Discord data package by representing it as an object.

Specifically, it processes the `package.zip` file that you receive from Discord when you request all your data from them and creates an instance of the `DiscordDataPackage` class. This object exposes several properties that represent various data within the package.

## Example

Using the path to package.zip:
```csharp
var dataPackage = DiscordDataPackage.FromPath(@"path\to\package.zip");

var moneySpent = dataPackage.Payments.Sum(payment => payment.Amount);
Console.WriteLine($"Total money spent on Discord: ${moneySpent}"); // => $39.92
```

From a `ZipArchive` instance:
```csharp
ZipArchive packageZip = ZipFile.OpenRead(@"path\to\package.zip"); // Or any other way you can obtain an instance
// ... maybe some other processing or tasks
var dataPackage = DiscordDataPackage.FromZipArchive(packageZip);

//TODO
```

## Properties

## Package Specification

So far, I have not seen any attempt at defining exactly what is contained in a data package. Discord does not provide any official schema or layout, only a [generalized overview](https://support.discord.com/hc/en-us/articles/360004957991-Your-Discord-Data-Package).

Therefore, I hope the following documentation helps implementing a similar library in other languages.
It's reverse-engineered from the package layout, what Discord says about specific parts of the package, the [Discord Developer Docs](https://discord.com/developers/docs/intro), and what other projects like [DDPE](https://github.com/Androz2091/discord-data-package-explorer) do. Needless to say, it may not be fully accurate to how Discord really stores user data in their servers, but it's a best attempt at such.

Filename: `package.zip`\
Filetype: `application/zip`

### Root Directory Layout

* **account/**
  * avatar.{png, jpg, jpeg, gif, webp} -- If the user has no avatar then this file is likely not to exist.
  * [user.json](#user.json)
* **activity/**
  *  
* **messages/**

* **programs/**

* **servers/**

* **README.txt**

### user.json

This file contains all the information about the account. It is an object with the following fields:

Field | Type | Description
---|---|---
id | string | user's snowflake ID
username | string | first part of the user's tag
discriminator | string | the 4-digit discriminator of the tag
email | string | user's email address
verified | bool | whether the email on this account has been verified
avatar_hash | string | used to get user's avatar from the Discord CDN
has_mobile | bool |  Unknown. Either a flag for if the user has the mobile app installed, or if they have registered a phone number at any point in the account's history
needs_email_verification | bool | 
