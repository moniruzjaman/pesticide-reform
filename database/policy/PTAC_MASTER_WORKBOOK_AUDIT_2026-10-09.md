# PTAC Master Workbook Audit — 2026-10-09

**Assessment: NEEDS REVISION before formal PTAC submission.**

## 1. Scope and reproducibility

- **Audited file:** `PTAC_Pesticide_MoA_Master_Database_2026_Rev4.xlsx`
- **File size:** 1,030,552 bytes
- **SHA-256:** `9f4bcb72d7bafcd35b142cb89b7e2223486b61dcb15d61972e098508be930368`
- **Audit date:** 2026-10-09
- **Method:** Read workbook structure and cell values with Python `openpyxl`; recomputed row counts, unique counts, missing values, duplicate keys, and category totals from the workbook's own cells. This was a structural/data-consistency audit, **not** verification against the live official DAE register or independent scientific/legal certification.
- **Important distinction:** This is the uploaded PTAC master workbook. It is not the separate `database/bangladesh_pesticides_db_ready.xlsx` workbook described elsewhere in this repository. Its sheet/formula counts must not be conflated with that other file.

## 2. Verified structural observations

| Check | Observed result | Interpretation |
|---|---:|---|
| Worksheets | 21 | Workbook contains 21 sheets |
| Live Excel formulas | 0 | All calculated summaries/claims are static values in this file; there are no live formulas to re-calculate |
| Product_Register rows | 5,452 records | Sequence `Sl_No` runs from 1 through 5,452 |
| Distinct nonblank registration numbers | 5,437 | 5,451 rows have a registration number; 14 registration-number keys repeat |
| Registration holder | 879 distinct nonblank holders | One product row has no holder |
| Distinct `Common_Name` strings in Product_Register | 462 | The Common_Name_Index title says 464 entries; reconcile differences in grain/normalisation |
| Combination flag | 1,398 products marked `Yes` | This count alone does not prove that every combination is correctly component-mapped |
| Combination_Register | 1,398 `CMB-` records | Two registration-number keys repeat in the annex; reconcile at product/registration/ingredient grain |
| MoA code placeholder | 1,123 products show `—` in `MoA_Code` | These must not be described as fully coded/mapped without a documented exception rule |
| Committee placeholder | 1,312 products show `Unknown` | The committee field is not resolved for a substantial set |
| WHO toxicity class | 5,452 products show `—` | The workbook does not currently contain populated WHO toxicity-class assignments |
| PHI / REI | 1,165 records have PHI and 1,165 have REI values; 4,287 have `—` in each field | Partial enrichment only; not a fully populated safety-data layer |
| EU/global status | 4,135 products show `—` | Comparative regulatory status is unpopulated/unknown for most records |
| Formulation code | 819 products show `—` | Formulation data needs confirmation for these products |
| Data dictionary coverage | 10 named workbook sheets plus an “All sheets” entry | The workbook has 21 sheets; the claim that all sheets are documented is not supported by the dictionary's sheet list |

## 3. High-priority findings

### F-01 — MoA coverage claims do not reconcile

The checklist says **4,716 products AI-mapped**. The `MoA_Mapping_Basis` categories instead show 4,522 “AI-dictionary matched” + 189 “AgriChem 59-update applied” = **4,711**, not 4,716. More importantly, 540 records labelled “AI-dictionary matched” still have `MoA_Code = —`; 582 records labelled “Register MoA code” also have `MoA_Code = —`; and one record is “Not mapped.” The workbook has 4,329 non-dash `MoA_Code` values overall. These counts measure different things and must not be presented interchangeably.

**Required action:** Define “mapped” at product and active-ingredient/component level, recalculate it from explicit component-level evidence, and disclose unresolved cases. Correct the checklist and every dashboard/annex that states 4,716 until reconciled.

### F-02 — Combination product records require product-level reconciliation

The main register marks 1,398 rows as `Combination = Yes`, and the combination annex contains 1,398 `CMB-` records, but this equality is only a row-count match. The registration-number sets do not establish a one-to-one product match because registration identifiers repeat. Example: `Laraco 9SC` (AP-6069) lists “Abamectin (3%) + Indoxacarb (6%)” but the main register marks `Combination = No` and gives only `IRAC 6`; the annex lists `IRAC 6 + IRAC 22A`. This is a material component-mapping inconsistency.

**Required action:** Reconcile each combination by a stable product key plus registration number, brand, holder, and ingredient composition. Do not certify per-component mapping until the row-level reconciliation is complete.

### F-03 — Registration-number collisions and missing identifiers

There are 5,451 nonblank registration-number cells but only 5,437 distinct values: 14 registration-number keys repeat (28 rows affected). Some duplicates associate different brands, ingredients, or holders. There is also one product with a missing registration number and one with a missing registration holder.

**Required action:** Compare each collision against the dated official registration source. Preserve legitimate variants only with evidence; otherwise correct or quarantine them. Do not silently deduplicate.

### F-04 — WHO toxicity class is not populated

Every record in `WHO_Toxicity_Class` contains `—`, while the PTAC checklist claims WHO toxicity class assignment is populated from safety data.

