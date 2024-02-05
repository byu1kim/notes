+++
title = "Database"
weight = 5
chapter = true
pre = "<i class='fas fa-book-open'></i> &nbsp"
+++

# Database

---

#### ERD

An Entity Relationship Diagram (ERD) illustrates how “entities” (tables) and entity properties (columns) relate to each other within a relational database system.

#### Convention

- Table(entity) name: noun, singular, PascalCase
- Column : camelCase

* Minimal Form
  : do not contain the value with , (comma). divide by multiple columns..
  ex) address-Vancouver, BC -> city-vancouver, province-BC

#### Relationships

- zero or one
- exactly one
- zero or many
- one or many

- one to one
- one to many
- many to many (use bridge table)

* Make sure you have derived attribue
  : A derived attribute is an attribute whose value is calculated from other attributes. For example, age can be calculated by subtracting the birth date from the current date.
