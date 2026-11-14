# mark-ik/rudof provenance and patch ledger

`mark-ik/rudof` is the family's fork of [rudof](https://github.com/rudof-project/rudof),
kept under the upstream licence with the upstream authors. Mere's engine split
rules it: one fork for every rudof crate the family takes (engine-split D150),
grown to close SHACL-SPARQL (D151), on the Oxigraph fork's `oxiri` 0.3.1 (D155).

## Base

- Branch `mark-ik/main`, cut from upstream's release tag `0.3.39` (`52770ad45`).
- Moves to each upstream release by rebasing (D150, "Every release").

## Family patches over the base

Oldest first. None has an upstream source.

| Commit | Purpose |
|---|---|
| 07bba613 | Native builds without `sparql` leave out oxigraph, tokio and reqwest; `rudof_iri/http` is off by default and turned on by `rudof_rdf/sparql`; proptest moves to dev-dependencies. |
| bba37020 | `shacl/sparql-constraints` carries `sh:sparql` support apart from the `sparql` feature. |
| 6b57304d | A root `[patch.crates-io]` takes Oxigraph's crates from `mark-ik/oxigraph` so the workspace holds one `oxiri` (0.3.1); `OxrdfIriParseError` loses its `#[from]`. |
| f6d71103 | `toml` 1; `dashmap` and rudof_iri's all-target `getrandom` row dropped; the wasm `getrandom` rows move to the workspace's 0.4 with `wasm_js`. |
| 9eeed853 | Lock update; thiserror 2 and spareval's sha1 0.11 edges kept as upstream's lock has them. |

## Rebasing to a new release

1. Rebase `mark-ik/main` onto the new tag; drop any patch upstream now carries.
2. Check that the `[patch.crates-io]` rows still name the Oxigraph line the family uses.
3. Run the crate tests the assessment counts (prefixmap, rudof_iri with and
   without `http`, rudof_rdf with and without `sparql`, shacl in its three feature
   sets) and the native closure check: no oxigraph, oxhttp, hyper, tokio,
   reqwest, rustls or ring with defaults off and `sparql-constraints` on.
4. Update this file.
