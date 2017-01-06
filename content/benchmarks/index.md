---
menu: main
title: Benchmarks
weight: 6
---

Some initial benchmarks of HodDB

All code can be found at [https://github.com/gtfierro/brick_database_eval](https://github.com/gtfierro/brick_database_eval).
Just run the Jupyter notebook and make sure Docker is installed.

Databases evaluated here:

- [Jena/Fuseki](https://jena.apache.org/documentation/serving_data/)
- [alegrograph](http://franz.com/agraph/downloads/)
- [rdf3x](https://github.com/gh-rdf3x/gh-rdf3x)
- [rdflib](https://rdflib.readthedocs.io/en/stable/)
- [hoddb](hoddb.org)

**Notes**:

- Jena/Fuseki only completed in a reasonable time on the first, trivial, query
- RDF3X is consistently the fastest, but has the following issues:
    - cannot use underscores in node/edge names
    - cannot use `*` or `+` operators on predicates
    - cannot use UNION
    - does not support loading multiple graphs

### Query 1

```sparql
SELECT ?vav
WHERE {
    ?vav rdf:type brick:VAV .
}
```

Database | Avg Execution Time (ms)
---------|------------------
alegro   |  9.71
fuseki   | 12.87
rdf3x    |  5.92
rdflib   | 29.40
hod      |  7.58

### Query 2

```sparql
SELECT DISTINCT ?sensor ?room
WHERE {

    ?sensor rdf:type/rdfs:subClassOf* brick:Zone_Temperature_Sensor .
    ?room rdf:type brick:Room .
    ?vav rdf:type brick:VAV .
    ?zone rdf:type brick:HVAC_Zone .

    ?vav bf:feeds+ ?zone .
    ?zone bf:hasPart ?room .

    {?sensor bf:isPointOf ?vav }
    UNION
    {?sensor bf:isPointOf ?room }
}
```

Database | Avg Execution Time (ms)
---------|------------------
alegro   | 1714.37
fuseki   | 24977452.12 (~7 hours)
rdf3x    | 9.86
rdflib   | 11967.27
hod      | 18.66

### Query 3

```sparql
SELECT ?vlv_cmd ?vav
WHERE {
    {
      { ?vlv_cmd rdf:type brick:Reheat_Valve_Command }
      UNION
      { ?vlv_cmd rdf:type brick:Cooling_Valve_Command }
    }
    ?vav rdf:type brick:VAV .
    ?vav bf:hasPoint+ ?vlv_cmd .
}
```

Database | Avg Execution Time (ms)
---------|------------------
alegro   |  792.135339
rdf3x    |    7.225884
rdflib   | 6186.903193
hod      |   18.360305

### Query 4

```sparql
SELECT ?floor ?room ?zone
WHERE {
    ?floor rdf:type brick:Floor .
    ?room rdf:type brick:Room .
    ?zone rdf:type brick:HVAC_Zone .

    ?room bf:isPartOf+ ?floor .
    ?room bf:isPartOf+ ?zone .
}
```

Database | Avg Execution Time (ms)
---------|------------------
alegro   | 109.66
rdf3x    | 9.25
rdflib   | 421.21
hod      | 38.24
