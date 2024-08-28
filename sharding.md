# Sharding

An important concept within the Nuffle Protocol is the shard. Here, we outline the concept of shards and how they are integrated into Nuffle.

## Definition

At the base level, a shard is just a partition of the members that participate in the protocol. The partition is formed by aggregating members that match certain criteria, and that criteria is based on two concepts: the strategy and the likeness.

### Strategy

The strategy defines how a collection of assets are allocated. This definition doesn't suppose anything about whose assets belong to. For example, the asset allocation can define how an entity holding assets would like them restaked; or can define what assets an entity demands in order to operate. These allocations can be as complicated as desired, but a simple stategy example would be `STRATEGY = {BTC: 0.5, ETH: 0.5}`, meaning half of the allocated assets would be the cryptocurrency Bitcoin and the other half would be Ethereum.

### Likeness

The likness is simply a measure showing how similar are two entities: one that has assets to allocate, and another that needs assets to operate. We use the concept of likeness to match both sides of the strategy. 

## Summary

The concept of sharding is very similar to grouping in clustering methods from mathematics, and the overall conception of sharding allows for greater efficiency and adaptability to the market structure and conditions.

