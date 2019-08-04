- [STL Container map](#stl-container-map)
  - [1. map](#1-map)
    - [1.1. 常用 API](#11-常用-api)
    - [1.2. 实现原理](#12-实现原理)
  - [2. multimap](#2-multimap)
  - [3. unordered_map](#3-unordered_map)
    - [3.1. 常用 API](#31-常用-api)
    - [3.2. 实现原理](#32-实现原理)
  - [4. unordered_multimap](#4-unordered_multimap)
  - [5. Refer Links](#5-refer-links)

# STL Container map

## 1. map

https://en.cppreference.com/w/cpp/container/map

### 1.1. 常用 API

### 1.2. 实现原理

map containers are generally slower than unordered_map containers to access individual elements by their key, but they allow the direct iteration on subsets based on their order.

## 2. multimap

## 3. unordered_map

https://en.cppreference.com/w/cpp/container/unordered_map

### 3.1. 常用 API

### 3.2. 实现原理

unordered_map containers are faster than map containers to access individual elements by their key, although they are generally less efficient for range iteration through a subset of their elements.

## 4. unordered_multimap

## 5. Refer Links

[Is unordered_set faster than set?](https://codeforces.com/blog/entry/22947)
