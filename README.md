# University Course Registration and Degree Planning

## Summary

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