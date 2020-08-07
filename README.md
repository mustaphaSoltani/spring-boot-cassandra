
# spring-boot-cassandra

##Technology

- Java 8
- Spring Boot (with Spring Web MVC, Spring Data Cassandra)
- Cassandra 3.9.0
- cqlsh 
- Maven 

##Set up Cassandra Database
Open Cassandra CQL Shell:

- Create Cassandra must keyspace:
```
create keyspace must with replication={'class':'SimpleStrategy', 'replication_factor':1};
```
- Create tutorial table in the keyspace:
```
use must;
 
CREATE TABLE tutorial(
   id timeuuid PRIMARY KEY,
   title text,
   description text,
   published boolean
);
```

We need the to create a custom index that has options with mode: CONTAINS along with analyzer_class to make case_sensitive effective.
Run the command:
```
CREATE CUSTOM INDEX idx_title ON must.tutorial (title) 
USING 'org.apache.cassandra.index.sasi.SASIIndex' 
WITH OPTIONS = {
'mode': 'CONTAINS', 
'analyzer_class': 'org.apache.cassandra.index.sasi.analyzer.NonTokenizingAnalyzer', 
'case_sensitive': 'false'};
