Title: data models and query languages
Date: 2024-12-23
Category: notes

## Relational data models

Data models are central to what is possible with an application. There are many levels of abstraction at play here. A programmer, developing an application for an end-user will choose a certain type of model that fits the use case—for example, creating classes for real-world entities. 

The language they might use will expose certain APIs that abstract over the computer memory, neatly encapsulating low-level details in a programming interface. The computer chip developer shall choose certain abstractions to represent memory units that best represent the computational model that the chip implements.

The most popular data model for developers could be considered the relational model, synonymous with the term database or “SQL DB”. It has remained popular and useful over decades even though many new-age coolDBs tried to displace it. 

### Impedance Mismatch

Relation models often lead to an impedance mismatch in how the data is represented and how it is stored in a DB. Generally speaking, developers would come across classes of entities. However, due to some restrictions on what data types can be present in the database, the schema of the data would look different from the classes defined. This leads to a need for a layer in the middle to do this mapping correctly and without writing too much special code. This middle layer is typically known as an ORM. In the Java ecosystem, Hibernate is a popular ORM.

## NoSQL

NoSQL was created as a cheeky takedown of relational DBs that promised to reduce the impedance mismatch between data and domain. There is no single type of database system that can be labeled as NoSQL, but a bunch of systems that moved away from the relational model and offered query operations not typically possible in the SQL world.

Document-type database systems are one kind of popular NoSQL database. MongoDB is perhaps the most widely known document-based database. JSON format is considered to represent the self-contained documents in a sufficiently good manner. Document databases provide locality since all the data relevant in a context is self-contained within the document. It is harder to manage many-to-many relationships in document data systems and joins often get complicated. The complexity of managing the join is often pushed to the application. This makes the application code complex and resource-intensive.


### A look at history

Before the document model, there was the network model which was standardized as something like CODASYL. This even predated the relational model.

In the network model, the link between two records would be something like a pointer in a typical programming language.

The relational model simplified things by laying records as tuples and keeping them in “relations”. The link between two records was maintained by assigning keys. In the network model to access the related records, the developer would have to write code to follow the right access paths, where this was abstracted in the relational model in the querying engine.

Document databases preserved the locality of data like in the network model, but they did away with maintaining pointers to related tuples and used the foreign key construct instead.

## Query Languages
The author makes two classifications here, declarative and imperative query languages.
Imperative languages are those where a developer specifies how to do things and not just the what. In the case of declarative languages, the developer only specifies what is to be achieved and a framework underneath the language determines how best to achieve it. 

SQL is a declarative language, other famous examples are RegEx and Haskell. 
CODASYL supported an imperative language. An example of imperative languages in databases would be stored procedures. They allow writing complex functions that generally are only achievable with the declarative SQL.

The author mentions the MapReduce programming model which has been adopted by databases. Some popular databases that support this model are MongoDB and Cassandra.


