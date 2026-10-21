# Evidence ledger

**Ledger initialized 2026-10-21.** Stable IDs refer to claims, not experiment attempts. Keep the exact readings even when interpretations change.

**Provenance key:** `BR` = user-provided Clanker/Blitzcore experiment reports in the research conversation (raw capture not yet attached here); `MR` = [Morrowmake GA100 commit](https://github.com/Morrowmake/cmpunlocker/commit/19254b5ce80f801af7c7516211846b889cac359c); `CS` = [third-party CMP fuse survey](https://github.com/Consensus-Protocol/cmp170hx/blob/main/docs/hardware/fuses-and-otp.md); `NH` = [NVIDIA public driver/docs](https://github.com/NVIDIA/open-gpu-kernel-modules). **Externally documented is not equivalent to reproduced on Blitzcore.**

| ID | Layer | Finding / precise claim | Status | Source | Follow-up |
| --- | --- | --- | --- | --- | --- |
| AA-001 | L6/L7 | Blitzcore recovered to HiveOS 615.71.09; two GPUs healthy, no Xids/failed services | Reported observation | BR, 2026-10-21 | Archive boot and Xid log |
| AA-002 | L4/L6 | GA102 and GA104 report SS0=`0x88888888`, SS1=`0x8` and pass same-boot Tensor correctness | Reported observation | BR | Archive test executable/output |
| AA-003 | L6 | Prior matched-clock tests found ~1.98× FP16/BF16 and ~1.96× TF32 throughput | Reported observation | BR, earlier trials | Attach benchmark baselines and clocks |
| AA-004 | L0/L5/L6 | GA102 reports 68 active SMs; GA104 reports 40 | Reported observation | BR | Preserve `nvidia-smi` / CUDA topology evidence |
| AA-005 | L1/L4 | GA102 OPT_GPC_DISABLE previously read `0x40`, identifying GPC6 as disabled under current interpretation | Reported observation + interpretation | BR | Identify physical versus effective mask semantics |
| AA-006 | L4 | GPC0/TPC0 and GPC1/TPC0 identified as disabled in earlier GA102 mask investigations | Reported observation + interpretation | BR | Attach original per-GPC register captures |
| AA-007 | L3 | Cold-entry GA102 experiment opened RECONFIG PLM | Reported observation | BR | Attach transaction log |
| AA-008 | L4 | GPC0 `RECONFIG_OVERRIDE` at `0x820A40` changed `0 → 1` | Reported observation | BR | Verify against raw logs and exact readback stage |
| AA-009 | L4 | GPC0 STATUS at `0x820C38` remained `1 → 1`, including through SEC2 reset | Reported observation | BR | Isolate which contributing source or latch |
| AA-010 | L4 | Candidate OPT0 remained `1 → 1`; its GA102 register identity/semantics are not independently established | Reported observation + unresolved identity | BR | Confirm address and signal mapping from independent evidence |
| AA-011 | L4 | GPC0 CTRL0 remained `0 → 0` in the same trial | Reported observation | BR | Check CTRL register identity and applicability |
| AA-012 | L2/L3 | Trial second Booter pass timed out opening FEAT PLM; CPUCTL reported `0x0` on second entry vs `0x10` on first | Reported observation | BR | Explain changed SEC2 state; no repeat without review |
| AA-013 | L5/L6 | Failed SM trial did **not** reach FEAT completion, FLR, driver topology readback, or CUDA `%smid` validation | Reported observation | BR | No claim of 70 SMs |
| AA-014 | L4/L6 | Upstream CMP 170HX 8GB device reportedly recovered 70→74 SMs through RECONFIG override, with `%smid` proof | Externally documented, GA100 only | MR | Compare mask origins; do not generalize directly |
| AA-015 | L4 | Formula `STATUS = DISABLE | DEFECTIVE | CTRL | (RECONFIG & ~OVERRIDE)` is published for GA100 research context | Externally documented; GA102 applicability unknown | MR | Test whether GA102 implements equivalent live relation |
| AA-016 | L1/L4 | A third-party cross-card survey discusses differences in software-override enable state across Ampere | Externally documented, not locally reproduced | CS | Capture GA102-specific raw read with version and stage |
| AA-017 | L1/L4 | Address `0x820760` has **no verified GA102 definition in sources currently in hand** | Unknown identity | BR / public-header search | Seek independent named definition/capture before attributing semantics |
| AA-018 | L2/L3 | No GA102-specific full fuse manual or decrypted SEC2 Booter helper capture has been provided to this atlas | Evidence gap | BR / NH | Document acquisition if genuine source emerges |

## Active interpretation

Given AA-008 through AA-011, an independent disable/defect term is a plausible explanation **if** the GA100 relation applies to GA102 **and** STATUS is computed live. A previously latched STATUS or incorrectly identified OPT0 remains possible. No conclusion that the disabled TPC is physically defective or functional follows from this trial.

## Evidence capture template

For future entries: GPU PCI ID and die; VBIOS/firmware hash (if known); driver version; full boot stage; register address and width; before/write/readback; effective STATUS/topology; relevant logs; `FLR` / reset events; `%smid` and arithmetic correctness when claiming recovered SMs; experiment commit and log artifact URL. An unverified source should be called out explicitly.

Next: [Blitzcore case study](../devices/GA102-BLITZCORE.md) · [Open questions](../research/OPEN-QUESTIONS.md).