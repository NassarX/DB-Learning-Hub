# ****ER and Relational Data Models****

>**The entity-relationship (ER) data model**
A high-level conceptual data model, also called ER schemas, are represented by **ER diagrams.**

- **Entities**, defined as tables that hold specific information (data attributes).
    - **Types of attributes**
        - Simple → `Name`
        - Composite → `Address` (House_no, Street and Suburb)
        - Multivalued → `Degree` (BSc, MSc or Phd)
        - Derived → `Age` which derived from `Bdate` attribute
- **Relationships**, defined as the associations or interactions between entities.
    - **The degree of a relationship type** → number of participating entities types
        - **Unary** (recursive) → Involves only one entity type.
        - **Binary** → Has two entity types linked together.
            - **Binary Constraints:**
                - ****Mapping cardinality**** → Maximum number of entities that a given entity can be associated with via a relationship.
                    - One to One (1:1), One to Many (1:N), and Many to Many (M:N).
                - **participation** → Whether the existence of an entity depends on it being related to another entity via the relationship type.
                    - ****Total (mandatory)**** → Must compulsorily participate in at least one relationship.
                    - ****Partial (optional) →**** may or may not participate in the relationship.
        - **Ternary** → Have three entity types linked together.

>**The relational data model** 
Represents the database as a collection of relations. A **relation** is nothing but a table of values.

****Properties of relational tables****

1. Each row is unique
2. Values are atomic
3. Column values are of the same kind
4. The sequence of columns is insignificant
5. The sequence of rows is insignificant
6. Each column has a unique name

**Database Keys** → To Uniquely identify any record or row of data inside a table. 

1. **Super Key** → Set of attributes within a table that can uniquely identify each record within a table.
2. ****Candidate key**** → Minimal set of fields that can uniquely identify each record in a table.
3. ****Primary key**** → Only one out of many candidate keys.
4. ****Composite key**** → Consists of two or more attributes that uniquely identify any record in a table.
5. **Foreign key** → A column or group of columns that provides a link between the data in two tables.