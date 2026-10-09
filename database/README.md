# Database — Provenance, Sync Status & File Inventory

This `database/` folder is the canonical pesticide-dataset bundle for the **pesticide-reform** submission dossier. It mirrors the dataset used by the live field-decision PWA at **[pesticide.krishiai.live](https://pesticide.krishiai.live)** — both this dossier and the PWA are built from the **same authoritative source**, the Bangladesh DAE 81st PTAC approval list.

## Source-of-truth repo

The "live" / operational counterpart of this dataset lives in:

> **https://github.com/moniruzjaman/AgriChem-Guide-Pest-Control-Database**
> (= PesticideNext PWA · canonical live URL: `pesticide.krishiai.live`)

Both repositories are kept in sync. When the source-of-truth repo's `data_raw/` is updated, the same files are mirrored into `database/source/` here so reviewers and PTAC members can verify byte-identity without leaving the dossier.

## Latest sync

| Field | Value |
|-------|-------|
| Source repo (latest commit at sync) | `411ad5e663fe2d8abc6afaefa6bbc461e1789e06` |
| Source commit date | 2026-10-03 14:25:53 +0600 |
| Source commit subject | `fix(sharing): hash routing + share URL + OG/Twitter meta + CNAME (#41)` |
| Sync date | 2026-10-05 |
| Sync status | ✅ **byte-identical** (SHA-256 verified — see checksums below) |

## File inventory

### `database/source/` — raw source files (mirrored from `AgriChem-Guide-Pest-Control-Database/data_raw/`)

| File | Size | Source repo path | SHA-256 (verified) |
|------|------|------------------|--------------------|
| `Bangladesh_Registered_Pesticides_Database_moa.xlsx` | 493 838 B | `data_raw/Bangladesh_Registered_Pesticides_Database_moa.xlsx` | `dfd4ebb4123d514a602d6941f3c1abfb32be6077c5a1df926b0243f50df3b528` |
| `all_pesticides.csv` | 1 020 340 B | `data_raw/all_pesticides.csv` | `0a913e817fcda9f71029c1f67d2df2440923d3986277e1c9192385c891c88efa` |
| `products.csv` | 186 792 B | `data_raw/products.csv` | `5af0533d2d2bceb2a27c9c36f9d11902c9ad79acdcebcf38f2f4023ccf14d1ab` |
| `insecticides_ptac81_part1.csv` | 17 906 B | `data_raw/insecticides_ptac81_part1.csv` | `c1b48d41f0455059f108b20c7889175c9c6957ba2bc22790b483e78fde54d177` |

**Re-verify sync locally:**

```bash
# from the pesticide-reform repo root
for f in Bangladesh_Registered_Pesticides_Database_moa.xlsx all_pesticides.csv products.csv insecticides_ptac81_part1.csv; do
  sha256sum database/source/$f
done
```

Compare against the same files at `https://github.com/moniruzjaman/AgriChem-Guide-Pest-Control-Database/tree/main/data_raw`.

### `database/bangladesh_pesticides_db_ready.xlsx` — normalized star schema (derived)

The **DB-Ready Edition** is a derived relational database built from `source/Bangladesh_Registered_Pesticides_Database_moa.xlsx`. It contains the **same 5,452 registered products** reorganised into a proper star schema for SQL/BI loading.

| Sheet | Rows | Description |
|-------|------|-------------|
| `dim_category` | 8 | Pesticide categories (Miticide, Fungicide, Insecticide, Herbicide, Stored-Grain Insecticide, Rodenticide, Bio Pesticide, Public Health) with live product counts |
| `dim_common_name` | 478 | Distinct (category, common-name) pairs with MoA codes and live product counts |
| `dim_holder` | 879 | Registration-holding companies with live product counts |
| `dim_crop` | 96 | Crops with live recommendation counts |
| `dim_pest` | 657 | Pests/diseases/weeds with live recommendation counts |
| `dim_reg_type` | 5 | Registration-number types (AP, AP (Bio), PHP, Other, missing) |
| `fact_product` | 5 452 | One row per registered brand/product (PK = `product_id`) |
| `fact_recommendation` | 7 250 | One row per (product × recommendation line; FK `product_id` → `fact_product`) |
| `audit` | 9 | Live cross-checks (`COUNTIF` / `SUMPRODUCT` / `COUNTBLANK`) — all `PASS`, 0 errors |
| `README` | — | Schema documentation |

**13,043 live formulas · 0 errors · 9 sheets** — every derived value is an Excel formula, so the workbook stays dynamic when opened in Excel.

### `database/bangladesh_pesticides_dashboard.html` — self-contained interactive dashboard

A **1.9 MB** self-contained HTML file (no backend) that visualises the same 5,452-product dataset with:

- KPI cards (total products / total recommendations / unique ingredients / unique holders / unique crops / unique pests)
- Filters: category, registration type, registration holder, free-text search
- Charts (ECharts 5.5.0): products by category, registration types, MoA top 12, top 15 holders, top crops, top pests
- Sortable, paginated 5,452-row table

Works offline — open in any modern browser.

### `database/policy/` — policy aids for global perspective

A two-file policy inventory that benchmarks the Bangladesh pesticide landscape against EU / WHO / FAO standards — useful for reviewers who want to position the submission's MoA-label proposal in a broader regulatory context.

| File | Size | Pages / Sheets | Description |
|------|------|----------------|-------------|
| `Bangladesh_Pesticide_Policy_Inventory.pdf` | 157 KB | 11 pages | PDF version — opens in any browser/PDF reader. Covers the 8-criterion WHO/FAO HHP blacklist, crop-by-crop transition plan, regulatory frameworks, residue data, and 46 references. |
| `Bangladesh_Pesticide_Policy_Inventory.xlsx` | 47 KB | 9 sheets | XLSX version with embedded charts. Sheets: Overview · HHP Criteria · Active Ingredients (45) · Formulation Materials (28) · Co-formulant Tiers (21) · Regulatory Frameworks (20) · Residue Bangladesh (27) · Policy Matrix (19) · Sources (46 references). |

**Key benchmarks surfaced by the inventory (not cleared for use as verified PTAC facts until independently reproduced and checked against the cited primary sources):**
- EU active substances: 1,473 (977 banned · 421 authorised · 75 under review)
- UN acceptable co-formulants list: 144 (EU Reg. 2021/383, +14 proposed)
- Vegetable samples exceeding MRL: 73% of contaminated produce (n = 1,577)
- HHP poisoning rate: 900 per 100,000 people — among the world's highest

The PDF is the human-readable reference; the XLSX is the machine-readable source-of-truth with all 46 references in the `Sources` sheet for traceability.

## Uploaded PTAC master workbook audit (2026-10-09)

A separate workbook, `PTAC_Pesticide_MoA_Master_Database_2026_Rev4.xlsx`, was structurally audited on 2026-10-09. The audit report is [here](policy/PTAC_MASTER_WORKBOOK_AUDIT_2026-10-09.md). Its SHA-256 is `9f4bcb72d7bafcd35b142cb89b7e2223486b61dcb15d61972e098508be930368`.

**Assessment: needs revision before formal PTAC submission.** The audited master workbook has 21 sheets and zero live Excel formulas; 5,452 Product_Register records; 1,123 `—` MoA codes; 14 duplicated registration-number keys; one missing registration number and one missing holder; all 5,452 WHO toxicity-class cells set to `—`; and a mismatch between the checklist's 4,716 mapped claim, the mapping-basis total of 4,711, and 4,329 non-dash MoA-code cells. The combination annex also needs product-level reconciliation; the audit documents the `Laraco 9SC` / AP-6069 inconsistency.

This audit concerns the uploaded **PTAC master workbook**, not the separate `database/bangladesh_pesticides_db_ready.xlsx` star-schema workbook described above. Do not transfer sheet counts, formula counts, or validation claims between these files.

The report records internal workbook findings only; it does not independently certify current DAE registration status, all scientific mappings, HHP classifications, foreign regulatory status, or Bangladesh legal interpretation. Do not describe the master workbook as fully validated or officially accepted until the release gates are cleared.

## PTAC data-quality and evidence gate

Before using this dataset for formal PTAC decision-making, follow [`PTAC_DATA_VALIDATION_AND_REGULATORY_EVIDENCE_PROTOCOL.md`](PTAC_DATA_VALIDATION_AND_REGULATORY_EVIDENCE_PROTOCOL.md). It defines source-status labels, reproducibility requirements, MoA/HHP safeguards, legal-review requirements, and a release checklist. Workbook formula checks do not independently validate source accuracy, current registration status, or scientific/regulatory interpretation.

## Cross-repo verification

The dataset in this `pesticide-reform` repo is the same dataset that powers the operational PWA at **`pesticide.krishiai.live`** (source repo: `AgriChem-Guide-Pest-Control-Database`). The dossier's MoA mapping page (`MoA_Mapping_PTAC_Bangladesh.html`) and the PWA's `DatabaseView` / `RotationPlanner` components both render the same 5,452 products / 55 MoA codes (56 including UNASSIGNED) / 478 active ingredients / 879 holders.

If the source repo is updated in the future, re-sync by re-running the `cp` commands above and updating the SHA-256 table + sync date in this README.
