# Proteinbase KRAS G12D pMHC benchmark

Challenge 05 of the 2026 Anthropic × Adaptyv competition. Submissions closed November 9, 2026 at 23:59 Anywhere on Earth. Use it as a retrospective benchmark. [Official challenge](https://proteinbase.com/competitions/anthropic-adaptyv-2026/challenges/pmhc)

| Role | Assay construct | Structural reference |
| --- | --- | --- |
| On target | HLA-A*11:01 + β2-microglobulin + KRAS G12D peptide `VVVGADGVGK` | TCR-bound crystal [7OW6](https://www.rcsb.org/structure/7OW6). TCR-free crystal [7OW4](https://www.rcsb.org/structure/7OW4) has unobserved coordinates at peptide positions 5 and 6. |
| Mutation control | Same HLA and β2-microglobulin + wild-type peptide `VVVGAGGVGK` | TCR-bound crystal [7OW5](https://www.rcsb.org/structure/7OW5). |
| Peptide-dependence control | Same HLA and β2-microglobulin, peptide-free | Proteinbase displays a derived model with the peptide and TCR removed. The peptide-free assay is separate evidence. |

KRAS residue 12 maps to peptide position 6. In 7OW6 and 7OW5, chains A/B/C are heavy chain, β2-microglobulin and peptide. Chains D/E are the co-crystallized TCR. The competition viewer removes the TCR from these bound structures, so its coordinates retain that structural context. Record any differences between the deposited sequences and the official assay constructs. [Challenge target information](https://proteinbase.com/competitions/anthropic-adaptyv-2026/challenges/pmhc)

For computational screening, track the mutant, wild-type and peptide-free conditions separately. Use the verified chain map to distinguish binder–peptide and binder–HLA predictions. Keep the missing peptide coordinates in 7OW4 and the TCR-bound context of 7OW5 and 7OW6 visible when interpreting predictions. The class I pMHC–binder complex is outside the two-chain input scope of `moldesk score interface`. Use native prediction outputs and structure inspection for its computational evidence.

## Official assay ranking

1. **Mutation specificity.** A measurable mutant `KD` and no detectable wild-type binding. For candidates that bind both and have both `KD` values, a larger wild-type-to-mutant `KD` ratio ranks higher within that group.
2. **Peptide dependence.** A measurable mutant `KD` and no detectable binding to peptide-free MHC. For candidates that bind both and have both `KD` values, a larger peptide-free-to-mutant `KD` ratio ranks higher within that group.
3. **Affinity.** Lower mutant `KD` wins the affinity comparison after the specificity criteria. A winner must clear all three objectives.

The organizers classify binding from a fittable `KD` or, for an unfittable curve, an association shift above their stated negative-control threshold. Use the reported assay call or consult the exact rule. Leave the ratio unavailable when either `KD` is absent. [Official ranking and detection rules](https://proteinbase.com/competitions/anthropic-adaptyv-2026/challenges/pmhc)
