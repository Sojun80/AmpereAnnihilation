# Register and permission map

This is a **working research map, not a GA102 register manual**. `0x...` addresses are not presumed portable between GA100 and GA102. Capture source, GPU, access stage, and raw values with every new finding.

| Register / concept | Address | Device context | Observed state / source | Confidence |
| --- | --- | --- | --- | --- |
| `RECONFIG_OVERRIDE_OPT_TPC_GPC0` | `0x820A40` | GA102 cold trial; GA100 upstream reference | GA102 override `0→1`, readback succeeded | Measured on GA102, reported |
| `STATUS_OPT_TPC_GPC0` | `0x820C38` | GA102 cold trial; GA100 upstream reference | GA102 STATUS `1→1`, including SEC2 reset | Measured on GA102, reported |
| `RECONFIG_PLM` | `0x8200D8` | GA102 / GA100 | GA102 PLM opened in cold trial | Measured on GA102, reported |
| `OPT_GPC_DISABLE` | `0x820350` | GA102 / GA100 | Earlier GA102 `0x40` interpreted as GPC6 disabled | Earlier GA102 observation |
| `OPT_PLM` | `0x8200FC` | GA100 code | Listed in upstream SEC2 patch | Externally documented, GA102 status unresolved |
| `SS0` | `0x82381C` | GA102/GA104 production | `0x88888888` after unlock | Measured on both, reported |
| `SS1` | `0x823820` | GA102/GA104 production | `0x8` after unlock | Measured on both, reported |
| Candidate OPT0 | **Unknown / not independently verified** | GA102 | `1→1`; potential disable source | Identity unresolved |
| CTRL0 | **Not verified here** | GA102 | `0→0` | Raw mapping needed |
| Unknown fuse candidate | `0x820760` | GA102 hypothesis only | Public headers examined by Clanker do not define it; no independently verified identity | **Unknown — do not label as a specific fuse bit** |

## Access dimensions

For each address, distinguish: readable after driver init; writable with an opened PLM; writable before GFW/FWSEC; readback persists across reset; hardware STATUS changes; GSP topology changes; and actual SM executes. A successful one-step write is not evidence of the later stages.

## Source comparison

- [Morrowmake 170HX commit](https://github.com/Morrowmake/cmpunlocker/commit/19254b5ce80f801af7c7516211846b889cac359c) describes a GA100 RECONFIG reservation override and 70→74 SM test.
- [NVIDIA open GPU modules](https://github.com/NVIDIA/open-gpu-kernel-modules) supplies source and headers, not necessarily the private GA102 register definitions.
- A decrypted/plaintext SEC2 Booter helper capture and GA102-specific fuse register manual have **not** been supplied. The absence limits attribution; it is not proof the address is unused.

## Rule for new entries

Record exact silicon/PCI device ID, register width/address, firmware state and permission owner, original value, attempted write, subsequent readback, effective STATUS, reset boundary, topology, runtime proof, and linked raw evidence.
