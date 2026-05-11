# CCO Education Extension

This ontology extends the Core Compliance Ontology (CCO) for the education funding domain. It introduces role-based classes for education-specific agents and aligns selected concepts from the Educational Funding Regulations Ontology (EFRO) to CCO.

## Files

- `CCO-Education.ttl` — The education domain extension ontology
- `CCO_V1.ttl` — A local copy of the Core Compliance Ontology (CCO)
- `catalog-v001.xml` — Ontology catalog file for resolving imports in Protégé
- `imports/efro_module.owl` — A targeted MIREOT extract of selected EFRO classes

## Opening in Protégé

1. Download all files maintaining the folder structure
2. Open `CCO-Education.ttl` in Protégé
3. The catalog file will automatically resolve the CCO and EFRO module imports

## Local Use

If you experience import resolution errors on reload, update the `owl:imports` statement in `CCO-Education.ttl` to use absolute file paths:

```turtle
owl:imports <file:///your/local/path/CCO_V1.ttl>
owl:imports <file:///your/local/path/imports/efro_module.owl>
```

## Ontology IRIs

- CCO Education: `https://www.w3id.org/cco/cco-education`
- CCO Core: `https://www.w3id.org/cco/cco`
