# 03 — Push and verify

We use [api.peviitor.ro](https://api.peviitor.ro) to push data to the peviitor.ro search engine. The API implementation and OpenAPI spec are in the [api repository](https://github.com/peviitor-ro/api).

Once you push data, you must test that it exists in peviitor.ro. A successful HTTP response is a good first signal, but it is not enough on its own. Search the API for your job URLs or company name to confirm the data is really indexed. See [13 — Testing and verification](13-testing-verification.md).
