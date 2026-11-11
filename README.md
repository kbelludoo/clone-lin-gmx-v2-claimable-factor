# clone-lin-gmx-v2-claimable-factor

Experimental LIN clone of GMX V2 `_getClaimableFactor` (uint64-scale: max of time-key and account factors, delay-default to FLOAT, then reduction floor at 0).

- **Upstream:** [gmx-io/gmx-synthetics](https://github.com/gmx-io/gmx-synthetics) `contracts/market/MarketUtils.sol` commit `a85ea3491c19c93bb4b5a002d9b358fb769b7849` (GPL-2.0-or-later after BUSL Change Date 2026-08-31).
- **LIN kernel:** [`src/lin_gmx_v2_claimable_factor.lin`](src/lin_gmx_v2_claimable_factor.lin)
- **C11 oracle copy:** [`test/oracles/gmx_v2_claimable_factor_c11.c`](test/oracles/gmx_v2_claimable_factor_c11.c)
- **Proofs (lin-open only):** [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/gmx_v2_claimable_factor/` and `python3 test/prove_gmx_v2_claimable_factor_external.py`
- **Class:** EXPERIMENTAL. Not Arbitrum GMX V2 uint256. Not FLOAT 1e30. Not a production replacement. Not `applyDeltaToCollateralSum` (always-max unsigned sum) and not a working i64 port of `getPendingPositionImpactPoolDistributionAmount` (rate*duration clip).

Named goldens (uint64-scale, divisor=3600 delay=86400 timeKey=1000 unless noted): max(time 4e5, account 6e5) → **600000** vs min **400000** vs time-only **400000**; both-zero after delay+1 → **1000000** vs no-default **0** vs exact delay **0** vs `>= delay` **1000000**; 8e5-3e5 → **500000** vs ignore **800000** vs add **1100000**; over-reduction → **0** vs signed **-200000**; reduction blocks delay-default **0** vs default-then-reduce **800000**; factor 2e6 **ok=0** vs cap **1000000**; now < timeKey*divisor **ok=0**; 7e9*2e9 **ok=0** vs naive i64 wrap **-4446744073709551616**; amount 1e8 * 5e5 / 1e6 claimed 0 → **50000000**, claimed 1e7 → **40000000**; impact-pool rate*duration **500000** vs delay-default **1000000**.
