---
title: Tornado Cash
date: "2021-12-30T23:46:37.121Z"
template: "post"
draft: true
slug: "borrow"
category: "crypto"
tags:
  - "Notes"
  - "Crypto"
  - "tornadocash"

description: "A quick primer on Tornado Cash"
socialImage: "/media/image-2.jpg"
---

# Mixers and Tornado Cash

Constructing a mixing scheme with a Trusted Intermediary

1. If you want to wash your funds, you can use a trusted intermediary and communicate via a secret channel
2. There are two people, bob and joe. Bob made a dollar from selling black tar heroin. Joe made a dollar from selling lemonade.
3. Each person gives their dollar to the washing machine along with their private key.
4. A while later, they ask for their dollar back from the washing machine using their private key.
5. However, you don't know who has the clean dollar since you don't know which private key corresponds to the black tar heroin dollar.

Let A be an adversary who wants to guess which dollar went to agent 1, what is the best they can do?
1/2 since a blind guess is the best

## Important Definitions

Diffie Hellman Key Exchange - a way to exchange packets of information that allow you to establish a private shared secret key
Merkle Tree - You have an array of data blocks, you hash data blocks pairwise to create a tree
Pederson Hash Function - Maps a random number to a point onto an elliptic curve
MiMc Hash - Used to repeated hash things to a merkle tree, used to compute hashes in merkle tree

withdraw - prove you know k,r of the secret
also need to tip a relayer to pay the gas fee for you, since you are on a fresh account with no eth
