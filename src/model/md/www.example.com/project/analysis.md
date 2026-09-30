---
ontology: http://www.example.com/project/description#
---

# My Project Analysis

This notebook answers the Module 1 methodology questions using evidence from this project model.

## Question 1 — Required Course Availability

**Question:** Which degree programs and students would be affected if a required course became unavailable during a particular term?

For this analysis, Fall 2026 is the term being examined. The query identifies the required courses for each degree program and student and reports whether each required course has an offering during that term.

```table
---
orderBy: ["Program asc", "Student asc"]
---
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Program ?Student ?RequiredCourse ?CurrentAvailability
WHERE {
    VALUES (?UnavailableCourse ?UnavailableTerm) {
        ("IS 101" "Fall 2026")
    }

    ?student a method:Student ;
        method:name ?Student ;
        method:pursues ?program .

    ?program a method:DegreeProgram ;
        method:programName ?Program ;
        method:hasRequirement ?requirement .

    ?requirement method:satisfiedBy ?course .

    ?course method:courseCode ?RequiredCourse .

    FILTER (?RequiredCourse = ?UnavailableCourse)

    BIND(
        IF(
            EXISTS {
                ?course method:hasOffering ?offering .
                ?offering method:offeredIn ?term .
                ?term method:termName ?UnavailableTerm .
            },
            "Currently Available",
            "Currently Unavailable"
        )
        AS ?CurrentAvailability
    )
}
ORDER BY ?Program ?Student
```

### Finding

The Fall 2026 analysis identifies the students and degree program associated with the required course being examined and shows whether that course has a current offering in the selected term.

## Question 2 — Degree Requirement Progress

**Question:** Which degree requirements has a student completed, which requirements remain, and which planned courses can satisfy those remaining requirements?

For this analysis, Nathan Pocock (ME!) is the student being examined. A requirement is treated as completed when the student has a completed enrollment in a course that satisfies that requirement. Planned courses are reported when a course in the student's academic plan can satisfy a remaining requirement.

```table
---
orderBy: ["RequirementStatus asc", "Requirement asc"]
---
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Requirement ?RequirementStatus ?PlannedCourse
WHERE {
    ?student a method:Student ;
        method:name "Nathan Pocock" ;
        method:pursues ?program ;
        method:hasPlan ?plan .

    ?program a method:DegreeProgram ;
        method:hasRequirement ?requirement .

    ?requirement method:name ?Requirement ;
        method:satisfiedBy ?course .

    BIND(
        IF(
            EXISTS {
                ?student method:hasEnrollment ?enrollment .
                ?enrollment method:forOffering ?offering ;
                    method:enrollmentStatus "Completed" .

                ?course method:hasOffering ?offering .
            },
            "Completed",
            "Remaining"
        )
        AS ?RequirementStatus
    )

    OPTIONAL {
        FILTER (?RequirementStatus = "Remaining")

        ?plan method:plansCourse ?plannedCourse .
        ?requirement method:satisfiedBy ?plannedCourse .
        ?plannedCourse method:courseCode ?PlannedCourse
    }
}
ORDER BY ?RequirementStatus ?Requirement
```

### Finding

The analysis shows which of Nathan Pocock's degree requirements are completed and which remain, and identifies planned courses that can satisfy remaining requirements.

## Question 3 — Next Course Options

**Question:** Which courses can a student take next based on their completed prerequisites, remaining degree requirements, and the terms in which courses are offered?

