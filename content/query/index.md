---
menu: main
title: Query Language
weight: 5
---

## RDF Primer

<img src="../img/refresher.png">

Brick models are graphs consisting of **nodes** and **directed edges**.
These graphs are usually represented using the RDF data model, which represents a directed, labeled graph as a set of *triples*.
A triple consists of a *subject* (node), a *predicate* (directed edge) and an *object* (node).
Each triple represents 2 nodes (the subject and object) connected with a directed edge (the predicate).
We will interact with the graph by way of these triples.

<img src="../img/subpredobj.png" />

Triples are usually stored in a self-contained text file in the **Turtle format** (file ending `.ttl`).


The **nodes** in the Brick graph represent the sensors, actuators, equipment, subsystems, location and logical groupings in the building. **Edges** represent the relationships between these.
There are only a few types of relationships  that Brick captures. Here's a brief recap of some common ones:

- **Subcomponents**: `bf:hasPart`/`bf:isPartOf`. Used for mechanical composition, relating HVAC zones with physical spaces
- **Identifying flows**: `bf:feeds`/`bf:isFedBy`. Used for flows of water, electricity, air, etc.
- **Sensing + actuation**: `bf:hasPoint`/`bf:isPointOf`. Used for associating sensor, setpoints, statuses, commands, meters, etc with related equipment, locations, etc
- **Location**: `bf:hasLocation`/`bf:isLocationOf`. Identifies the physical location of something
- **Instantiation**: `rdf:type`

Recall that in the RDF data model, all nodes are either Literals (strings) or URIs. All URIs have a **namespace**: for example in the URI `https://brickschema.org/schema/1.0.1/Brick#VAV`, the namespace is `https://brickschema.org/schema/1.0.1/Brick#` and the "node" is `VAV`. For convenience, we will abbreviate the URI so we can write `brick:VAV` instead of `https://brickschema.org/schema/1.0.1/Brick#VAV`.

Here are the abbreviations we will use:

| Namespace | Usage | Abbreviation |
|-----------|-------|--------------|
| `https://brickschema.org/schema/1.0.1/Brick#` | Brick classes | `brick:` |
| `https://brickschema.org/schema/1.0.1/BrickFrame#` | Brick relationships | `bf:` |
| `http://www.w3.org/1999/02/22-rdf-syntax-ns#` | class instantiation | `rdf:` |
| `http://www.w3.org/2000/01/rdf-schema#` | subclassing | `rdfs:` |

There's a lot in those namespaces, but don't worry! It will feel managable soon enough, and HodDB includes tools to make it easier to find the URIs you need.

<a name="examplebuilding"></a>
### Example Building

Our example building contains the following components:

* one floor (`floor_1`)
* one room on the floor (`room_1`)
* one HVAC zone, containing the one room (`hvaczone_1`)
* one zone temperature sensor in the room (`ztemp_1`)
* one VAV supplying the HVAC zone (`vav_1`)
* one AHU supplying the VAV (`ahu_1`)

The Brick model of these relationships will be the triples representing the following graph

<img src="../img/building_graph.png" />

This graph would be defined by this set of triples:

```
mybuilding:ahu_1         rdf:type        brick:AHU
mybuilding:room_1        rdf:type        brick:Room
mybuilding:ztemp_1       rdf:type        brick:Zone_Temperature_Sensor
mybuilding:floor_1       rdf:type        brick:Floor
mybuilding:hvaczone_1    rdf:type        brick:HVAC_Zone
mybuilding:vav_1         rdf:type        brick:VAV
mybuilding:ahu_1         bf:feeds        mybuilding:vav_1
mybuilding:room_1        bf:isPartOf     mybuilding:floor_1
mybuilding:room_1        bf:isPartOf     mybuilding:hvaczone_1
mybuilding:ztemp_1       bf:isPointOf    mybuilding:vav_1
mybuilding:vav_1         bf:feeds        mybuilding:hvaczone_1
```

Note that we are using a new namespace to "store" the names of the entities that are actually in our building.

## SPARQL

SPARQL is a query language for RDF, but common Brick usage only uses a subset of its features.

One way of thinking about SPARQL is treating a query like pattern matching over the graph (i.e. finding graph isomorphisms). A SPARQL query consists of a `WHERE` clause containing the patterns of triples we want to match, and a `SELECT` clause identifying which parts of those triples we want to return.

### Variables



### Basic Query Construction

### Querying Multiple Buildings

## HodDB Integration
