# DSE4101-CryptoWhale-2











#### Features

##### Whale transaction data

1. whale_txn_count — Number of whale transactions in the current hour (t−1h, t].
2. whale_burst_flag — 1 if whale_txn_count ≥ the 95th percentile of the previous 24 hours’ counts (window excludes the current hour via shift(1)), else 0.
3. etow_usd_log — Log-scaled USD inflows from exchanges → whales in the current hour.
4. etow_coins_log — Log-scaled coin amount inflows from exchanges → whales in the current hour.
5. wtoe_usd_log — Log-scaled USD outflows from whales → exchanges in the current hour.
6. wtoe_coins_log — Log-scaled coin amount outflows from whales → exchanges in the current hour.
7. whale_net_usd_log — Log-scaled net USD flow for the hour, where net = etow_usd − wtoe_usd (positive = accumulation, negative = distribution).
8. whale_net_usd_24h_log — Log-scaled rolling 24-hour net USD (sum of hourly net over the last 24 hours).
