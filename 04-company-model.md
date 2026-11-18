# 04 — Company model

A job is linked to a specific company identified by `CIF/CUI` and legal `company` name. The company must exist in the [peviitor company core](https://github.com/peviitor-ro/peviitor_core#company-model-schema), where `id` (the CIF/CUI) and `company` are mandatory.

If the company is missing, create it first as described in [10 — Creating a company in peviitor core](10-company-creation.md). To look up the correct CIF/CUI and legal name, follow [09 — Source of truth](09-source-of-truth.md).
