# CCO Stage 1: Competency Questions and SPARQL ASK Queries

This document contains the 20 competency questions (CQs) used in Stage 1 of the CCO evaluation. Each CQ is translated into a SPARQL ASK query executed against the CCO TBox to verify schema-level axioms.

---

## Category: Agent and Role

### CQ1: Which agents are subject to a given regulation through the roles to which its norms apply?

**Key Classes:** Agent, Regulation, Norm, Role, RoleHolding  
**Key Properties:** specifiesNorm, appliesToRole, holdsRole, hasRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Regulation    a owl:Class .
    cco:Norm          a owl:Class .
    cco:Role          a owl:Class .
    cco:RoleHolding   a owl:Class .
    cco:Agent         a owl:Class .

    # Property existence checks
    cco:specifiesNorm a owl:ObjectProperty .
    cco:appliesToRole a owl:ObjectProperty .
    cco:hasRole       a owl:ObjectProperty .
    cco:holdsRole     a owl:ObjectProperty .

    # Domain and range checks
    cco:specifiesNorm rdfs:domain cco:Regulation .
    cco:specifiesNorm rdfs:range  cco:Norm .

    cco:appliesToRole rdfs:domain cco:Norm .
    cco:appliesToRole rdfs:range  cco:Role .

    cco:hasRole       rdfs:domain cco:RoleHolding .
    cco:hasRole       rdfs:range  cco:Role .

    cco:holdsRole     rdfs:domain cco:Agent .
    cco:holdsRole     rdfs:range  cco:RoleHolding .
}
```

** Replace ?regulation with IRI of regulation being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?agent {
    
    ?agent a cco:Agent.
    ?agent cco:holdsRole ?roleHolding.
	?roleHolding cco:hasRole ?role.
	
	?regulation a cco:Regulation.
	?regulation cco:specifiesNorm ?norm.
	?norm cco:appliesToRole ?role.

}
```


---

### CQ2: Which roles does a given agent hold?

**Key Classes:** Agent, Role, RoleHolding  
**Key Properties:** holdsRole, hasRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Agent       a owl:Class .
    cco:RoleHolding a owl:Class .
    cco:Role        a owl:Class .

    # Property existence checks
    cco:holdsRole a owl:ObjectProperty .
    cco:hasRole   a owl:ObjectProperty .

    # Domain and range checks
    cco:holdsRole rdfs:domain cco:Agent .
    cco:holdsRole rdfs:range  cco:RoleHolding .

    cco:hasRole   rdfs:domain cco:RoleHolding .
    cco:hasRole   rdfs:range  cco:Role .
}
```

** Replace ?agent with IRI of agent being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?role {
    ?agent a cco:Agent.
    ?agent cco:holdsRole ?roleHolding.
	?roleHolding cco:hasRole ?role.

}
```

---

### CQ3: Which regulatory authority agents have issued a given regulation?

**Key Classes:** RegulatoryAuthorityAgent, Regulation  
**Key Properties:** issues

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:RegulatoryAuthorityAgent a owl:Class .
    cco:Regulation               a owl:Class .

    # Property existence check
    cco:issues a owl:ObjectProperty .

    # Domain and range checks
    cco:issues rdfs:domain cco:RegulatoryAuthorityAgent .
    cco:issues rdfs:range  cco:Regulation .
}
```

** Replace ?regulation with IRI of regulation being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?raa {
    # Class existence checks
    ?raa a cco:RegulatoryAuthorityAgent.
	?raa cco:issues ?regulation.
	?regulation a cco:Regulation.
}
```

---

### CQ4: Which agents hold a regulatory authority role?

**Key Classes:** Agent, RoleHolding, RegulatoryAuthorityRole  
**Key Properties:** holdsRole, hasRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Agent                    a owl:Class .
    cco:RoleHolding              a owl:Class .
    cco:RegulatoryAuthorityRole  a owl:Class .

    # Property existence checks
    cco:holdsRole a owl:ObjectProperty .
    cco:hasRole   a owl:ObjectProperty .

    # Domain and range checks
    cco:holdsRole rdfs:domain cco:Agent .
    cco:holdsRole rdfs:range  cco:RoleHolding .

    cco:hasRole   rdfs:domain cco:RoleHolding .
    cco:hasRole   rdfs:range  cco:Role .

    # RegulatoryAuthorityRole subclass check
    cco:RegulatoryAuthorityRole rdfs:subClassOf cco:Role .
}
```

** Replace ?rar with IRI of regulatory authority role being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?agent {
    ?agent a cco:Agent.
    ?agent cco:holdsRole/cco:hasRole ?rar.
	?rar a cco:RegulatoryAuthorityRole.
}
```

---

### CQ5: Which agents are allocated a given resource?

