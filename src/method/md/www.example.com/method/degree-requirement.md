---
template:
  id: http://www.example.com/method/degree-requirement
  name: "Degree Requirement"
expose:
  - kind: compose
params:
  - id: ontology
    type: iri
    defaultValue: ${context.ontology}
    required: true
---

# Degree Requirement

This pattern is used to create and review a degree requirement.

## Pattern

**Focal type:** `method:DegreeRequirement`

**Required structure:** A degree requirement has a name and identifies at least one course that can satisfy the requirement.

**Expected content:** The requirement name and the course or courses that satisfy the requirement.

**Rule:** A degree requirement must identify at least one course that satisfies it.

```tree-editor
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix method: <http://www.example.com/method/vocabulary#> .

method:DegreeRequirementShape
    a sh:NodeShape ;
    sh:targetClass method:CourseRequirement ;
    sh:targetClass method:ElectiveRequirement ;
    sh:property [
        sh:path method:name ;
        sh:name "Name" ;
        sh:datatype <http://www.w3.org/2001/XMLSchema#string> ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A degree requirement must have exactly one name." ;
    ] ;

    sh:property [
        sh:path method:satisfiedBy ;
        sh:name "Satisfied By" ;
        sh:class method:Course ;
        sh:minCount 1 ;
        sh:message "A degree requirement must identify at least one course that satisfies it." ;
    ] .
```