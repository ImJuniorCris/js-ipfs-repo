# JsIPFSRepo

> The repo and migration tools used by IPFS, providing a storage backend and API for managing block, datastore, keys, and config data.

## About

A JavaScript implementation of the IPFS Repo, providing a flexible storage backend and migration tools for IPFS nodes.

## Table of Contents

- [Install](#install)
- [Background](#background)
- [Usage](#usage)
- [API](#api)
  - [Setup](#setup)
  - [Repos](#repos)
  - [Blocks](#blocks)
  - [Datastore](#datastore)
  - [Config](#config)
  - [Version](#version)
  - [API Addr](#api-addr)
  - [Status](#status)
  - [Lock](#lock)
- [Notes](#notes)
  - [Migrations](#migrations)
- [Contribute](#contribute)
- [License](#license)

## Install

```bash
npm install ipfs-repo
```

## Background

This repository implements the IPFS Repo architecture, providing a modular and pluggable interface for storing blocks, keys, configuration, and metadata. It supports multiple backend datastores, with default options for Node.js and browser environments.

## Usage

```js
import { createRepo } from 'ipfs-repo'

const repo = createRepo('/tmp/ipfs-repo')

await repo.init({ cool: 'config' })
await repo.open()
console.log('repo is ready')
```

This creates the following structure (on disk or in-memory):

```
├── blocks
│   ├── SHARDING
│   └── _README
├── config
├── datastore
├── keys
└── version
```

## API

### Setup

- `createRepo(path[, options])` – Creates an IPFS Repo.
- `Promise repo.init()` – Initializes the repo folder structure.
- `Promise repo.open()` – Locks the repo for usage.
- `Promise repo.close()` – Unlocks the repo.
- `Promise<boolean> repo.exists()` – Checks if the repo exists.
- `Promise<boolean> repo.isInitialized()` – Checks if the repo is initialized.

### Repos

- `Promise repo.put(key, value:Uint8Array)` – Store a value.
- `Promise<Uint8Array> repo.get(key)` – Retrieve a value.

### Blocks

- `Promise<Block> repo.blocks.put(block:Block)`
- `AsyncIterator<Block> repo.blocks.putMany(source:AsyncIterable<Block>)`
- `Promise<Block> repo.blocks.get(cid:CID)`
- `AsyncIterable<Block> repo.blocks.getMany(source:AsyncIterable<CID>)`
- `Promise<boolean> repo.blocks.has(cid:CID)`
- `Promise<boolean> repo.blocks.delete(cid:CID)`
- `AsyncIterator<Block|CID> repo.blocks.query(query)`

### Datastore

- `repo.datastore` – Implements the `interface-datastore` API.

### Config

- `Promise repo.config.set(key:String, value:Object)`
- `Promise repo.config.replace(value:Object)`
- `Promise<?> repo.config.get(key:String)`
- `Promise<Object> repo.config.getAll()`
- `Promise<boolean> repo.config.exists()`

### Version

- `Promise<Number> repo.version.get()`
- `Promise repo.version.set(version:Number)`

### API Addr

- `Promise<String> repo.apiAddr.get()`
- `Promise repo.apiAddr.set(value)`

### Status

- `Promise<Object> repo.stat()`

### Lock

IPFS Repo supports memory and fs locks. You may also provide a custom lock implementation.

```js
import { FSLock } from 'ipfs-repo/locks/fs'      // Default in Node.js
import { MemoryLock } from 'ipfs-repo/locks/memory' // Default in browser
```

#### Migrations

When repo migrations are needed, ensure to propagate changes as needed. For browser-based tools, provide a way to trigger migrations manually, since CLI access may not be available.

## License

Dual licensed under Apache-2.0 and MIT.