**Key Classes:** Agent, Resource  
**Key Properties:** allocatedTo

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Resource a owl:Class .
    cco:Agent    a owl:Class .

    # Property existence check
    cco:allocatedTo a owl:ObjectProperty .

    # Domain and range checks
    cco:allocatedTo rdfs:domain cco:Resource .
    cco:allocatedTo rdfs:range  cco:Agent .
}
```

** Replace ?resource with IRI of resource being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?agent {
	?resource a cco:Resource;
		?cco:allocatedTo ?agent.
	?agent a cco:Agent.
}
```

---

## Category: Norm and Regulation

### CQ6: What norms does a given regulation specify?

**Key Classes:** Regulation, Norm  
**Key Properties:** specifiesNorm

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Regulation a owl:Class .
    cco:Norm       a owl:Class .

    # Property existence check
    cco:specifiesNorm a owl:ObjectProperty .

    # Domain and range checks
    cco:specifiesNorm rdfs:domain cco:Regulation .
    cco:specifiesNorm rdfs:range  cco:Norm .
}
```


** Replace ?regulation with IRI of the regulation being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?norm {
    ?regulation a cco:Regulation;
		cco:specifiesNorm ?norm.
	?norm a cco:Norm.
}
```


---

### CQ7: What obligations specified by a given regulation apply to a given role?

**Key Classes:** Regulation, Obligation, Role  
**Key Properties:** specifiesNorm, appliesToRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Regulation  a owl:Class .
    cco:Norm        a owl:Class .
    cco:Obligation  a owl:Class .
    cco:Role        a owl:Class .

    # Subclass check
    cco:Obligation rdfs:subClassOf cco:Norm .

    # Property existence checks
    cco:specifiesNorm a owl:ObjectProperty .
    cco:appliesToRole a owl:ObjectProperty .

    # Domain and range checks
    cco:specifiesNorm rdfs:domain cco:Regulation .
    cco:specifiesNorm rdfs:range  cco:Norm .

    cco:appliesToRole rdfs:domain cco:Norm .
    cco:appliesToRole rdfs:range  cco:Role .
}
```
** Replace ?regulation and ?role with IRI of the regulation and role being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?obligation {
    ?regulation a cco:Regulation;
		cco:specifiesNorm ?obligation.
	?obligation a cco:Obligation;
		cco:appliesToRole ?role.
}
```
---

### CQ8: What permissions apply to a given role?

**Key Classes:** Permission, Role  
**Key Properties:** appliesToRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Permission a owl:Class .
    cco:Norm       a owl:Class .
    cco:Role       a owl:Class .

    # Subclass check
    cco:Permission rdfs:subClassOf cco:Norm .

    # Property existence check
    cco:appliesToRole a owl:ObjectProperty .

    # Domain and range checks
    cco:appliesToRole rdfs:domain cco:Norm .
    cco:appliesToRole rdfs:range  cco:Role .
}
```

** Replace ?role with IRI of the role being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?permission {
	?permission a cco:Permission;
		cco:appliesToRole ?role.
}
```

---

### CQ9: What prohibitions does a given regulation specify?

**Key Classes:** Prohibition, Regulation  
**Key Properties:** specifiesNorm

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Regulation  a owl:Class .
    cco:Norm        a owl:Class .
    cco:Prohibition a owl:Class .

    # Subclass check
    cco:Prohibition rdfs:subClassOf cco:Norm .

    # Property existence check
    cco:specifiesNorm a owl:ObjectProperty .

    # Domain and range checks
    cco:specifiesNorm rdfs:domain cco:Regulation .
    cco:specifiesNorm rdfs:range  cco:Norm .
}
```

** Replace ?regulation with IRI of the regulation being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?prohibition {
    ?regulation a cco:Regulation;
		cco:specifiesNorm ?prohibition.
	?prohibition a cco:Prohibition.
}
```
---
---

### CQ10: Which regulation supersedes a given regulation?

**Key Classes:** Regulation  
**Key Properties:** supersedes

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence check
    cco:Regulation a owl:Class .

    # Property existence check
    cco:supersedes a owl:ObjectProperty .

    # Domain and range checks
    cco:supersedes rdfs:domain cco:Regulation .
    cco:supersedes rdfs:range  cco:Regulation .

    # Transitivity check
    cco:supersedes a owl:TransitiveProperty .
}
```

** Replace ?regulation with IRI of the regulation being superseeded**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?reg {
	?reg a cco:Regulation;
		cco:supersedes ?regulation.
}
```

---

### CQ11: What resources does a given regulation regulate?

