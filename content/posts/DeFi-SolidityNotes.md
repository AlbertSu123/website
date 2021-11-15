---
title: Random Solidity Notes
date: "2021-10-18T23:46:37.121Z"
template: "post"
draft: false
slug: "solidity-notes"
category: "DeFi Notes"
tags:
  - "Notes"
  - "Solidity"
  
description: "Things I didn't know"
socialImage: "/media/image-2.jpg"
---

# [Solidity 101 Notes](https://secureum.substack.com/p/solidity-101)

1. A ‘^’ symbol prefixed to x.y.z in the pragma indicates that the source file may be compiled only from versions starting with x.y.z until x.(y+1).z. For e.g., “pragma solidity ^0.8.3;” indicates that source file may be compiled with compiler version starting from 0.8.3 until any 0.8.z but not 0.9.z. This is known as a “floating pragma."

2. NatSpec Comments: NatSpec stands for “Ethereum Natural Language Specification Format.” These are written with a triple slash (///) or a double asterisk block(/** ... */) directly above function declarations or statements to generate documentation in JSON format for developers and end-users. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). These comments contain different types of tags:

@title: A title that should describe the contract/interface

@author: The name of the author (for contract, interface)

@notice: Explain to an end user what this does (for contract, interface, function, public state variable, event)

@dev: Explain to a developer any extra details (for contract, interface, function, state variable, event)

@param: Documents a parameter (just like in doxygen) and must be followed by parameter name (for function, event)

@return: Documents the return variables of a contract’s function (function, public state variable)

@inheritdoc: Copies all missing tags from the base function and must be followed by the contract name (for function, public state variable)

@custom…: Custom tag, semantics is application-defined (for everywhere)