For this analysis, Nathan Pocock is the student being examined. The query identifies courses that can satisfy one of Nathan's remaining degree requirements, are offered in Spring 2027, and have prerequisites that Nathan has already completed.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT DISTINCT ?Course ?CourseTitle ?Term
WHERE {
    ?student a method:Student ;
        method:name "Nathan Pocock" ;
        method:pursues ?program .

    ?program method:hasRequirement ?requirement .

    ?requirement method:satisfiedBy ?Course .

    ?Course a method:Course ;
        method:title ?CourseTitle ;
        method:hasOffering ?offering .

    ?offering method:offeredIn ?academicTerm .

    ?academicTerm method:termName ?Term .

    FILTER (?Term = "Spring 2027")

    FILTER NOT EXISTS {
        ?student method:hasEnrollment ?completedEnrollment .
        ?completedEnrollment method:forOffering ?completedOffering ;
            method:enrollmentStatus "Completed" .

        ?Course method:hasOffering ?completedOffering .
    }

    FILTER NOT EXISTS {
        ?student method:hasEnrollment ?completedEnrollment2 .
        ?completedEnrollment2 method:forOffering ?completedOffering2 ;
            method:enrollmentStatus "Completed" .

        ?completedCourse method:hasOffering ?completedOffering2 .

        ?requirement method:satisfiedBy ?completedCourse .
    }

    FILTER NOT EXISTS {
        ?Course method:hasPrerequisite ?prerequisite .

        FILTER NOT EXISTS {
            ?student method:hasEnrollment ?prereqEnrollment .
            ?prereqEnrollment method:forOffering ?prereqOffering ;
                method:enrollmentStatus "Completed" .

            ?prerequisite method:hasOffering ?prereqOffering .
        }
    }
}
ORDER BY ?Course
```

### Finding

The analysis identifies courses that can satisfy Nathan Pocock's remaining degree requirements, are offered in Spring 2027, and have prerequisites that he has already completed.

## Question 4 — Proposed Plan and Graduation Delay

**Question:** Could a student's proposed course plan delay graduation because of prerequisites, course sequencing, or course availability?

For this analysis, Nathan Pocock is the student being examined. The query checks the courses in Nathan's academic plan against their prerequisites and course offerings. The current model does not explicitly represent a graduation date, so the analysis identifies modeled prerequisite or availability conflicts rather than calculating a graduation date.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT DISTINCT ?Course ?CourseTitle ?Availability ?PrerequisiteStatus
WHERE {
    ?student a method:Student ;
        method:name "Nathan Pocock" ;
        method:hasPlan ?plan .

    ?plan method:plansCourse ?Course .

    ?Course a method:Course ;
        method:title ?CourseTitle .

    BIND(
        IF(
            EXISTS {
                ?Course method:hasOffering ?offering .
            },
            "Available",
            "No Offering"
        )
        AS ?Availability
    )

    OPTIONAL {
        ?Course method:hasPrerequisite ?prerequisite .

        OPTIONAL {
            ?student method:hasEnrollment ?completedEnrollment .
            ?completedEnrollment method:forOffering ?completedOffering ;
                method:enrollmentStatus "Completed" .

            ?prerequisite method:hasOffering ?completedOffering .
        }
    }

    BIND(
        IF(
            NOT EXISTS {
                ?Course method:hasPrerequisite ?anyPrerequisite .
            },
            "No Prerequisite",
            IF(
                BOUND(?completedOffering),
                "Prerequisite Completed",
                "Prerequisite Not Completed"
            )
        )
        AS ?PrerequisiteStatus
    )
}
ORDER BY ?Course
```

### Finding

The analysis identifies modeled prerequisite and course-availability issues in Nathan Pocock's proposed plan, the current model does not include graduation dates, so it does not calculate an actual graduation delay.

## Gap Detections - Degree Requirements

**Question:** Which degree requirements are missing a course that satisfies them?

The query checks every degree requirement for a missing `satisfiedBy` relationship. A clean result means that all degree requirements currently have at least one satisfying course recorded.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Requirement ?RequirementName
WHERE {
    ?Requirement a method:DegreeRequirement ;
        method:name ?RequirementName .

    OPTIONAL {
        ?Requirement method:satisfiedBy ?Course .
    }

    FILTER(!BOUND(?Course))
}
ORDER BY ?RequirementName
```

## Gap Detections - Course Offerings

**Question:** Which course offerings are missing an academic term?

The query checks every course offering for a missing `offeredIn` relationship. A clean result means that all course offerings currently identify an academic term.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Offering ?Term
WHERE {
    ?Offering a method:CourseOffering .

    OPTIONAL {
        ?Offering method:offeredIn ?Term .
    }

    FILTER(!BOUND(?Term))
}
ORDER BY ?Offering
```

## Gap Detections - Academic Plans

**Question:** Which academic plans are missing a planned course?

