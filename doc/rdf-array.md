# json-ld-api-array: the JSON-LD 1.1 API test suite for rdf-array

This fork of [w3c/json-ld-api](https://github.com/w3c/json-ld-api) carries the JSON-LD 1.1 API test
suite with its RDF expectations rewritten for [rdf-array](https://github.com/rdf-array/rdf-array): a
JSON-LD `@list` is one sequence term, `[]` the empty sequence, and there are no `rdf:first`/`rdf:rest`
ladders. Only the tests that touch RDF change (`toRdf`, `fromRdf`, `html`); `expand`, `compact`,
`flatten` and `remote-doc` go from JSON to JSON and are untouched. The specification text is not
changed. Branch `rdf-sequences`; tests at <https://rdf-array.github.io/json-ld-api-array/tests/>.
New rdf-array tests live in [json-ld-api-contrib](https://github.com/rdf-array/json-ld-api-contrib).

## How the rewrite was made

- **toRdf and html**: each expected N-Quads file with a ladder is folded into sequences
  (`tools/rdfarray.py fold --holes` in rdf-array/rdf-array; a cell with `rdf:rest` but no `rdf:first`,
  left where a list value converts to nothing, contributes no element). For toRdf the fold must agree
  with what jsonld-streaming-parser-array reads from the test's input; it does for every test but
  `#tdi03`, where that parser mishandles `@direction` in a list (unrelated to lists, the expectation is
  the specification's). The HTML tests' inputs cannot be read by that parser; their folds were checked
  by hand.
- **fromRdf**: the input N-Quads are folded with JSON-LD 1.1's own list well-formedness rule
  (`fold --json-ld`: a cell used exactly once in the whole dataset, carrying nothing but `rdf:first`,
  `rdf:rest` and an optional `rdf:type rdf:List`), so a well-formed ladder becomes the sequence the
  expected `@list` stands for and a malformed or partial one keeps exactly the triples the expected
  output already shows as properties. Reading each expected JSON-LD back with
  jsonld-streaming-parser-array gives the folded input for 19 of the 20 tests; the twentieth, `#t0008`,
  is JSON-LD 1.0 only, whose ban on lists of lists cannot hold when nested lists are nested sequences
  (`#tli03` is its 1.1 reading and holds).
- Each changed test's manifest entry gains an `"rdfArrayNote"`, which JSON-LD processors ignore.

`python3 tools/rewrite-json-ld.py ~/checkouts/rdf-array/json-ld-api-array/tests` (in rdf-array/rdf-array)
reruns it and finds nothing left to rewrite.

## What changed

| Manifest | Test | File | Change |
|---|---|---|---|
| toRdf-manifest.jsonld | `#t0013` | [0013-out.nq](../tests/toRdf/0013-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#t0014` | [0014-out.nq](../tests/toRdf/0014-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#t0015` | [0015-out.nq](../tests/toRdf/0015-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#t0025` | [0025-out.nq](../tests/toRdf/0025-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#t0036` | [0036-out.nq](../tests/toRdf/0036-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tdi03` | [di03-out.nq](../tests/toRdf/di03-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te004` | [e004-out.nq](../tests/toRdf/e004-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te015` | [e015-out.nq](../tests/toRdf/e015-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te016` | [e016-out.nq](../tests/toRdf/e016-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te023` | [e023-out.nq](../tests/toRdf/e023-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te027` | [e027-out.nq](../tests/toRdf/e027-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te029` | [e029-out.nq](../tests/toRdf/e029-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#te062` | [e062-out.nq](../tests/toRdf/e062-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli01` | [li01-out.nq](../tests/toRdf/li01-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli02` | [li02-out.nq](../tests/toRdf/li02-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli03` | [li03-out.nq](../tests/toRdf/li03-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli04` | [li04-out.nq](../tests/toRdf/li04-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli05` | [li05-out.nq](../tests/toRdf/li05-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli06` | [li06-out.nq](../tests/toRdf/li06-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli07` | [li07-out.nq](../tests/toRdf/li07-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli08` | [li08-out.nq](../tests/toRdf/li08-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli09` | [li09-out.nq](../tests/toRdf/li09-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli10` | [li10-out.nq](../tests/toRdf/li10-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli11` | [li11-out.nq](../tests/toRdf/li11-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli12` | [li12-out.nq](../tests/toRdf/li12-out.nq) | `@list` → sequence in the expected N-Quads; the value that converts to nothing leaves no element |
| toRdf-manifest.jsonld | `#tli13` | [li13-out.nq](../tests/toRdf/li13-out.nq) | `@list` → sequence in the expected N-Quads |
| toRdf-manifest.jsonld | `#tli14` | [li14-out.nq](../tests/toRdf/li14-out.nq) | `@list` → sequence in the expected N-Quads; the value that converts to nothing leaves no element |
| fromRdf-manifest.jsonld | `#t0004` | [0004-in.nq](../tests/fromRdf/0004-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0005` | [0005-in.nq](../tests/fromRdf/0005-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0006` | [0006-in.nq](../tests/fromRdf/0006-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0008` | [0008-in.nq](../tests/fromRdf/0008-in.nq) | input folded; JSON-LD 1.0 only (lists of lists), see #tli03 |
| fromRdf-manifest.jsonld | `#t0009` | [0009-in.nq](../tests/fromRdf/0009-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0011` | [0011-in.nq](../tests/fromRdf/0011-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0013` | [0013-in.nq](../tests/fromRdf/0013-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0014` | [0014-in.nq](../tests/fromRdf/0014-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0016` | [0016-in.nq](../tests/fromRdf/0016-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0020` | [0020-in.nq](../tests/fromRdf/0020-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0021` | [0021-in.nq](../tests/fromRdf/0021-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0022` | [0022-in.nq](../tests/fromRdf/0022-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0026` | [0026-in.nq](../tests/fromRdf/0026-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#tli01` | [li01-in.nq](../tests/fromRdf/li01-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#tli02` | [li02-in.nq](../tests/fromRdf/li02-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#tli03` | [li03-in.nq](../tests/fromRdf/li03-in.nq) | ladder → sequence in the input N-Quads |
| fromRdf-manifest.jsonld | `#t0010` | [0010-in.nq](../tests/fromRdf/0010-in.nq) | unchanged: not a well-formed JSON-LD list |
| fromRdf-manifest.jsonld | `#t0012` | [0012-in.nq](../tests/fromRdf/0012-in.nq) | unchanged: not a well-formed JSON-LD list |
| fromRdf-manifest.jsonld | `#t0015` | [0015-in.nq](../tests/fromRdf/0015-in.nq) | unchanged: not a well-formed JSON-LD list |
| fromRdf-manifest.jsonld | `#t0023` | [0023-in.nq](../tests/fromRdf/0023-in.nq) | unchanged: not a well-formed JSON-LD list |
| html-manifest.jsonld | `#tr001` | [r001-out.nq](../tests/html/r001-out.nq) | `@list` → sequence (HTML input: folded, not cross-checked) |
| html-manifest.jsonld | `#tr002` | [r002-out.nq](../tests/html/r002-out.nq) | `@list` → sequence (HTML input: folded, not cross-checked) |
| html-manifest.jsonld | `#tr004` | [r004-out.nq](../tests/html/r004-out.nq) | `@list` → sequence (HTML input: folded, not cross-checked) |
| html-manifest.jsonld | `#tr005` | [r005-out.nq](../tests/html/r005-out.nq) | `@list` → sequence (HTML input: folded, not cross-checked) |
