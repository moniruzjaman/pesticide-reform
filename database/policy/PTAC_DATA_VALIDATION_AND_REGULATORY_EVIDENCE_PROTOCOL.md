# PTAC Submission: Data Validation and Regulatory Evidence Protocol

**Status:** Draft control document for technical review  
**Purpose:** Prevent untraceable statistics, inferred hazard classifications, and unsupported legal conclusions from being presented as verified facts in a PTAC submission.  
**Scope:** Pesticide registration/product dataset, active-ingredient and Mode-of-Action (MoA) mapping, HHP screening, and the proposed label-information reform.

> **Important:** This protocol is a quality-control framework. It does not certify that the underlying workbook or every record has passed independent verification. A repository-reported count or an Excel audit formula is not, by itself, proof that a value matches the current official DAE register or a primary scientific/regulatory source.

## 1. Evidence-status labels

Use one of these labels for every material claim in the dossier, dashboard, annex, and presentation:

- **VERIFIED — PRIMARY SOURCE:** Checked against a named primary source, with document/version/date, URL or repository path, access date, and the relevant page/table/record.
- **REPRODUCIBLY DERIVED:** Calculated from a versioned dataset using a documented rule; input file hash, code/formula, exclusions, and output are recorded.
- **CORROBORATED:** Supported by two or more credible sources but not yet checked against the responsible primary authority.
- **PROVISIONAL — REVIEW REQUIRED:** Plausible but not fully checked, including ambiguous names, missing identifiers, conflicting sources, or unverified mappings.
- **PROPOSAL / POLICY OPTION:** A recommended future action, not a statement of existing law or current government policy.
- **NOT VERIFIED:** Evidence is absent or insufficient. Do not present as an established fact.

Do not use “validated”, “official”, “current”, “banned”, “approved”, or “HHP” as unqualified labels unless the evidence supports the exact meaning and scope.

## 2. Dataset identity and reproducibility

The repository's database README reports a derived workbook containing 5,452 product rows, 7,250 recommendation rows, 478 distinct category/common-name pairs, 879 holders, 96 crops, and 657 pest/disease/weed entries. These are **repository-reported figures** until the exact files and formulas are re-run and compared with the current official register.

Before using any count in a formal submission, record:

1. Dataset title and exact source authority/document.
2. Source publication/version/date and retrieval date.
3. Repository commit SHA and branch.
4. File name, byte size, and SHA-256 hash.
5. Workbook sheet/table and row inclusion/exclusion rules.
6. Whether a row represents a product, registration, active ingredient, recommendation, or formulation.
7. Duplicate logic and the number of records removed or retained.
8. Script/formula version, execution date, and audit output.
9. Known gaps and unresolved discrepancies.

A count from a product register must not be compared with a count of active ingredients or recommendation lines as though they were the same unit.

## 3. Minimum machine-checkable data quality gates

The release is **not submission-ready for quantitative claims** until the following checks are run on the exact release files and their results are retained in an audit report.

| Gate | Required test | Pass condition |
|---|---|---|
| File integrity | SHA-256 of source files; compare source and mirrored copies | Hashes match the declared manifest or differences are explained |
| Row identity | Product primary key and registration number checks | No unexplained duplicate IDs; duplicates are classified |
| Completeness | Missing registration number, product name, holder, active ingredient, concentration, formulation, category | Counts and percentages disclosed; critical missingness reviewed |
| Normalisation | Trim whitespace; case/Unicode normalisation; aliases for names and units | Transformations logged; raw values preserved |
| Registration status | Current, expired, cancelled, suspended, pending, unknown | Status only assigned from dated authoritative evidence; unknown remains unknown |
| Product-to-ingredient link | Ingredient(s), concentration, salt/ester where relevant, formulation | Ambiguous links quarantined; combination products retain every component |
| MoA mapping | Compare active ingredient and target use against current IRAC/FRAC/HRAC/WSSA classification as applicable | Source edition/date captured; unresolved mappings remain provisional |
| HHP screening | Apply a declared WHO/FAO criterion to the correct substance and source edition | Each criterion is individually evidenced; product-level conclusions are qualified |
| Referential integrity | Product → ingredient → MoA / recommendation links | No orphaned references; exceptions listed |
| Cross-repository sync | Compare exact source files between this dossier and the operational PWA repository | Hash equality or a dated discrepancy log |
| Output reconciliation | Recompute totals independently from raw rows | Workbook, CSV, dashboard, and report totals reconcile |
| Regression | Re-run checks on each data update | No unexplained count drift; change report attached |

An Excel sheet reporting “PASS” is evidence only for the checks actually encoded in that sheet. It does not establish that source data are accurate, up to date, legally authoritative, or scientifically interpreted correctly.

## 4. Hazard classification and HHP safeguards

HHP screening is a **hazard-identification exercise**, not a complete exposure or risk assessment and not an automatic determination that every product containing a flagged active ingredient is unlawful or should immediately be withdrawn.

For every flagged active ingredient, retain:

- the exact WHO/FAO criterion and edition used;
- the primary evidence source and the specific criterion it supports;
- whether the match is direct, inferred, or ambiguous;
- the substance identity resolution, including salts/isomers where relevant;
- any conflicting or outdated classification;
- the mapping from substance to registered product(s);
- reviewer, review date, and confidence/status.

Do not infer a product's registration status from its hazard profile. Do not infer that a substance is currently banned in Bangladesh because it is prohibited, not approved, or under review in another jurisdiction. Foreign regulatory status can be comparative evidence, not a substitute for the applicable Bangladesh legal record.

