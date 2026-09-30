---
ontology: http://www.example.com/project/description#
---

# My Project Notebook

This notebook demonstrates the reusable description patterns and editors for the My Project university course registration and degree-planning model.

The Academic Plan pattern is used as an example. An academic plan identifies a catalog year and the courses included in the plan.

## Academic Plan Editor

The live editor below uses the Academic Plan structure defined by the method.

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

## Degree Requirement Editor

The live editor below uses the Degree Requirement structure defined by the method.

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

## Course Offering Editor

The live editor below uses the Course Offering structure defined by the method.

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

## Enrollment Editor

The live editor below uses the Enrollment structure defined by the method.

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

## Reusable Required Course Availability View

The view below reuses the Required Course Availability analysis defined by the method.

```compose
template: http://www.example.com/method/required-course-availability
```