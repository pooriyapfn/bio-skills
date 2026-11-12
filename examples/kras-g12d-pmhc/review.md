# KRAS G12D pMHC worked example

## Reference structure audit

Run with MolDesk 0.0.9 on the public RCSB mmCIF downloads. The selectors use author IDs. [7OW6 download](https://files.rcsb.org/download/7OW6.cif) · [7OW5 download](https://files.rcsb.org/download/7OW5.cif) · [7OW4 download](https://files.rcsb.org/download/7OW4.cif)

```bash
moldesk analyze 7OW6.cif --json > 7OW6.analysis.json
moldesk analyze 7OW5.cif --json > 7OW5.analysis.json
moldesk analyze 7OW4.cif --json > 7OW4.analysis.json
moldesk inspect 7OW6.cif C:6 --json > 7OW6.position-6.json
moldesk inspect 7OW5.cif C:6 --json > 7OW5.position-6.json
moldesk view 7OW6.cif --highlight C:6 --output 7OW6-review.html
```

| Entry | Observed peptide | Position 6 | Coordinate context |
| --- | --- | --- | --- |
| 7OW6 | `VVVGADGVGK` | `C:6` ASP | Heavy chain A, β2-microglobulin B, peptide C, TCR D/E |
| 7OW5 | `VVVGAGGVGK` | `C:6` GLY | Heavy chain A, β2-microglobulin B, peptide C, TCR D/E |
| 7OW4 | `VVVGGVGK` in each peptide copy | Unobserved | Four TCR-free pMHC copies. Chains C/F/I/L have the full deposited G12D sequence with positions 5 and 6 unobserved. |

In 7OW5 and 7OW6, the deposited A and B sequences are 276 and 100 residues. The official assay sequences are 281 and 99 residues, respectively. Track the reference and assay constructs separately. [Proteinbase assay constructs](https://proteinbase.com/competitions/anthropic-adaptyv-2026/challenges/pmhc)

The two position-6 inspections returned neighbors in the co-crystallized TCR as well as the pMHC. The highlighted HTML therefore shows the deposited TCR-bound reference. All three analyses returned `NO_CONFIDENCE_SOURCE`, consistent with experimental structures that carry no predicted pLDDT. Binding and selectivity require separate assay evidence.

## Example candidate assessment

[Fictional assay fixture](assays.synthetic.csv) with hypothetical matched conditions and `KD` values in nM. Apply the [benchmark profile](../../skills/moldesk-binder-campaign/references/proteinbase-kras-g12d.md) to supplied calls.

| Candidate | Mutant `KD` | Wild-type control | Peptide-free control | Assessment |
| --- | ---: | --- | --- | --- |
| `demo-a` | 12 | No detectable binding | No detectable binding | Meets all three objectives in the fixture. |
| `demo-b` | 2 | `KD` 8 nM | No detectable binding | Fails the wild-type specificity criterion despite stronger mutant affinity. Wild-type-to-mutant `KD` ratio is 4. |
| `demo-c` | 6 | No detectable binding | Missing | Mutation-specific in the fixture. Peptide dependence remains unresolved. |
| `demo-d` | 3 | No detectable binding | Binding detected, `KD` unavailable | Fails the peptide-dependence criterion. The peptide-free-to-mutant ratio is unavailable. |

`demo-a` is the only candidate that clears both control criteria. To assess a real candidate against the archived challenge, supply comparable measurements for the mutant, wild-type and peptide-free constructs.
