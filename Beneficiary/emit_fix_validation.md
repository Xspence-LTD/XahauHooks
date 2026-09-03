# HandyHooks PREPARE_PAYMENT_SIMPLE emit-fix LIVE validation
Generated: 2026-09-03 13:06:37 UTC / 14:06:37 BST (Europe/London)
Network: Xahau Testnet NetworkID 21338 (`https://xahau-test.net`)
Namespace tag: `emitfix1`

## Macro change
- `PREPARE_PAYMENT_SIMPLE` / `PREPARE_PAYMENT_SIMPLE_TRUSTLINE` now take `sizeout`.
- `etxn_details` uses remaining buffer; advance by `edlen`; fee over exact `sizeout`.
- Callers `emit(..., txn, txn_len)` â€” no trailing bytes.
- Timer fail-closed (`Timer not armed`) kept.

## PASS/FAIL matrix

| File | Step | Result | Engine | Hash | HookReturn |
|------|------|--------|--------|------|------------|
| `SingleBeneficiary` | SetHook THRESHOLD=1 | **PASS** | `tesSUCCESS` | `D46B60C233F6656FD3779FCDF2FFEFFC3E92F723DBD3EF23A06933EDDADADD67` | `` |
| `SingleBeneficiary` | Unarmed SEND fail-closed | **PASS** | `tecHOOK_REJECTED` | `8B672B5C7DF0447AD2E8CDABC49898AC523C43C1B3FE6CC7DB0A0AC4932A14EE` | `SBC:: Error :: Timer not armed - owner must make one outgoing payment` |
| `SingleBeneficiary` | Owner arm Payment | **PASS** | `tesSUCCESS` | `840D73844556286B813B12A749FB93100F201206BFB0A8BD95C65422EE90BFF7` | `SBC:: Success :: Outgoing payment from hook account accepted, timer reset` |
| `SingleBeneficiary` | SEND after threshold â€” emit success | **PASS** | `tesSUCCESS` | `C3E25F08196E4D34F067224982FA27CA3EA4B2746E30147E7DAE9577C927B525` | `SBC:: Success :: Balance sent to beneficiary successfully (threshold exceeded)` |
| `MultiBeneficiaryDelegate` | SetHook DELEGATE | **PASS** | `tesSUCCESS` | `4BDBA865C1F0D02B28693DE2FCEDBF8B6A9CF43FC7B50CDB92995807D7E5533E` | `` |
| `MultiBeneficiaryDelegate` | BA1/BP1 config | **PASS** | `tesSUCCESS` | `305F998A97C93DB6F1C89911F59BDDA6788624A237D3EC0C34B91F30DEB878DE` | `MBDC:: Success :: BA1 and BP1 configured` |
| `MultiBeneficiaryDelegate` | Delegate SEND emit success | **PASS** | `tesSUCCESS` | `6B58B9B264A89DFB57BDCA5C31EA61C2BF154A45B44378122745C9C16125EB10` | `MBDC:: Success :: Balance distributed to beneficiaries` |
| `SavingsHook` | SetHook | **PASS** | `tesSUCCESS` | `7A980F72B68D3C169C642F575FB481956DF4F3153101E653230D336E87359D28` | `` |
| `SavingsHook` | SA1 config | **PASS** | `tesSUCCESS` | `B77E0CE96E617B0AACEFAD0EAAB285666F5C910D619F331C59EF6613126A0C59` | `IPS:: Success :: SA1 configured` |
| `SavingsHook` | SP1 config | **PASS** | `tesSUCCESS` | `D077C017EB9E1B5DC2F68F951FA3BE59A6D6D353A2F5830AAB320C21E15AD007` | `IPS:: Success :: SP1 configured` |
| `SavingsHook` | Incoming payment (savings emit path) | **PASS** | `tesSUCCESS` | `1BD68721077ED80725845EBB3B411FE7ADA6E20E6B68BACA01BC4EBFAF6CE8BA` | `IPS:: Success :: Payment percentage forwarded to savings accounts` |

## Overall: **PASS**

Do not invent hashes. Seeds omitted.
