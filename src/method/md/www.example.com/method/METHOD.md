# My Project Method

This method provides reusable patterns, editors, and templates for describing a university course registration and degree-planning model.

## What the method prescribes

The method organizes recurring authoring work into four description patterns:

- [Degree Requirement](degree-requirement.md) — defines the structure of a degree requirement and the courses that can satisfy it.
- [Course Offering](course-offering.md) — defines a course offering and the academic term in which it occurs.
- [Academic Plan](academic-plan.md) — defines an academic plan, its catalog year, and the courses included in it.
- [Enrollment](enrollment.md) — defines an enrollment, its course offering, and enrollment status.

Each pattern combines narrative guidance with a SHACL editor shape. The shapes specify the expected structure and validation constraints, while the project supplies the model and context.

## Why the method is organized this way

The patterns separate recurring authoring concerns so that each task has a focused editor. The reusable method-side templates can then be composed by the project without copying the methodology into the project.

The project-side Markdown files provide the project context and invoke these reusable method templates.

## Organization

The method files are stored under:

`src/method/md/www.example.com/method/`

The project descriptions are stored separately under:

`src/model/md/www.example.com/project/`

This separation keeps reusable methodology artifacts distinct from project-specific model content.