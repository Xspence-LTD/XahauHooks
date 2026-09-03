# XahauHooks

Open-source [Xahau](https://xahau.network) Hooks by **Xspence-LTD**.

This repository contains two curated, testnet-validated hook collections: **Beneficiary** (dead-man's-switch / inheritance-style distribution) and **Savings** (automatic payment splits and time-released savings management).

> **License / disclaimer:** Open source. Use at your own risk. Always test on Xahau testnet before mainnet. Hooks move real funds — review source and install carefully.

## Security fixes (testnet-validated)

These sets include verified hardening:

- Fail-closed timer arming
- Stranger payment guard
- Early-release gate
- Contiguous packing
- `ttPAYMENT` compile fix
- **Emit encoding fix (2026-09-03):** `PREPARE_PAYMENT_SIMPLE` now writes exact txn length (`sizeout` / `txn_len`) so `etxn_details` / `etxn_fee_base` / `emit` no longer include trailing buffer bytes. Re-verified on Xahau Testnet NetworkID 21338 — see `Beneficiary/emit_fix_validation.md` (public hashes only). Timer fail-closed still holds.

## Beneficiary collection

Dead-man's-switch style hooks that distribute the hook account's XAH balance after inactivity (or via delegate invoke).

| Hook | Source | Summary |
|------|--------|---------|
| **Single Beneficiary (SBC)** | `Beneficiary/SingleBeneficiary/SingleBeneficiary/SingleBeneficiary.c` | After inactivity threshold, auto-forwards full balance to one beneficiary. |
| **Single Beneficiary Delegate (SBDC)** | `Beneficiary/SingleBeneficiary/Single Delegate/SingleBeneficiaryDelegate.c` | Designated delegate invokes to release full balance to the beneficiary. |
| **Single Beneficiary Threshold (SBTC)** | `Beneficiary/SingleBeneficiary/Single Threshold/SingleBeneficiaryThreshold.c` | After threshold, any non-owner incoming tx triggers forward to the beneficiary. |
| **Multi Beneficiary (MBC)** | `Beneficiary/MultipleBeneficiary/MultBeneficiarys/MultBeneficiarys.c` | Multi-beneficiary dead man's switch; delegate invoke distributes to all configured share recipients. |
| **Multi Beneficiary Delegate (MBDC)** | `Beneficiary/MultipleBeneficiary/Multi Delegate/MultiBeneficiaryDelegate.c` | Delegate invokes to distribute full balance across configured beneficiaries. |
| **Multi Beneficiary Threshold (MBTC)** | `Beneficiary/MultipleBeneficiary/Multi Threshold/MultiBeneficiaryThreshold.c` | After threshold, any non-owner incoming tx triggers multi-beneficiary distribution. |

Prebuilt `.wasm` artifacts sit beside each hook's `.c` source (including `SingleBeneficiaryDelegate`). Verified Xahau Testnet proof hashes are documented in each hook README (Single Delegate is documented as out-of-scope with no invented hashes).

## Savings collection

| Hook | Source | Summary |
|------|--------|---------|
| **Savings Manager (SSM)** | `Savings/Savings Manager/SavingsManager.c` | Time-released, parent-managed savings with spending limits and discipline rules. |
| **Savings Hook / IPS** | `Savings/Savings Hook/SavingsHook.c` | On incoming payments, forwards configurable percentages to up to 3 savings accounts (keeps ≥1% for fees). |

## Layout

```
Beneficiary/     # Single + Multi categories; each hook in a folder named after the hook
Savings/         # Savings Manager + Savings Hook sources, READMEs, and .wasm
```

Layout is folder-per-hook: each hook lives in a folder named after the hook (matching siblings such as `Single Threshold` / `Savings Hook`). Category folders under Beneficiary list their variants. Each hook folder includes its `.c`, colocated `.wasm` when present, and install / usage README (with verified testnet hashes where applicable).

## Org

Maintained by [Xspence-LTD](https://github.com/Xspence-LTD).

## Independent review

Independent review by [Kairo Vault Technologies GK](https://kairovault.com).

## Build headers

Compile these hooks against the headers in `headers/` in this repository. They contain the exact-length `PREPARE_PAYMENT_SIMPLE` / `PREPARE_PAYMENT_SIMPLE_TRUSTLINE` macros (`sizeout`), which the hook sources rely on when calling `emit(..., txn, txn_len)`.

Live emit proofs: `emit_fix_validation.md`.
