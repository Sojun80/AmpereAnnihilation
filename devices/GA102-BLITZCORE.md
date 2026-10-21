# Blitzcore: GA102 / GA104 case study

**Snapshot:** October 21, 2026. **Platform:** remote HiveOS, NVIDIA 615.71.09. **GPUs:** RTX 3080 GA102 (68 reported active SMs) and RTX 3070 Mobile GA104 (40). Existing production compute-selector overrides: `SS0=0x88888888`, `SS1=0x8`, passing same-boot Tensor correctness checks. Recovery boot entry and HiveOS reported healthy; no Xids. These readings come from Clanker reports relayed in conversation; attach actual logs when available.

## Goal

Identify whether GA102 TPCs excluded by floorsweeping can be made operational, independently of the already demonstrated Tensor Core selector speedup. No additional GA102 SM has yet been demonstrated.

## Prior topology observations

- `OPT_GPC_DISABLE=0x40`: interpreted as GPC6 disabled.
- GPC0/TPC0 and GPC1/TPC0 identified as excluded.
- Earlier pre-driver selector overrides preceded GSP topology discovery and survived FLR; late GSP-discovery timing alone does not explain the unchanged 68 SMs.
- Demonstrated privilege access occurs after GFW/FWSEC; no equivalent earlier path identified.

## Cold-entry GPC0 trial

| Event | Result |
| --- | --- |
| First staged boot | Missing packaged CUDA validator source skipped GA102; GA104 and normal Linux startup worked |
| Corrected cold entry | RECONFIG PLM opened |
| GPC0 `RECONFIG_OVERRIDE` `0x820A40` | `0 → 1` with successful readback |
| GPC0 STATUS `0x820C38` | `1 → 1` immediately and through SEC2 reset |
| Candidate OPT0 | `1 → 1`, *address/identity not yet independently established* |
| CTRL0 | `0 → 0` |
| Second Booter pass | FEAT PLM open timed out; SEC2 CPUCTL entry `0x0` versus first pass `0x10` |
| FLR, driver topology, new-SM validation | **Not reached** |
| Recovery | Known-good 615 boot restored; both selector states and CUDA correctness confirmed |

## Mechanism: what follows, what does not

[Morrowmake's CMP 170HX experiment](https://github.com/Morrowmake/cmpunlocker/commit/19254b5ce80f801af7c7516211846b889cac359c) documents a GA100 STATUS expression:

`STATUS = DISABLE | DEFECTIVE | CTRL | (RECONFIG & ~OVERRIDE)`

Its measured 70→74 SM outcome concerns **RECONFIG-reserved TPCs** on GA100, with execution proof. On GA102 the override write was accepted but STATUS did not move. This is compatible with an independent DISABLE/DEFECTIVE term, a stale/latched STATUS, or differences in register semantics. It does **not** establish which is responsible.

## Evidence requests

Import saved readback logs, relevant commit hashes, SEC2/FWSEC/GFW versions, raw register captures, and the existing local `research/early-sm/STATUS-FOLLOWUP.md` when made available. Do not pretend this GitHub repository currently contains those private-machine artifacts.

Related: [register map](../registers/REGISTER-MAP.md), [evidence ledger](../evidence/LEDGER.md), [questions](../research/OPEN-QUESTIONS.md).
