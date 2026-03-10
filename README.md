# sDOLA LlamaLend Donation Attack -- PoC

> **Educational Purpose Only** -- This PoC is created for security research and education
> purposes only. It is a simplified simulation, not a fork replay against mainnet.

## Overview
- **Date:** 2026-03-02
- **Loss:** ~$240,000 (6.74 WETH + 227,325 DOLA)
- **Chain:** Ethereum
- **Category:** Price Manipulation / Donation Attack
- **Exploit Block:** #24566937
- **Fork Block:** #24566936

## Vulnerability
An attacker used ~$30M in flash loans to donate DOLA directly into the sDOLA vault,
inflating the sDOLA exchange rate from ~1.188 to ~1.358 DOLA per sDOLA. The LlamaLend
sDOLA/crvUSD pool used an oracle vulnerable to this donation-style manipulation.
The inflated rate triggered liquidation of 27 users' positions, allowing the attacker
to act as liquidator and collect rewards before repaying the flash loan.

Inverse Finance's own contracts were not affected -- the vulnerability was in the
LlamaLend pool's oracle configuration for sDOLA-collateralized positions.

## Reproduction
```bash
git clone https://github.com/NomosLabs-Security/poc-inverse-finance-dola-2026
cd poc-inverse-finance-dola-2026
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Key Contracts
- `MocksDOLAVault`: Simulates sDOLA vault with exchange rate vulnerable to donations
- `VulnerableLendingPool`: LlamaLend-style pool using raw vault exchange rate as oracle
- `FixedLendingPool`: Uses TWAP-smoothed oracle resistant to donation attacks

## References
- [YAM Analysis](https://x.com/yieldsandmore/status/2028368378457362629)
- [Attack TX](https://etherscan.io/tx/0xb93506af8f1a39f6a31e2d34f5f6a262c2799fef6e338640f42ab8737ed3d8a4)

## License

MIT -- For educational use only.
