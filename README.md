# University Course Registration and Degree Planning

[![CI](https://github.com/itsnathan632/PocockN-sie-502-Project/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/itsnathan632/PocockN-sie-502-Project/actions/workflows/ci.yml)
[![CD](https://github.com/itsnathan632/PocockN-sie-502-Project/actions/workflows/cd.yml/badge.svg)](https://github.com/itsnathan632/PocockN-sie-502-Project/actions/workflows/cd.yml)

**Latest release:** [v1.0.0](https://github.com/itsnathan632/PocockN-sie-502-Project/releases/tag/v1.0.0)

**Live documentation:** [Academic Planning Methodology](https://itsnathan632.github.io/PocockN-sie-502-Project/www.example.com/project/academic-plan.html)

## Model Summary

This ontology models the academic planning and course registration process for undergraduate students. 
It focuses on degree programs, degree requirements, courses, prerequisites, course offerings, academic terms, student plans, and enrollments.

## Deliverable 2 Contributions

### Model Organization
- **Vocabulary:** Defines the core concepts, properties, relationships, restrictions, and characteristics used to represent the university domain.
- **Description:** Uses the vocabulary to represent a sample university planning scenario through specific students, programs, requirements, courses, offerings, plans, and enrollments.
- **Bundles:** Setting up to organize the vocabulary and description components and their dependencies following the OML/Sierra approach.

### Key Modeling Elements
- Concept hierarchy for students, degree programs, requirements, courses, offerings, terms, plans, and enrollments.
- Scalar properties such as student ID, course code, course title, credit hours, and enrollment status.
- Relations connecting students, programs, requirements, courses, prerequisites, offerings, plans, and enrollments.
- Property characteristics such as functional, inverse functional, asymmetric, and irreflexive where appropriate.
- A defined concept, **CourseWithPrerequisite**, using an OML restriction.
- Representative instances demonstrating the university course-planning domain.

### Validation
- The project has been checked using OML linting and reasoning to ensure the model is valid and consistent.

## Deliverable 3 Contributions

### Method and Pattern Organization

- **Description Patterns:** Defined reusable patterns for degree requirements, course offerings, academic plans, and enrollments.

- **Editors:** Created one editor for each description pattern with narrative guidance and SHACL based validation rules.

- **Project Organization:** Organized project descriptions around recurring authoring concerns and provided a project bundle for combining the descriptions for analysis.

- **Method Documentation:** Added `METHOD.md` to document what the method prescribes and why the patterns and editors are organized this way.

- **Dogfooding:** Used the editors to create model instances across the four description patterns.

### Focus Questions

The analysis work focused on four questions:

1. **Required Course Availability:** Which degree programs and students would be affected if a required course became unavailable during a particular term?

2. **Degree Requirement Progress:** Which degree requirements has a student completed, which requirements remain, and which planned courses can satisfy those remaining requirements?

3. **Next Course Options:** Which courses can a student take next based on completed prerequisites, remaining degree requirements, and the terms in which courses are offered?

4. **Proposed Plan and Graduation Delay:** Could a student's proposed course plan delay graduation because of prerequisites, course sequencing, or course availability?


### Analysis and Dashboard

- **Methodology Questions:** Created analyses addressing four questions involving required course availability, degree requirement progress, next-course options, and whether a proposed course plan could create graduation delays.

- **Gap Detection:** Added queries to identify missing relationships for degree requirements, course offerings, academic plans, and enrollments.

- **Dashboard:** Created a live analysis dashboard using multiple view types, including tables, a chart, and a list.

- **Computed Analysis:** Added a Python-based analysis of course offering distribution across academic terms.

- **Reusable View Template:** Created a method-owned compose template for required course availability and reused it from the project notebook.

- **Analysis Documentation:** Organized the analysis around the question, evidence, and finding for each methodology question.

### Validation

- The project was validated using OML linting with **17 OML files checked** and no lint errors.

## Reproduction

The project artifacts are organized under the following paths:

- **Method patterns and editors:** [`src/method/md/www.example.com/method/`](src/method/md/www.example.com/method/)
- **Project descriptions:** [`src/model/md/www.example.com/project/`](src/model/md/www.example.com/project/)
- **Project OML model:** [`src/model/oml/www.example.com/project/`](src/model/oml/www.example.com/project/)

Key project files include:

- [`METHOD.md`](src/method/md/www.example.com/method/METHOD.md)
- [`degree-requirement.md`](src/method/md/www.example.com/method/degree-requirement.md)
- [`course-offering.md`](src/method/md/www.example.com/method/course-offering.md)
- [`academic-plan.md`](src/method/md/www.example.com/method/academic-plan.md)
- [`enrollment.md`](src/method/md/www.example.com/method/enrollment.md)
- [`analysis.md`](src/model/md/www.example.com/project/analysis.md)
- [`notebook.md`](src/model/md/www.example.com/project/notebook.md)
- [`description.oml`](src/model/oml/www.example.com/project/description.oml)
- [`bundle.oml`](src/model/oml/www.example.com/project/bundle.oml)