**Key Classes:** Regulation, Resource  
**Key Properties:** regulates

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Regulation a owl:Class .
    cco:Resource   a owl:Class .

    # Property existence check
    cco:regulates a owl:ObjectProperty .

    # Domain and range checks
    cco:regulates rdfs:domain cco:Regulation .
    cco:regulates rdfs:range  cco:Resource .
}
```

** Replace ?regulation with IRI of the regulation being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?reg {
	?regulation a cco:Regulation;
		cco:regulates ?resource.
}
```
---

### CQ12: Which norms apply to a given agent through the roles that the agent holds?

**Key Classes:** Agent, Role, RoleHolding, Norm  
**Key Properties:** holdsRole, hasRole, appliesToRole

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Agent       a owl:Class .
    cco:RoleHolding a owl:Class .
    cco:Role        a owl:Class .
    cco:Norm        a owl:Class .

    # Property existence checks
    cco:holdsRole     a owl:ObjectProperty .
    cco:hasRole       a owl:ObjectProperty .
    cco:appliesToRole a owl:ObjectProperty .

    # Domain and range checks
    cco:holdsRole     rdfs:domain cco:Agent .
    cco:holdsRole     rdfs:range  cco:RoleHolding .

    cco:hasRole       rdfs:domain cco:RoleHolding .
    cco:hasRole       rdfs:range  cco:Role .

    cco:appliesToRole rdfs:domain cco:Norm .
    cco:appliesToRole rdfs:range  cco:Role .
}
```

** Replace ?agent with IRI of the agent being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?norm {
	?norm a cco:Norm;
		cco:appliesToRole ?role.
	?agent a cco:Agent;
		cco:holdsRole/cco:hasRole ?role.
		
}
```

---

## Category: Temporal

### CQ13: During which time period did a given agent hold a specific role?

**Key Classes:** Agent, Role, RoleHolding  
**Key Properties:** holdsRole, hasRole, hasStartTime, hasEndTime

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

ASK {
    # Class existence checks
    cco:Agent       a owl:Class .
    cco:RoleHolding a owl:Class .
    cco:Role        a owl:Class .

    # Object property existence checks
    cco:holdsRole a owl:ObjectProperty .
    cco:hasRole   a owl:ObjectProperty .

    # Datatype property existence checks
    cco:hasStartTime a owl:DatatypeProperty .
    cco:hasEndTime   a owl:DatatypeProperty .

    # Domain and range checks
    cco:holdsRole rdfs:domain cco:Agent .
    cco:holdsRole rdfs:range  cco:RoleHolding .

    cco:hasRole   rdfs:domain cco:RoleHolding .
    cco:hasRole   rdfs:range  cco:Role .

    cco:hasStartTime rdfs:domain cco:RoleHolding .
    cco:hasStartTime rdfs:range  xsd:date .

    cco:hasEndTime   rdfs:domain cco:RoleHolding .
    cco:hasEndTime   rdfs:range  xsd:date .
}
```

** Replace ?agent with IRI of the agent being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?start ?end {

	?agent a cco:Agent;
		cco:holdsRole ?roleHolding.
	?roleHolding cco:hasRole ?role;
		cco:hasStartTime ?start;
		cco:hadEndTime ?end.
		
}
```

---

### CQ14: Which norms were applicable on a given date?

**Key Classes:** Norm  
**Key Properties:** hasApplicabilityStart, hasApplicabilityEnd

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

ASK {
    # Class existence check
    cco:Norm a owl:Class .

    # Datatype property existence checks
    cco:hasApplicabilityStart a owl:DatatypeProperty .
    cco:hasApplicabilityEnd   a owl:DatatypeProperty .

    # Domain and range checks
    cco:hasApplicabilityStart rdfs:domain cco:Norm .
    cco:hasApplicabilityStart rdfs:range  xsd:date .

    cco:hasApplicabilityEnd   rdfs:domain cco:Norm .
    cco:hasApplicabilityEnd   rdfs:range  xsd:date .
}
```

** Replace ?targetDate with the date being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?norm {
	?norm a cco:Norm;
		cco:hasApplicabilityStart ?start;
	
	OPTIONAL { ?norm cco:hasApplicabilityEnd ?end }
	FILTER (?targetDate >= ?start && (!BOUND(?end) || ?targetDate <= ?end))
}
```

---

### CQ15: Which regulations were valid during a given time period?

**Key Classes:** Regulation  
**Key Properties:** hasValidityStart, hasValidityEnd

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

ASK {
    # Class existence check
    cco:Regulation a owl:Class .

    # Datatype property existence checks
    cco:hasValidityStart a owl:DatatypeProperty .
    cco:hasValidityEnd   a owl:DatatypeProperty .

    # Domain and range checks
    cco:hasValidityStart rdfs:domain cco:Regulation .
    cco:hasValidityStart rdfs:range  xsd:date .

    cco:hasValidityEnd   rdfs:domain cco:Regulation .
    cco:hasValidityEnd   rdfs:range  xsd:date .
}
```


** Replace ?targetDate with the date being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?regulation {
	?regulation a cco:Regulation;
		cco:hasValidityStart ?start;
	
	OPTIONAL { ?regulation cco:hasValidityStart ?end }
	FILTER (?targetDate >= ?start && (!BOUND(?end) || ?targetDate <= ?end))
}
```

