>**Normalization** procedure focuses on the characteristics of specific entities and represents the micro view of entities within the entity-relationship diagram.

> **Normalization** → The branch of relational theory that provides design insights. It is the process of determining how much redundancy exists in a table.

While
 
>**Entity relation diagram (ERD** provide the big picture, or macro view, of an organization’s data requirements and operations. 

****Normal forms****

1. **1NF →** States that the domain of an attribute must include only atomic
 (simple, indivisible) values.
    - 1NF disallows having a set of values, a tuple of values, or a combination of both as an attribute value for a single tuple.
        
        
| Roll_No | Name | Course |
| --- | --- | --- |
| 101 | Micheal | Databases, Operating Systems |
| 102 | Huber | Computer Networks, Data Structures |
        
The relation is not in 1NF because of the multi-valued attribute `Course`, To convert this table into 1NF, we must make sure that the values for the `Course` attribute are atomic.
        
| Roll_No | Name | Course |
| --- | --- | --- |
| 101 | Micheal | Databases |
| 101 | Micheal | Operating Systems |
| 102 | Huber | Computer Networks |
| 102 | Huber | Data Structures |
2. ****2NF → A**** relation must be in first normal form (1NF) and it must not contain any **partial dependencies** (**If X → Y, then XZ → YZ for any Z**).
    - A relation is in 2NF as long as it has no partial dependencies, i.e., no non-prime attributes (attributes which are not part of any candidate key) is dependent on any proper subset of a composite primary key of the table.
        
        
| Stud_Id | Course_Id | Course_Fee |
| --- | --- | --- |
| 1 | C1 | 1000 |
| 2 | C2 | 1500 |
| 1 | C4 | 2000 |
| 4 | C3 | 1000 |
| 4 | C1 | 1000 |
| 2 | C5 | 3000 |
1. `Course_Fee` alone cannot be used to identify each tuple uniquely. as well combination of `Course_Fee`together with `Stud_Id` or `Course_I` also cannot be used to uniquely identify each tuple.
2. it is evident that `Course_Id` → `Course_Fee` So this results in a partial dependency and so this relation is not in 2NF.
- To convert the above relation to 2NF, we need to split the table into two other tables such as:

| Stud_Id | Course_Id |
| --- | --- |
| 1 | C1 |
| 2 | C2 |
| 1 | C4 |
| 4 | C3 |
| 4 | C1 |
| 2 | C5 |
        
| Course_Id | Course_Fee |
| --- | --- |
| C1 | 1000 |
| C2 | 1500 |
| C3 | 1000 |
| C4 | 2000 |
| C5 | 3000 |
3. **3NF →** For a table to be in the third normal form:
    - It should be in the second normal form.
    - It should not have **transitive dependency** (**If X → Y and Y → Z, then X → Z**).
        
| Std_Id | Subject_Id | Marks_obtained | Exam_Type | Total_Marks |
| --- | --- | --- | --- | --- |
| 1 | CS-100 | 50 | Final | 100 |
| 2 | CS-100 | 70 | Final | 100 |
| 3 | CS-100 | 85 | Final | 100 |
| 1 | Math-101 | 30 | Mid-term | 50 |
| 1 | PHY-100 | 10 | Practical | 30 |
| 2 | CHEM-100 | 20 | Practical | 30 |
| 3 | PHY-120 | 40 | Mid-term | 50 |
1. The column `Exam_Type` depends on both `Std_Id`and `Subject_Id`.
2. The column `Total_Marks`depends on `Exam_Type`.
- To convert this table into 3NF, we take out the attributes `Exam_Type`
 and `Total_Marks`from the SCORE table and put them in their own table.
        
| Std_Id | Subject_Id | Marks_obtained | Exam_Id |
| --- | --- | --- | --- |
| 1 | CS-100 | 50 | 1 |
| 2 | CS-100 | 70 | 1 |
| 3 | CS-100 | 85 | 1 |
| 1 | Math-101 | 30 | 2 |
| 1 | PHY-100 | 10 | 3 |
| 2 | CHEM-100 | 20 | 3 |
| 3 | PHY-120 | 40 | 2 |
        
| Exam_Id | Exam_Type | Total_Marks |
| --- | --- | --- |
| 1 | Final | 100 |
| 2 | Mid-term | 50 |
| 3 | Practical | 30 |
        
4. ****BCNF**** → To satisfy the Boyce-Codd normal form, it should satisfy the following two conditions:
    - It should be in the third normal form.
    - For any dependency A → B, A should be a super key.
    
| Std_Id | Subject | Professor |
| --- | --- | --- |
 | 101 | CS-100 | Eddie Jessup |
 | 101 | MATH-101 | Charles Kingsfield |
 | 102 | CS-100 | Robert Langdon |
 | 102 | CHEM-101 | John Keating |
 | 103 | CS-100 | Eddie Jessup |
- There is a dependency between the subject and professor where the subject depends on the professor’s name.
- Table is not in the Boyce-Codd normal form. The reason being that `Std_Id`and `Subject`form a composite primary key, which means `Subject`is a prime attribute. but as `Professor` → `Subject` while  `Subject` is a prime attribute, `Professor` is non-prime attribute.