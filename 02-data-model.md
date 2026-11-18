# 02 — Data model

Before pushing data to peviitor.ro, you need to know the structure of the information. The authoritative schemas are in the [peviitor_core README](https://github.com/peviitor-ro/peviitor_core#job-model-schema), which documents both the **job model** and the **company model**.

At the upload endpoint, a job needs at least `url`, `title`, and `company`. Adding `cif` is strongly recommended so the job is linked to the correct company in search results. Optional fields include `location`, `tags`, `workmode`, `date`, `status`, `expirationdate`, and `salary`.

See [04 — Company model](04-company-model.md) for the company side of the relationship.
