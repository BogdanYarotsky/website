---
title: 'First post'
description: 'Lorem ipsum dolor sit amet'
pubDate: 'Jul 08 2022'
heroImage: '/blog-placeholder-3.jpg'
---

# Azure Functions with Rust

TLDR; Jump to the quickstart section for the step-by-step code

## Introduction

Serverless functions lure developers and managers with a simple promise - pay only for what you use. The less resources you use, the lower is the paycheck the cloud provider will biil you in the end of the month. I personally also like to think about this as environment-friendly development.

Azure documentation has straightforward quickstart guides for .NET, Python, Node.js, Java. But these technologies are not the optimal choices to maximize the power of serverless computing. They are either slow (Python, JS) or memory-hungry (.NET, Java). Usually teams accept those drawbacks for the improved speed of development until cloud compute costs become the bottleneck for them.

Rust is one obvious solution to the problem because of it's unique combination of strenghts:
1. Efficiency on par with C
2. Top-notch developer experience (modern, safe, expressive language)

Being a .NET developer during daytime, I decided to stick with Azure, namely their Functions offering to write a simple HTTP web hook in Rust. Microsoft has kindly documented how to get started but to my shock their guide is lackluster at the time of writing of this article.

First of all, following the guide didn't yield a working applicaton in the cloud because in the documentation it's assumed that a Function created in Azure is running Linux by default which was not a case for me. I've spent some time troubleshooting this since there were not obvious logs in the Azure Portal.

Another problem with the guide is over-reliance on GUI to deploy an app. A lot of magic is happening under the hood when using Visual Studio Code extension and it also takes more time to get going than a bunch of CLI commands.

The third reason why I write my own guide is that Microsoft's documentation uses Rust library called "warp". It's a very surprising choice since there is much more popular framework for building HTTP servers called "axum" which is current state-of-the-art in the rust ecosystem. Both libraries have equal performance (source) but axum offers simpler API to get started and beter community support.

Without further ado:

## Quickstart

### Requirments

1. Active Azure subscription
2. Installed Azure CLI (az)
3. Installed Rust toolchain (cargo, rustup)
4. Installed Azure Functions Core Tools (func)

### Steps

1. func init --worker-runtime custom
2. Add "enableForwardingHttpRequest": true to customHandler config in host.json
3. func new --template "HTTP Trigger" --name "func"
4. Minimal axum setup
5. build + func start to validate
6. az login
7. az group create --name <your-resource-group-name> --location <your-location>
8. az storage account create --name <yourstorageaccountname> --location <your-location> --resource-group <your-resource-group-name> --sku Standard_LRS
9. az functionapp create --resource-group telegramtrigger2 --storage-account telegramtrigger2 --os-type Linux --name telegramtrigger2 --functions-version 4 --consumption-plan-location germanywestcentral --runtime custom
10. func azure functionapp publish <your-function-app-name>










