# MolDesk commands and scores

Run commands in a campaign directory and retain the inputs, JSON outputs and run records. Use the installed release's `--help` and `moldesk describe <model-id> --json` for exact input formats, parameters, hardware, cloud eligibility and output IDs. [MolDesk documentation](https://moleculedesk.com/docs)

## Discover and inspect

| Task | Command or source | Read from the result |
| --- | --- | --- |
| Find structures and papers | [RCSB PDB search](https://www.rcsb.org/search), [PubMed](https://pubmed.ncbi.nlm.nih.gov/), [UniProt](https://www.uniprot.org/) | Verify accession, species, construct, chain roles and experimental or predicted provenance. |
| Check capabilities | `moldesk doctor --json`, `moldesk list --json`, `moldesk describe <model-id> --json` | Local compatibility, model I/O, parameters and `cloud.available`. A supported release still needs a successful quote for the specific cloud job. |
| Retrieve a known public structure | `moldesk fetch pdb:<id> --json` | Local path, original URL and hash. Search RCSB first when the PDB ID is unknown. |
| Audit chain mapping | `moldesk analyze <structure> --json` | `authId`, `labelId`, observed and full sequences, unobserved residues, `warnings[]`. Map assay roles to explicit chains. |
| Inspect a pose | `moldesk inspect <structure> <selector> --json`, `moldesk view <structure> --highlight <selector> --output <report.html>` | Nearby heavy atoms, distances and a reviewable overlay of highlighted residues on the structure. Selectors default to author IDs. |
| Compare poses | `moldesk compare <reference> <model> --json` | Alignment, RMSD and TM-score. Requires the managed US-align tool. Specify chains or multimer mode deliberately and examine the mapped region. |

## Model roles

`moldesk run <model-id> <input> --compute local --json` runs a model on the machine hosting the CLI. Use it only on a machine allowed by the user. For managed rental, use the Runpod path below. Pass only parameters supported by `describe`. On success, resolve artifacts from `record.outputs[]` by `id` and `path`. Keep the `run.json`, model revision, input hashes and candidate-to-sample mapping. Preserve failures and valid runs with zero accepted designs.

| Model ID | Input and role | Outputs to use |
| --- | --- | --- |
| `rfd4-proteina` | YAML or JSON condition spec with CIF inputs. Candidate generation. | `generated_structures`, resolved spec and summary. Treat the generated pose as the model's proposal. |
| `bindcraft2` | JSON campaign. Alternative candidate generator. | Trajectories, refolded structures and accepted structures with campaign metrics. Its built-in filters are part of the generation route. The current managed Runpod path is unavailable. |
| `boltz` | YAML or FASTA accepted by the CLI. Use YAML to specify multichain complexes for structure prediction. | `structures`, `confidence`, and matching `pae` when requested using `--param write_full_pae=true` on a supported runtime. Read chain-pair scores from the matched confidence JSON. |
| `opendde` | JSON co-folding job. Optional second structure predictor. | `structures`, `confidence` and `atom_confidence` where emitted. Read the fields present in the pinned version's output. |

Use the model's input documentation when creating a job file, then confirm that the installed MolDesk adapter accepts the needed options. [RFD4-Proteina](https://rfd4-proteina.org/) · [BindCraft2](https://github.com/PacesaLab/BindCraft2) · [Boltz](https://github.com/jwohlwend/boltz/blob/main/docs/prediction.md) · [OpenDDE](https://github.com/aurekaresearch/OpenDDE)

AlphaFold 3 is another option for reassessment when independently available on an allowed resource. Use its documented inputs and native confidence files. [AlphaFold 3 outputs](https://github.com/google-deepmind/alphafold3/blob/main/docs/output.md)

## Interpret evidence

| Score or observation | Meaning and use |
| --- | --- |
| pLDDT | Local confidence in a predicted structure. Review the binder and interface residues. Experimental B-factors have a different meaning. |
| Chain-pair ipTM | Confidence in the relative placement of two chains. Boltz reports `pair_chains_iptm`. Resolve matrix indices to actual chains, then compare the same chain pair across comparable conditions. |
| PAE | Predicted alignment error in Å. Lower values in the relevant inter-chain block support more confident relative placement. Preserve direction and match the PAE file to the structure and sample. |
| Contacts, clashes and pose agreement | Inspect where the binder sits and whether that pose repeats across samples or predictors. Check the interface itself when a whole-complex score is high. |
| ipSAE | Optional interface confidence from PAE and geometry. `moldesk score interface` supports a complete two-protein-chain Boltz mmCIF only. Supply matching `--pae` and `--confidence` sidecars plus explicit `--target-chain` and `--binder-chain`. The managed scorer is a CPU tool. |
| Measured `KD` and assay calls | Evidence of binding and selectivity under the assay conditions. Retain measurement units and distinguish detected without fitted `KD`, no detectable binding, inconclusive and missing. Boltz's built-in affinity head covers small molecule–protein affinity. |

Use matched target/control settings and inspect score spread across samples where available. Use model-native scales and evaluate agreement in the pose and control observations across models. A failed control prediction leaves its binding status unknown. [Boltz-2 prediction outputs](https://github.com/jwohlwend/boltz/blob/main/docs/prediction.md)

## Runpod jobs

1. Check `moldesk cloud status --json`. If credentials are missing, the user connects the account with `moldesk cloud connect runpod` in their terminal. Credentials stay in the credential store.
2. Quote the exact job before provisioning. Replace the placeholders with the selected model and input, the portion of the campaign budget allocated to this job in USD, and its lifetime in seconds.

   ```bash
   moldesk run <model-id> <input> --compute runpod --cache-policy ephemeral \
     --max-budget <job-usd> --max-lifetime <seconds> --dry-run --json
   ```

3. Compare the quote to the remaining campaign budget and verify the upload manifest. `--max-budget` guards the job's compute quote. Runpod bills actual usage, and storage or transfer can be separate or unknown. Use ephemeral cache unless the user wants a retained volume.
4. Use the quoted settings for execution. Supply `--yes` for the accepted quote and `--accept-lifetime-risk` for the acknowledged possibility of charges after disconnect. Add `--accept-file-upload` or `--accept-storage-charges` only when required and covered by the user's authorization. If material terms change or required consent is missing, show the exact quote or upload list and ask for the missing consent.
5. Record the result and verify cleanup with `moldesk cloud resources --json`. If a run needs transfer or cleanup recovery, use `moldesk cloud recover <run-id>` before allocating more compute. Record estimated and confirmed charges separately. [MolDesk Runpod guide](https://moleculedesk.com/docs/guides/runpod-cloud-execution)