---

### CQ16: Which regulations have been superseded and are no longer valid?

**Key Classes:** Regulation  
**Key Properties:** supersedes, hasValidityEnd

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

ASK {
    # Class existence check
    cco:Regulation a owl:Class .

    # Property existence checks
    cco:supersedes     a owl:ObjectProperty .
    cco:hasValidityEnd a owl:DatatypeProperty .

    # Domain and range checks
    cco:supersedes     rdfs:domain cco:Regulation .
    cco:supersedes     rdfs:range  cco:Regulation .

    cco:hasValidityEnd rdfs:domain cco:Regulation .
    cco:hasValidityEnd rdfs:range  xsd:date .

    # Transitivity check
    cco:supersedes a owl:TransitiveProperty .
}
```

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?regulation {
	?regulation a cco:Regulation;
		cco:hasValidityEnd ?end.
	?superceed cco:supersedes ?regulation.
	?regulation a cco:Regulation.
		
	FILTER (now() >= ?end)
}
```

---

## Category: Exception and Condition

### CQ17: Under what conditions does a given norm apply?

**Key Classes:** Norm, Condition  
**Key Properties:** appliesUnder

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Norm      a owl:Class .
    cco:Condition a owl:Class .

    # Property existence check
    cco:appliesUnder a owl:ObjectProperty .

    # Domain and range checks
    cco:appliesUnder rdfs:domain cco:Norm .
    cco:appliesUnder rdfs:range  cco:Condition .
}
```

** Replace ?norm with the norm being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?condition {
	?norm a cco:Norm;
		cco:appliesUnder ?condition.
	?condition a cco:Condition.
}
```

---

### CQ18: Which exceptions modify a given norm?

**Key Classes:** Norm, Exception  
**Key Properties:** hasException, modifiesNorm

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Norm      a owl:Class .
    cco:Exception a owl:Class .

    # Disjointness check
    cco:Exception owl:disjointWith cco:Norm .

    # Property existence checks
    cco:hasException  a owl:ObjectProperty .
    cco:modifiesNorm  a owl:ObjectProperty .

    # Domain and range checks
    cco:hasException rdfs:domain cco:Norm .
    cco:hasException rdfs:range  cco:Exception .

    cco:modifiesNorm rdfs:domain cco:Exception .
    cco:modifiesNorm rdfs:range  cco:Norm .
}
```

** Replace ?norm with the norm being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?exception {
	?norm a cco:Norm;
		cco:hasException ?exception.
	?exception a cco:Exception.
}
```

---

### CQ19: Under what condition does a given exception apply?

**Key Classes:** Exception, Condition  
**Key Properties:** hasCondition

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Exception a owl:Class .
    cco:Condition a owl:Class .

    # Property existence check
    cco:hasCondition a owl:ObjectProperty .

    # Domain and range checks
    cco:hasCondition rdfs:domain cco:Exception .
    cco:hasCondition rdfs:range  cco:Condition .
}
```


** Replace ?exception with the exception being queried about **                                                                                                                                                                                                                                    being queried about**
```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?condition {
	?norm a cco:Norm;
		cco:hasException ?exception.
	?exception a cco:Exception;
		cco:hasCondition ?condition.
}
```

---

### CQ20: Which norms have exceptions, and under what conditions do those exceptions apply?

**Key Classes:** Norm, Exception, Condition  
**Key Properties:** hasException, modifiesNorm, hasCondition

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK {
    # Class existence checks
    cco:Norm      a owl:Class .
    cco:Exception a owl:Class .
    cco:Condition a owl:Class .

    # Disjointness check
    cco:Exception owl:disjointWith cco:Norm .

    # Property existence checks
    cco:hasException a owl:ObjectProperty .
    cco:modifiesNorm a owl:ObjectProperty .
    cco:hasCondition a owl:ObjectProperty .

    # Domain and range checks
    cco:hasException rdfs:domain cco:Norm .
    cco:hasException rdfs:range  cco:Exception .

    cco:modifiesNorm rdfs:domain cco:Exception .
    cco:modifiesNorm rdfs:range  cco:Norm .

    cco:hasCondition rdfs:domain cco:Exception .
    cco:hasCondition rdfs:range  cco:Condition .
}
```

```sparql
PREFIX cco:  <http://www.example.org/cco#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?norm {
	?norm a cco:Norm;
		cco:hasException ?exception.
	?exception a cco:Exception;
		cco:hasCondition ?condition.
}
```

