# Balancer Metadata

Public metadata for the Balancer protocol.

## Tags

Tags are used to tag sets of pools in the [Balancer API](https://github.com/balancer/backend) and [frontend](https://github.com/balancer/frontend-monorepo).

### Adding new tags

To add a new tag, update the [tag json](https://github.com/balancer/metadata/blob/main/pools/tag/index.json) with an object using the following schema:

```ts
type Tag = {
    id: string; // If it's a points tag, prepend id with `points_`, see existing tags for examples.
    name: string;
    description: string;
    value?: string; // Arbitrary value that may be used by the frontend to provide more context, e.g. points multiples for points tags.
    url?: string; // A link you want to associate with the tag.
    icon?: string; // Icon to be used in frontend to represent the tag. Should be a relative path, see existing tags for examples.
    pools: string[]; // A list of pool IDs that should receive the tag.
};
```

#### Adding a tag icon

To add an icon to be used for a tag, add a new file to the /icons directory with
the ID of the tag as its name, e.g. my_tag_id.svg. Then add the
`icon` property to the category object in `index.json`.

#### Creating a points tag

Tags are generic pool <> metadata associations that are ingested by the API. We use these tags for specific use cases like 3rd party point programs. To add a new point program tag there are a couple of things to pay attention to.

1. The `id` should include `points`, preferably as a prepend, e.g. `id: "points_protocol_name"`.
2. If the points program has a multiple, you can use the `value` attribute to specify that multiple. If the multiple is 5x then `value: "5"`.

## Pools

### Adding pool metadata

To add new pool metadata, update the [pools
json](https://github.com/balancer/metadata/blob/main/pools/index.json) with a
object using the following schema:

```ts
type PoolMeta = {
    name?: string
    description?: string
    iconUrl?: string
}

## Aggregators

Aggregators are used to tag swaps in a pool's activity log in the frontend. A swap is tagged when, in the swap's
transaction, one of the aggregator's addresses on the pool's chain transferred tokens into the vault
(`0xFF00009C1A83Ec25434522ef355b99e9a70000FF`).

These are usually the aggregator's executor contracts, not its router or the transaction sender. To find one, open an
aggregator-routed swap on a block explorer and look at the token transfer whose recipient is the vault: its sender is
the address to add.

### Adding or updating an aggregator

Update the [aggregators json](aggregators/index.json) with an object using the following schema:

```ts
type Aggregator = {
    id: string; // Prefixed with `aggregator_`, e.g. `aggregator_1inch`.
    name: string;
    logo: string; // File name of the logo in aggregators/images, e.g. `1inch.png`. Shared across all chains.
    url?: string; // The aggregator's website.
    addresses: Record<string, string[]>; // Chain ID -> addresses that transfer tokens into the vault on that chain.
};
```

Addresses can differ per chain, so list them under each chain ID, e.g.

```json
"addresses": {
    "1": ["0x1111111111111111111111111111111111111111"],
    "8453": ["0x2222222222222222222222222222222222222222"]
}
```

Add the logo to `aggregators/images` (square, at least 64x64).
