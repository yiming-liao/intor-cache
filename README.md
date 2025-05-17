# IntorCache

A generic, in-memory cache class with TTL (time-to-live) expiration and size limit, designed to efficiently store and manage cached data.

> **Note:** Used in [Intor](https://github.com/yiming-liao/intor) i18n library to cache translations for better performance.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

`IntorCache` is a simple yet effective cache implementation in TypeScript that provides:

- **TTL-based expiration**: Automatically removes stale cache entries after a configurable duration.
- **Max size limit**: Keeps the cache size bounded by evicting the oldest entries once capacity is exceeded.
- **Generic support**: Can store any type of value with full TypeScript type safety.
- **Automatic cleanup**: Cleans expired entries on each cache operation to maintain freshness.

This makes `IntorCache` suitable for caching frequently accessed data like translations, API responses, or computed results, enhancing performance and resource usage.

---

## Features

- Time-to-live (TTL) expiration with automatic cleanup
- Maximum cache size with oldest-entry eviction
- TypeScript generic support for flexible value types
- Easy-to-use API: get, set, has, delete, clear
- Export current cache state for debugging or testing

---

## Installation

Install via npm or yarn:

```bash
npm install intor-cache
```

or use **yarn**

```bash
yarn add intor-cache
```

---

## Usage

```typescript
import { IntorCache } from "intor-cache";

const cache = new IntorCache<string>(100, 1000 * 60 * 5); // max 100 items, 5 minutes TTL

// Set a cache item
cache.set("greeting", "Hello, world!");

// Get a cache item
const message = cache.get("greeting");
console.log(message); // Output: 'Hello, world!'

// Check if a key exists
console.log(cache.has("greeting")); // true

// Delete a cache item
cache.delete("greeting");

// Clear all cache entries
cache.clear();
```

---

## API Reference

`constructor(maxSize?: number, ttl?: number)`

Creates a new IntorCache instance.

- maxSize (optional): Maximum number of cache entries (default: 100).
- ttl (optional): Time-to-live for each entry in milliseconds (default: 5 minutes).

`get(key: string): T | undefined`

Retrieves the cached value for the given key. Returns undefined if not found or expired.

`set(key: string, value: T): void`

Sets a new cache entry for the given key and value. If the cache exceeds maxSize, the oldest entry is removed.

`has(key: string): boolean`

Returns true if the cache contains a valid (not expired) entry for the key.

`delete(key: string): boolean`

Removes the entry associated with the key. Returns true if the entry existed and was deleted.

`clear(): void`

Clears all cache entries.

`exportCache(): Map<string, { value: T; timestamp: number }>`

Returns a shallow copy of the current cache state. Useful for inspection or testing.

---

## Examples

```typescript
async function simulateCache() {
  const cache = new IntorCache<number>(3, 2000); // small cache with 2 sec TTL

  cache.set("a", 1);
  cache.set("b", 2);

  console.log(cache.get("a")); // 1

  // Wait 3 seconds to let TTL expire
  await new Promise((r) => setTimeout(r, 3000));

  console.log(cache.get("a")); // undefined (expired)

  cache.set("c", 3);
  cache.set("d", 4);
  cache.set("e", 5);

  console.log(cache.exportCache().keys()); // only 3 newest keys due to maxSize
}

simulateCache();
```

---

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests to improve the cache or add features.

---

## License

This project is licensed under the MIT License. See LICENSE for details.
