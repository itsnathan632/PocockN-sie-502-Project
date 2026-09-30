---
template:
  id: http://www.example.com/method/academic-plan
  name: "Academic Plan"
expose:
  - kind: compose
params:
  - id: ontology
    type: iri
    defaultValue: ${context.ontology}
    required: true
---

# Academic Plan

This pattern is used to create and review an academic plan.

## Pattern

**Focal type:** `method:AcademicPlan`

**Required structure:** An academic plan identifies the catalog year and the courses included in the plan.

**Expected content:** The catalog year and the courses planned by the student.

**Rule:** An academic plan must identify a catalog year and at least one course.

```tree-editor
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix method: <http://www.example.com/method/vocabulary#> .

method:AcademicPlanShape
    a sh:NodeShape ;
    sh:targetClass method:AcademicPlan ;
    sh:property [
        sh:path method:catalogYear ;
        sh:name "Catalog Year" ;
        sh:datatype <http://www.w3.org/2001/XMLSchema#integer> ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;
    
    sh:property [
        sh:path method:plansCourse ;
        sh:name "Plans Course" ;
        sh:class method:Course ;
        sh:minCount 1 ;
    ] .
```