The query checks every academic plan for a missing `plansCourse` relationship. A clean result means that all academic plans currently identify at least one planned course.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Plan
WHERE {
    ?Plan a method:AcademicPlan .

    OPTIONAL {
        ?Plan method:plansCourse ?Course .
    }

    FILTER(!BOUND(?Course))
}
ORDER BY ?Plan
```

## Gap Detections - Enrollments

**Question:** Which enrollments are missing the course offering for which the student is enrolled?

The query checks every enrollment for a missing `forOffering` relationship. A clean result means that all enrollments currently identify a course offering.

```table
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Enrollment
WHERE {
    ?Enrollment a method:Enrollment .

    OPTIONAL {
        ?Enrollment method:forOffering ?Offering .
    }

    FILTER(!BOUND(?Offering))
}
ORDER BY ?Enrollment
```

## Dashboard

The dashboard presents live evidence for the project's methodology questions using views matched to the questions being answered.

### View 1 — Required Course Availability

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
```

## View 2 — Degree Requirement Progress

**Question:** Which degree requirements has a student completed, which requirements remain, and which planned courses can satisfy those remaining requirements?

The table provides the requirement status and planned course evidence for Nathan Pocock.

```table
SELECT ?RequirementName ?RequirementStatus ?PlannedCourse
WHERE {
    ?Requirement a <http://www.example.com/method/vocabulary#DegreeRequirement> ;
        <http://www.example.com/method/vocabulary#name> ?RequirementName .

    OPTIONAL {
        ?Requirement <http://www.example.com/method/vocabulary#satisfiedBy> ?Course .
        ?Course <http://www.example.com/method/vocabulary#courseCode> ?CourseCode .
    }

    OPTIONAL {
        <http://www.example.com/project/description#Nathan>
            <http://www.example.com/method/vocabulary#hasEnrollment> ?Enrollment .

        ?Enrollment <http://www.example.com/method/vocabulary#forOffering> ?Offering ;
            <http://www.example.com/method/vocabulary#enrollmentStatus> "Completed" .

        ?Course <http://www.example.com/method/vocabulary#hasOffering> ?Offering .
    }

    BIND(
        IF(BOUND(?Enrollment), "Completed", "Remaining")
        AS ?RequirementStatus
    )

    BIND(
        IF(?RequirementStatus = "Remaining" && BOUND(?CourseCode), ?CourseCode, "")
        AS ?PlannedCourse
    )
}
ORDER BY ?RequirementName
```

## View 3 — Course Offerings by Term

**Question:** How are the project's course offerings distributed across academic terms?

The chart summarizes the number of course offerings recorded for each academic term.

```chart
---
type: bar
data:
  labels: Term
  datasets:
    - label: Course Offerings
      data: OfferingCount
options:
  plugins:
    title:
      display: true
      text: Course Offerings by Term
    legend:
      position: right
---
PREFIX method: <http://www.example.com/method/vocabulary#>

SELECT ?Term (COUNT(?Offering) AS ?OfferingCount)

WHERE {
    ?Offering a method:CourseOffering ;
        method:offeredIn ?AcademicTerm .

    ?AcademicTerm method:termName ?Term .
}

GROUP BY ?Term
ORDER BY ?Term
```

### View 4 — Planned Courses

**Question:** Which courses are currently included in Nathan Pocock's academic plan?

The list shows the courses currently recorded in Nathan's academic plan.

```list
SELECT ?CourseCode

WHERE {

    <http://www.example.com/project/description#Nathan>

        <http://www.example.com/method/vocabulary#hasPlan> ?Plan .

    ?Plan <http://www.example.com/method/vocabulary#plansCourse> ?Course .

    ?Course <http://www.example.com/method/vocabulary#courseCode> ?CourseCode

}

ORDER BY ?CourseCode
```

## Computed Analysis — Course Offering Distribution

**Question:** What percentage of the project's course offerings occur in each academic term?

This analysis uses the course-offering data from the project model. The SPARQL query retrieves the number of offerings for each academic term, and a Python computation calculates each term's percentage of the total. The calculation is performed in the script rather than written back into the model.

```python
SPARQL_QUERY = """
SELECT ?term (COUNT(?offering) AS ?count)
WHERE {
    ?offering a <http://www.example.com/method/vocabulary#CourseOffering> .
    ?offering <http://www.example.com/method/vocabulary#offeredIn> ?term .
}
GROUP BY ?term
ORDER BY ?term
"""

result = await query(SPARQL_QUERY)

rows = result["rows"]

total = sum(int(row["count"]) for row in rows)

distribution = []

for row in rows:
    term = row["term"].split("#")[-1]
    count = int(row["count"])
    percentage = (count / total) * 100 if total else 0
    distribution.append({
        "term": term,
        "count": count,
        "percentage": round(percentage, 1)
    })

display(distribution)
```