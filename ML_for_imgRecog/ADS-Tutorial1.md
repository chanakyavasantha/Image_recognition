# CS21B2008 - Tutorial 1

## Table of Contents
1. [Introduction](#introduction)  
2. [Perfect Hashing](#perfect-hashing)  
   - 2.1 [Definition](#definition)  
   - 2.2 [Types of Perfect Hashing](#types-of-perfect-hashing)  
   - 2.3 [Designing a Perfect Hash Function](#designing-a-perfect-hash-function)  
3. [IP Lookup and Longest Prefix Matching](#ip-lookup-and-longest-prefix-matching)  
   - 3.1 [IP Addressing Basics](#ip-addressing-basics)  
   - 3.2 [Longest Prefix Matching (LPM)](#longest-prefix-matching-lpm)  
4. [Hashing Techniques for IP Lookup](#hashing-techniques-for-ip-lookup)  
   - 4.1 [Trie-based Approaches](#trie-based-approaches)  
   - 4.2 [Binary Search on Prefixes](#binary-search-on-prefixes)  
   - 4.3 [Hash-based Approaches](#hash-based-approaches)  
5. [Designing an Efficient IP Lookup System](#designing-an-efficient-ip-lookup-system)  
   - 5.1 [Requirements](#requirements)  
   - 5.2 [Proposed Architecture](#proposed-architecture)  
6. [Performance Analysis](#performance-analysis)  
7. [Conclusion](#conclusion)  


## 1. Introduction

In computer networking, IP lookup is a crucial operation that determines how fast data packets are routed across the internet. The process of finding the correct route for each packet relies on efficiently matching an IP address to a routing prefix. A major challenge here is to perform this lookup as quickly as possible, even as routing tables become very large. One approach to address this is using **perfect hashing** for IP lookup, particularly for the **Longest Prefix Match (LPM)** problem. This report explores how perfect hashing can be applied to make IP lookups faster and more efficient.

## 2. Perfect Hashing

### 2.1 Definition

Perfect hashing is a hashing technique that guarantees no collisions for a specific set of keys, meaning that each key is assigned a unique slot in a hash table. This ensures that the lookup time is always constant, i.e., O(1) in the worst case.

### 2.2 Types of Perfect Hashing

1. **Static Perfect Hashing**: Used when the set of keys is fixed and does not change.  
2. **Dynamic Perfect Hashing**: Allows for inserting and deleting keys while maintaining the perfect hashing property.

### 2.3 Designing a Perfect Hash Function

To design a perfect hash function, the goal is to avoid collisions entirely. Here's a general approach:

1. **Select a hash function** from a family of universal hash functions.
2. **Hash the set of keys** and observe if any collisions occur.
3. If there are collisions, use a **secondary hash function** or rehash the set until all keys map to unique slots.

For example, a two-level hashing technique is commonly used. In this method, a primary hash function divides the keys into "buckets." For each bucket with more than one key, a secondary hash function is applied to ensure there are no collisions.

## 3. IP Lookup and Longest Prefix Matching

### 3.1 IP Addressing Basics

IP addresses (IPv4 or IPv6) identify devices on a network. Each address can be broken down into a prefix, which represents a network or a subnet. When routing packets, routers need to match the destination IP address to the most specific (longest) prefix in their routing table.

### 3.2 Longest Prefix Matching (LPM)

The Longest Prefix Match problem involves finding the most specific prefix that matches a given IP address. This process is crucial for forwarding packets to the correct destination in a network. Traditional approaches like trie-based methods are common but can be slow for large routing tables, making it necessary to explore other techniques like hashing.

## 4. Hashing Techniques for IP Lookup

### 4.1 Trie-based Approaches

Tries, or prefix trees, are commonly used for storing IP prefixes. They allow for efficient matching but can consume a lot of memory as the number of prefixes grows.

### 4.2 Binary Search on Prefixes

Another approach is sorting the prefixes and using binary search to find the longest match. This method is more space-efficient than tries but can be slower, especially as the size of the prefix table increases.

### 4.3 Hash-based Approaches

Hashing offers a way to perform faster lookups but can be challenging when dealing with prefixes. Some common techniques include:

1. **Hashing the prefix bits**: Hash the first `n` bits of the IP address.
2. **Controlled Prefix Expansion**: Expand prefixes to a fixed length and apply perfect hashing.
3. **Bloom Filters**: Use multiple hash functions to minimize false positives when searching for matching prefixes.

## 5. Designing an Efficient IP Lookup System

### 5.1 Requirements

For an efficient IP lookup system, the following requirements must be met:

- Fast lookup time, ideally O(1)
- Efficient use of memory
- Support for large routing tables
- Ability to handle Longest Prefix Matching

### 5.2 Proposed Architecture

We propose a hybrid approach that combines perfect hashing with trie-based techniques. The key steps are:

1. **Prefix Expansion**: Expand all prefixes to a fixed length to ensure uniformity.
2. **Perfect Hashing**: Create a perfect hash table for the expanded prefixes.
3. **Trie for Collision Resolution**: Use small tries to handle any colliding prefixes within each hash bucket.

Here's a simplified Python implementation:

```python
class PrefixNode:
    def __init__(self, prefix, next_hop):
        self.prefix = prefix
        self.next_hop = next_hop
        self.left = None
        self.right = None

class IPLookupSystem:
    def __init__(self, prefixes, expansion_bits):
        self.expansion_bits = expansion_bits
        self.hash_table = self._build_hash_table(prefixes)

    def _build_hash_table(self, prefixes):
        # Implement perfect hashing here
        # For each prefix, expand and hash it
        pass

    def lookup(self, ip_address):
        expanded_prefix = ip_address >> (32 - self.expansion_bits)
        bucket = self.hash_table[hash(expanded_prefix)]
        return self._search_trie(bucket, ip_address)

    def _search_trie(self, trie_root, ip_address):
        # Implement longest prefix matching in the trie
        pass
```

## 6. Performance Analysis

The proposed system offers:

- **O(1) lookup time** on average.
- Reduced memory usage compared to full trie-based implementations.
- Efficient handling of Longest Prefix Matching through a combination of hashing and trie techniques.

However, tuning the expansion bits and hash functions is crucial for optimizing performance, as these parameters can affect both speed and memory usage.

## 7. Conclusion

In this report, we've shown that combining perfect hashing with traditional IP lookup methods like tries can lead to a more efficient system for routing table lookups. This hybrid approach provides both speed and memory efficiency, which are essential in handling the ever-growing size of internet routing tables.

Future work could focus on optimizing the hash functions further and exploring hardware implementations to accelerate the lookup process even more.
