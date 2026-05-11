# CCO Education Extension — Use Case

This folder contains the education domain extension of the Core Compliance Ontology (CCO), demonstrating how CCO can be extended for a specific domain.

## Files

- `CCO-Education.ttl` — The education domain extension ontology introducing role-based classes for education-specific agents and aligning selected EFRO concepts to CCO
- `CCO_V1.ttl` — A local copy of the Core Compliance Ontology (CCO)
- `catalog-v001.xml` — Ontology catalog file for resolving imports in Protégé

## Folder

- `imports/` — Contains `efro_module.owl`, a selected extract of EFRO classes used for alignment

## Ontology IRIs

- CCO Education: `https://www.w3id.org/cco/cco-education`
- CCO Core: `https://www.w3id.org/cco/cco`

## Usage

Open `CCO-Education.ttl` in Protégé. Requires CCO (`https://www.w3id.org/cco/cco`) to be imported alongside this ontology. The `catalog-v001.xml` file maps the CCO IRI to the local `CCO_V1.ttl` file.

If opening locally, update the `owl:imports` statement in `CCO-Education.ttl` to use absolute file paths:

```turtle
owl:imports <file:///your/local/path/CCO_V1.ttl>
owl:imports <file:///your/local/path/imports/efro_module.owl>
```