**Required action:** Change the checklist status to **Not met / source data required**. Populate only from a named, dated authoritative WHO classification and record substance identity, edition, source, and mapping confidence. Until then, remove claims that the field is assigned.

### F-05 — Registration status and foreign regulatory status are not established by the workbook

The workbook's `EU_Global_Reg_Status` field is `—` for 4,135 products. More generally, a comparative foreign status does not establish current Bangladesh registration status, legality, or an automatic need to ban/withdraw a product.

**Required action:** Separate (a) current Bangladesh registration status, (b) comparative foreign status, (c) hazard screening, and (d) proposed policy action. Verify each against its proper primary source. Keep unverified cases as unknown.

### F-06 — Policy dashboard language overstates the evidence

The policy layer uses labels such as “Immediate Ban,” “Pre-emptive Ban,” and “safe alternative.” These are policy classifications in the workbook, not proof of an existing Bangladesh legal order or a complete product risk assessment.

**Required action:** Label these values as **proposed policy options / prioritisation flags**, not current legal status. Replace “safe” with “candidate alternative for expert assessment” unless product-specific efficacy, exposure, label, and safety evidence supports the stronger term.

### F-07 — Static workbook; no live formula audit

The audited workbook has zero Excel formulas. A revision-history claim such as “Zero formula errors” is therefore not an adequate quality assurance statement for this file. Static values may still be inconsistent or stale.

**Required action:** Describe the workbook as a static data release, record the build script or reproducible calculation procedure, and retain an independent audit output. Do not claim formula-based live validation.

### F-08 — Incomplete fields and unsupported checklist “Met” statuses

Examples: `WHO_Toxicity_Class` is unpopulated for all 5,452 products; `PHI_Days` and `REI_Hours` are populated for 1,165 each; `EU_Global_Reg_Status` is unpopulated for 4,135; `Formulation_Code` is `—` for 819; one registration number and one holder are missing. Therefore, the blanket statement that all 21 checklist requirements are “Met” and the workbook is ready for submission is not supported.

**Required action:** Reclassify checklist items as **Met**, **Partially met**, **Provisional—review required**, or **Not met** based on evidence; remove the blanket readiness declaration.

### F-09 — Draft status and authorisation language need correction

The cover uses “OFFICIAL REGULATORY SUBMISSION” and says the workbook is “the property of PTAC” / distribution is restricted, while the review/approval/receipt signature lines are blank. Those statements may imply formal acceptance or institutional ownership that the workbook itself does not establish.

**Required action:** Use “Draft for technical review / proposed PTAC submission” until formally submitted/accepted; remove the ownership/restriction assertion unless authorised; leave approval fields blank for the competent signatories.

## 4. Priority remediation table

| Priority | Remediation | Release gate |
|---|---|---|
| Blocker | Reconcile the 4,716 vs 4,711 vs 4,329 MoA mapping figures | No MoA coverage headline until defined and reproduced |
| Blocker | Resolve combination product / component mapping, including Laraco 9SC (AP-6069) | No claim of complete per-component combination mapping |
| Blocker | Review 14 duplicated registration-number keys and missing registration/holder fields against official records | No claim of a clean unique registration register |
| Blocker | Correct WHO toxicity checklist claim; all 5,452 values are placeholders | No claim that WHO toxicity classes are populated |
| High | Separate current Bangladesh registration status from EU/global status and policy proposals | No legal/ban claim without current primary-source evidence |
| High | Reclassify checklist statuses and remove “ready for submission” statement | Draft must disclose incomplete gates |
| High | Replace PTAC ownership / official status language pending formal acceptance | Cover reflects actual draft status |
| Medium | Reconcile 462 common-name strings vs 464 index entries; complete sheet inventory in data dictionary | Documentation aligns with release file |
| Medium | Add reproducible build/audit script, file hash, and change log | Future updates can be independently reproduced |

## 5. What this audit does and does not certify

**Reproducibly derived from this workbook:** sheet count, formula count, row counts, field-level missing/placeholder counts, distinct counts, and internal inconsistencies noted above.

**Not independently verified:** whether all 5,452 records match the current official DAE register; current registration/renewal/cancellation status; scientific correctness of every IRAC/FRAC/HRAC code; ingredient concentrations and formulations; WHO/FAO HHP criterion application; comparative foreign regulatory status; Bangladesh legal interpretation; and any proposed ban/phase-out classification.

The workbook is therefore **not ready to be described as a fully validated official dataset**. It can be submitted as a **provisional technical working file only if its draft status and limitations are clearly stated**, and only after the responsible reviewers agree that the outstanding issues are suitable for PTAC's consideration.

## 6. Release statement for the current revision

> “This is a provisional technical working file prepared to support PTAC review. Internal structural and consistency checks identified unresolved MoA coverage, combination-product mapping, duplicate registration identifiers, incomplete safety fields, and unverified regulatory-status claims. The dataset has not been independently reconciled in full against the current official DAE register. Findings and policy tiers are not determinations of current legal status or automatic product withdrawal.”

