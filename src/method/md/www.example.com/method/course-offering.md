---
template:
  id: http://www.example.com/method/course-offering
  name: "Course Offering"
expose:
  - kind: compose
params:
  - id: ontology
    type: iri
    defaultValue: ${context.ontology}
    required: true
---

# Course Offering

This pattern is used to create and review a course offering.

## Pattern

**Focal type:** `method:CourseOffering`

**Required structure:** A course offering identifies the academic term in which the course is offered.

**Expected content:** The academic term associated with the course offering.

**Rule:** A course offering must identify the academic term in which it occurs.

```tree-editor
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix method: <http://www.example.com/method/vocabulary#> .

method:CourseOfferingShape
    a sh:NodeShape ;
    sh:targetClass method:CourseOffering ;
    sh:property [
        sh:path method:offeredIn ;
        sh:name "Offered In" ;
        sh:class method:AcademicTerm ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] .
```