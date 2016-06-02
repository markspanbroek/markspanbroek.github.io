---
title: Ethereum on mobile phones
layout: post
---

I’m interested in seeing [Ethereum](https://ethereum.org) work on mobile phones. So I started reading up on the state of its development and want to share my findings here. I hope that it may prove useful to you. Please be aware that this is the state of affairs as I understand it at the moment of writing (June 2016). Ethereum development tends to go fast, so the information in this post will likely be outdated soon.

Currently there is no practical way to connect to the Ethereum network from a phone. I did find a tutorial on how to connect to the network from an iOS app, but it requires you to connect to a full ethereum node that runs elsewhere. There are trust issues with this, and [can be downright dangerous](https://blog.ethereum.org/2015/08/29/security-alert-insecurely-configured-geth-can-make-funds-remotely-accessible/).

Luckily, there is a solution in the works. The so-called Ethereum Light Client is being actively worked on, and will provide a way to access the Ethereum network from a resource and bandwidth constrained device, such as a phone. A proposal for the light client has been [introduced by Vitalik Buterin and Gavin Wood](https://www.youtube.com/watch?v=WD2sRXbmims) at the ÐΞVcon-0 conference in 2014. At the ÐΞVcon-1 conference in 2015 Zsolt Felföldi [presented an update](https://www.youtube.com/watch?v=KoEqh99U5QI) on the state of its development.

The latest state of the light client is being [discussed here](https://gitter.im/ethereum/light-client). Zsolt Felföldi is currently working on getting the light client integrated into the Go implementation of Ethereum (geth). It didn’t make the 1.4 release, but will likely be released as part of 1.5. This will then be the first implementation of the light client to be completed. Bob Summerwill, who is working on the C++ Ethereum client (eth), has expressed the desire to implement the light client in C++ as well.

The underlying protocol for the light client is also under development, and is [documented here](https://github.com/zsfelfoldi/go-ethereum/wiki/Light-Ethereum-Subprotocol-(LES)).

Jarrad Hope of [Syng](http://syng.im) is currently working on integrating the as-of-yet unfinished work on the ethereum light client into Android and iOS. He presented his progress [in this video](https://www.youtube.com/watch?v=uWAfS1Qr-ZI). His implementation is not publicly available yet.

[Comments, questions](mailto:mark@spanbroek.net)?