If a product contains multiple active ingredients, count the product once in product-level totals and separately document the number of ingredient-level criteria it meets. State whether category totals are mutually exclusive; if they are not, explain why subtotals may not sum to the total.

## 5. MoA mapping and label proposal safeguards

The proposal should distinguish:

1. **Scientific classification:** Current international MoA group/code assigned by the relevant classification body.
2. **Ingredient identity:** Exact active ingredient(s) in the registered formulation.
3. **Product registration:** Bangladesh registration record and status, checked independently.
4. **Label recommendation:** Proposed placement and wording for a label, subject to the competent authority's technical and legal review.

Use IRAC for insecticide/acaricide MoA, FRAC for fungicide MoA, and HRAC/WSSA classification for herbicide MoA as applicable. Capture the classification body's edition/date. Do not assign a single MoA code to a multi-ingredient product if the components have different codes; display each component and code or mark the mapping unresolved pending review. A code is not a substitute for the registered product label, permitted crop/pest claim, dose, pre-harvest interval, or other label directions.

Suggested label concept (subject to authority approval):

> **Mode of Action (MoA):** [classification body and group/code for each relevant active ingredient]  
> **Resistance-management note:** Follow the approved label and current crop/pest-specific guidance. Rotate or combine modes of action only where supported by the approved use directions and a technically sound resistance-management recommendation.

Do not imply that adding a code alone guarantees resistance prevention or authorises a tank mix.

## 6. Regulatory framing for PTAC

The immediate request should be framed as **technical examination, source verification, and a controlled pilot**, followed by a decision by the legally competent authority. The submission should not state that a field office, a general circular, or PTAC discussion by itself can change statutory registration or labelling requirements.

Recommended sequence:

1. Refer the proposal to the competent pesticide registration authority / Plant Protection Wing for legal and technical review.
2. Verify the relevant provisions of the Pesticides Act, 2018, applicable rules, registration conditions, and label-approval procedures against the current official text.
3. Convene technical review with relevant crop-protection, resistance-management, research, and extension experts.
4. Approve a limited pilot only after the authority confirms its legal and operational basis.
5. Define a controlled label template, mapping protocol, exception process, and change-control record.
6. Evaluate comprehension, mapping accuracy, label feasibility, and resistance-stewardship usefulness.
7. Submit a documented evaluation and options paper before any nationwide implementation decision.

Any legal citation in the final dossier must include the exact section/rule, official source, version/date, and a verified quotation or faithful paraphrase. If the official text has not been checked, mark the citation **LEGAL REVIEW REQUIRED**.

## 7. Quantitative claims requiring special review

Before publication, independently reproduce and source every claim about:

- total registered products and registration status;
- products or active ingredients meeting HHP criteria;
- RED/AMBER/GREEN or other priority categories;
- combination products;
- banned, cancelled, or withdrawn substances;
- pesticide poisoning, mortality, exposure, residues, MRL exceedances, environmental effects, or economic losses;
- EU/WHO/FAO/UN lists and their counts;
- the number of products/ingredients with verified MoA codes.

For each figure, provide a compact evidence note: **claim | unit | numerator | denominator | date | source | method | limitations**. If a denominator is unavailable, do not publish a percentage. If a source is secondary or its date is unclear, label the result provisional.

## 8. PTAC release checklist

- [ ] Official source register identified and dated.
- [ ] Dataset release commit, file hashes, and sync status recorded.
- [ ] Counts independently recomputed from the exact release files.
- [ ] Duplicate, missing, unknown, and ambiguous records reported.
- [ ] Product, active ingredient, recommendation, and holder units clearly distinguished.
- [ ] MoA codes checked against current classification sources; unresolved cases disclosed.
- [ ] HHP criteria applied criterion-by-criterion with source citations.
- [ ] Comparative foreign regulatory evidence not represented as Bangladesh law.
- [ ] Legal claims checked against current official Bangladesh legislation/rules.
- [ ] Every quantitative claim has a reproducible calculation and source note.
- [ ] Draft policy recommendations are clearly separated from existing legal requirements.
- [ ] A technical reviewer and a legal reviewer are identified before final submission.

## 9. Release statement template

Use only after completing the checks above:

> “This analysis uses the dataset release identified in the accompanying manifest. Counts were reproduced using the documented rules and reconciled to the cited source files. Product registration status, active-ingredient identity, MoA classification, and HHP screening were evaluated as separate data dimensions. Unresolved records are disclosed and are not silently treated as confirmed. The findings are intended to support PTAC's technical examination and do not independently determine registration status, legal compliance, or product-level risk.”

If checks remain incomplete, replace the first two sentences with:

> “This is a provisional analytical release. The stated counts are repository-derived and remain subject to independent reproduction and verification against current authoritative records.”

## 10. References to capture in the evidence register

Use primary sources wherever possible and record exact edition/date and retrieval date:

- Bangladesh: Pesticides Act, 2018; applicable rules, registration conditions, official registration list, and competent-authority decisions.
- IRAC: current Mode of Action Classification and relevant resistance-management guidance.
- FRAC: current FRAC Code List and relevant resistance-management guidance.
- HRAC / WSSA: current herbicide MoA classification, with the selected system explicitly identified.
- WHO / FAO: the precise HHP criteria and edition used for screening.
- Any comparative foreign regulator or scientific source: official URL/document, publication date, scope, and the limited proposition for which it is cited.

**This file is a quality gate for the PTAC dossier, not a substitute for the official source register, legal review, or expert review.**
