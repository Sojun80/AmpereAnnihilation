# Research questions and next observations

This page is a backlog of **unknowns**, not an instruction to perform dangerous or irreversible experiments.

## The GPC0/TPC0 STATUS bit

**Observed:** Override `0→1`, STATUS `1→1`, candidate OPT0 `1`, CTRL0 `0`.

**Question:** Which component keeps the effective status asserted? The GA100 research formula implies another DISABLE/DEFECTIVE source **only if** equivalent GA102 semantics exist and STATUS updates live.

**Competing models:**
1. Independent effective-disable source (possibly fuse-derived).
2. Actual defective-unit source.
3. STATUS or topology latched before the override.
4. A GA102-specific STATUS formula or misidentified register.

**Useful evidence:** independently named GA102 register definitions, read-only paired captures at different boot boundaries, and confirmed per-source STATUS relationships.

## The undefined `0x820760`

**Current state:** No independently verified public GA102 identity. No GA102-specific fuse manual or decrypted SEC2 Booter helper capture is available in our shared evidence. Record any source with revision, provenance, raw decode and relevant chip before assigning semantics.

**Falsifier for proposed identity:** authoritative symbol or trace assigning another meaning, or repeatable observations incompatible with the proposed source.

## Boot timing and privilege

The successful GA102 privilege path was reached **after** GFW/FWSEC. Is configuration consumed before that point, and by which processor? The failed second Booter pass saw CPUCTL `0x0` instead of first-pass `0x10`, then timed out opening FEAT PLM. Its cause remains unknown. Avoid treating this firmware failure as evidence about TPC functionality.

## GA100 versus GA102

GA100 CMP 170HX RECONFIG override recovered 70→74 SMs from reserved TPCs ([source](https://github.com/Morrowmake/cmpunlocker/commit/19254b5ce80f801af7c7516211846b889cac359c)). Do GA102 missing TPCs share the same reservation path, or do they depend on OPT, defects, or other effective fuses? The current GPC0 test suggests the published GA100 maneuver alone is insufficient, not that recovering GA102 units is impossible.

Permanent OTP/RIR fuse programming and volatile privilege-controlled overrides belong to **different branches of this research**. External claims of larger CMP unlocks require original device logs and distinct-SM execution evidence.

## Evidence import queue

- The private `research/early-sm/README.md` and `STATUS-FOLLOWUP.md` from the development checkout.
- Cold-entry console/SEC2 logs with full stage sequence and GPU identifiers.
- Known-good 615 verification output and commit hashes.
- GA102 per-GPC OPT/DISABLE/DEFECTIVE/CTRL register captures and provenance.
- Source-backed decode for `0x820760`, if one becomes available.

Link every imported artifact to a stable entry in [the ledger](../evidence/LEDGER.md). Always retain the original observation separately from a later interpretation.
