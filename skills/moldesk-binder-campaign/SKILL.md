---
name: moldesk-binder-campaign
description: Plan or run a MoleculeDesk binder design and screening campaign for a noninfectious human oncology target. Use when given a target, design brief, or competition link and asked to generate candidates, screen a candidate pool, reassess binding poses, or deliver a ranked shortlist under a compute budget and resource constraints. Covers the archived Proteinbase KRAS G12D pMHC challenge.
---

# MolDesk Binder Campaign

Own the campaign decisions. Resolve the brief, select compatible tools, decide which evidence warrants more compute, and produce a shortlist with reasons. Choose the model mix, sample counts, and ranking rules for the particular study and budget. Use this workflow for noninfectious human oncology targets.

## Required inputs

| Input | What to establish |
| --- | --- |
| Issue | A target, brief, or challenge URL that identifies the intended target and objective. Read the brief and recover constructs, controls, and rules from its sources. |
| Budget | Total campaign spend limit and currency, including whether paid compute is authorized. Zero is valid. Check what the provider quote covers. |
| Resources | Allowed execution locations, such as local GPU, an existing remote machine, or rented GPU only, plus any provider or data-transfer constraints. |

Read the user's request, provided files and linked brief before asking. If essential information is missing or ambiguous, ask for just the missing items together. For example, if the target is known but compute is unspecified, ask for the total spending limit and where model jobs may run. A planning-only request can proceed with budget and compute choices marked pending. For a campaign that will run models, resolve those choices before starting jobs. Use the requested shortlist size, or up to 10 when unspecified.

## Tools and decisions

- Read [MolDesk commands and scores](references/moldesk-tools.md) for model I/O, inspection and scoring commands, and the Runpod quote and cleanup workflow. Check the installed CLI before choosing a model or parameter.
- For the archived KRAS G12D pMHC challenge, read [the Proteinbase profile](references/proteinbase-kras-g12d.md). For another study, derive its own constructs, controls and success criteria from the brief. Keep the references and assay constructs distinct.
- Treat resource restrictions as binding. For rented-GPU-only inference, explicitly route every model job to the authorized remote compute. If the requested route cannot run a model, choose a compatible alternative or report the gap. Use local inspection or scoring only within the user's resource constraints.
- Keep a campaign-wide ledger of paid runs, quotes, spend and unresolved charges. Assign each run a portion of the remaining budget before quoting it. Scale the work only when the available evidence and remaining resources justify it. Honor the CLI's quote and consent requirements, using any authorization already given for the applicable quote and uploads.
- Compare like-for-like prediction evidence within a model and inspect the actual interfaces. Choose how to weigh repeatability, control evidence, independent reassessment and diversity. Record your decision criteria and changes. Mark missing results as unknown and return fewer candidates when the evidence supports fewer.

## Deliverable

Return the target and control map, model versions, resource use and cost evidence, and the ranked shortlist. Link each candidate to its structures, scores, control outcomes, and selection reason. Record exclusions and failures in the campaign record. Label computational predictions as hypotheses for testing and report measured binding separately when assay data exists.
