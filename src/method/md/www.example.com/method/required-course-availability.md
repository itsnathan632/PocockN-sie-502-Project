---
template:
  id: http://www.example.com/method/required-course-availability
  name: "Required Course Availability"
expose:
  - kind: compose
params:
  - id: ontology
    type: iri
    defaultValue: ${context.ontology}
    required: true
---

## Required Course Availability

**Question:** Which required courses are currently available for the students and degree programs affected by them?

The table provides the exact course and availability values for the required-course analysis.

```table
SELECT ?ProgramName ?StudentName ?CourseCode ?Availability

WHERE {
    ?Program a <http://www.example.com/method/vocabulary#DegreeProgram> ;
        <http://www.example.com/method/vocabulary#programName> ?ProgramName ;
        <http://www.example.com/method/vocabulary#hasRequirement> ?Requirement .

    ?Requirement <http://www.example.com/method/vocabulary#satisfiedBy> ?Course .

    ?Course <http://www.example.com/method/vocabulary#courseCode> ?CourseCode .

    ?Student a <http://www.example.com/method/vocabulary#Student> ;
        <http://www.example.com/method/vocabulary#name> ?StudentName ;
        <http://www.example.com/method/vocabulary#pursues> ?Program .

    BIND("Currently Available" AS ?Availability)
}

ORDER BY ?ProgramName ?StudentName ?CourseCode