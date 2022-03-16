# Disease-Data

LOAD CSV WITH HEADERS FROM 'file:///disease.csv' AS row MERGE (n:Disease {name: row.name, ko: row.ko, description: row.description, disease_category:row.disease_category});

LOAD CSV WITH HEADERS FROM 'file:///drug.csv' AS row MERGE (n:Drug {name: row.name, ko: row.ko});

LOAD CSV WITH HEADERS FROM 'file:///symptom.csv' AS row MERGE (n:Symptom {name: row.name, ko: row.ko, taxonomy: row.taxonomy});

CREATE CONSTRAINT ON (n:Disease) ASSERT n.ko IS UNIQUE;

CREATE CONSTRAINT ON (n:Drug) ASSERT n.ko IS UNIQUE;

CREATE CONSTRAINT ON (n:Symptom) ASSERT n.ko IS UNIQUE;

LOAD CSV WITH HEADERS FROM 'file:///drug_disease.csv' AS row MERGE (n1:Disease {ko: row.from}) MERGE (n2:Drug {ko: row.to}) MERGE (n1)-[r:treated_by]->(n2);

LOAD CSV WITH HEADERS FROM 'file:///symptom_disease.csv' AS row MERGE (n1:Symptom {ko: row.from}) MERGE (n2:Disease {ko: row.to}) MERGE (n1)-[r:caused_by]->(n2);
