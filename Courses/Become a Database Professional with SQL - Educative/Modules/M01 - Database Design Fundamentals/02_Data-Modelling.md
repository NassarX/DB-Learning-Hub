# Data Modeling


> A data model is a collection of concepts or notations for describing data, data relationships, data semantics, and data constraints.

**Types of data models**

1. **High-level conceptual data models** → A way to present data that is similar to how people perceive data.
    - Entity relationship model
2. **Record-based logical data models** → Provides concepts users can understand but are still similar to the way data is stored on the computer.
    - Hierarchical model
    - Network model
    - Relational model
3. **Physical data models** → Represents how data is stored in computer memory.

**Database schema**

> A schema is the blueprint of a database. The names of tables, columns of each table, datatype, functions, and other objects are included in the schema.

**Database instance**

> An instance is the information collected in a database at some specific moment in time, also known as the **database state**

****The three-schema architecture****

The goal of the three-schema architecture is to separate the user applications from the physical database.  In this architecture, schemas can be defined at the following three levels:

1. External schema
2. Conceptual schema
3. Internal schema

****Classification of Database Management Systems****

- **Based on data model**
    - Relational data models → like Oracle, MS SQL Server, DB2 and MySQL.
    - Hierarchical data models and network data models → mainly on mainframe platforms.
    - Object oriented data models (table-oriented) → DBMSs are O2, ObjectStore, and Jasmine.
- **Based on number of users**
- **Based on database distribution**
    - Centralized systems
    - Distributed database system
    - Homogeneous distributed
    - Heterogeneous distributed