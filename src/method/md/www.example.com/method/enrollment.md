---
template:
  id: http://www.example.com/method/enrollment
  name: "Enrollment"
expose:
  - kind: compose
params:
  - id: ontology
    type: iri
    defaultValue: ${context.ontology}
    required: true
---

# Enrollment

This pattern is used to create and review an enrollment.

## Pattern

**Focal type:** `method:Enrollment`

**Required structure:** An enrollment identifies the course offering and the student's enrollment status.

**Expected content:** The course offering associated with the enrollment and its enrollment status.

**Rule:** An enrollment must identify a course offering and an enrollment status.

```tree-editor
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix method: <http://www.example.com/method/vocabulary#> .

method:EnrollmentShape
    a sh:NodeShape ;
    sh:targetClass method:Enrollment ;
    sh:property [
        sh:path method:forOffering ;
        sh:name "For Offering" ;
        sh:class method:CourseOffering ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;
    
    sh:property [
        sh:path method:enrollmentStatus ;
        sh:name "Enrollment Status" ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] .
```