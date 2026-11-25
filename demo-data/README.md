# Fixed demo data

These are the exact 51 papers, the master data and the answer key that the course uses (generated with `python -m datagen.generate --shipments 10 --ensure-each-problem`, seed 7).

Why they are saved here: `make demo-data` produces the same content but not the same bytes (the PDFs carry a creation time), so file fingerprints, and the numbers shown in the course, would differ on your machine. Use this copy to follow along exactly.

```bash
make demo-fixed        # copies demo-data/ into data/generated/ and loads the master data
```

Contents: `files/` (51 papers), `master.json` (suppliers, purchase orders, GRNs), `ground_truth.json` (the answers `make eval` scores against). All of it is invented: Kaveri Distributors and its suppliers do not exist.
