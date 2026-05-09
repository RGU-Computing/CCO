# CCO Modelling Patterns

CCO is organised around seven modelling patterns that capture how regulations structure compliance requirements.

---

## 1. Regulation and Norm Pattern

A `cco:Regulation` is modelled as a regulatory instrument that specifies one or more norms via `cco:specifiesNorm`. Regulations can be related through `cco:supersedes`, a transitive property, to capture regulatory change over time, and are associated with a mandatory validity start date and an optional validity end date. This supports traceability queries from a regulation to the norms it introduces, as well as longitudinal analysis of regulatory versions.

---

## 2. Deontic Classification

CCO classifies clause-level normative statements into three mutually disjoint subclasses: `cco:Obligation`, `cco:Permission`, and `cco:Prohibition`. This classification pattern reflects a recurring distinction found across existing compliance ontologies and enables queries that separate normative statement types, such as retrieving all obligations specified by a regulation via `cco:specifiesNorm`, or isolating prohibitions from permissions when constructing downstream compliance artefacts.

---

## 3. Role-Based Applicability and RoleHolding

CCO models normative scope in terms of roles, using `cco:appliesToRole` to relate norms to the roles to which they apply. To avoid attaching roles directly to agents, CCO introduces `cco:RoleHolding` as a time-bounded association: an agent holds a role via `cco:holdsRole` through a `cco:RoleHolding` instance, which records the role held, a mandatory start date, and an optional end date.

---

## 4. Actions and Resources

CCO provides explicit links from norms to the behaviour and targets they regulate. Norms may be linked to a prescribed `cco:Action` via `cco:hasAction` and to an affected `cco:Resource` via `cco:hasObject`. Action descriptions are recorded as plain text strings via `cco:hasActionExpression`, preserving the wording of the regulatory source text.

---

## 5. Conditions and Exceptions

CCO represents normative qualification through explicit `cco:Condition` and `cco:Exception` entities, distinguishing activation conditions, under which a norm applies, from exception conditions, under which the norm's applicability or force is modified. A norm may be linked to a condition via `cco:appliesUnder`, and exceptions are linked to the norms they modify via `cco:modifiesNorm` and associated with their own applicability conditions via `cco:hasCondition`.

---

## 6. Temporal Scope: Validity and Applicability

CCO distinguishes temporal scope at two levels to reflect a nuanced difference between a regulatory instrument and the norms it defines. Regulation validity dates (`cco:hasValidityStart`, `cco:hasValidityEnd`) indicate when an instrument is in force, where the start date is mandatory and the end date is optional. Norm applicability dates (`cco:hasApplicabilityStart`, `cco:hasApplicabilityEnd`) are both optional and indicate when a specific normative statement is intended to apply. This allows a norm to be applicable either to a defined sub-period or to the complete validity period of the parent regulation.

---

## 7. Agent-Role-Obligation Pattern

To determine which agents are subject to which obligations at a given point in time, CCO combines role-based norm applicability with time-bounded role holding. A norm is scoped to a role via `cco:appliesToRole`, and an agent holds a role through a `cco:RoleHolding` instance linked via `cco:holdsRole`, recording a mandatory start date and an optional end date. An agent is therefore subject to a norm when the norm's applicability period intersects with the agent's role holding period. This pattern supports compliance queries that identify which agents are bound by which obligations during a specific time window.
