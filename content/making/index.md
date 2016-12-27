---
menu: main
title: Making a Brick Model
weight: 4
---

Here we review the construction and Brick representation of a small example building.

### Data Model

Brick can be thought of as a vocabulary for describing the things in the building and how they relate to each other.
In Brick, names for things are referred to as *tagsets* and names for how these things are connected are called *relationships*.
For example, an instance of a `VAV` would have a `feeds` relationship with an `HVAC_Zone`.

The Brick vocabulary and class structure are available at [brickschema.org](http://brickschema.org/download/).
The best way to view the file is either using the [Protege](http://protege.stanford.edu/) tool or Python's [rdflib](https://rdflib.readthedocs.io/) library.

#### RDF Triples

To use these names to describe a building, we construct tuples consisting following the form of:

```text
subject predicate object
```

where `subject` and `object` are both *tagsets* and `predicate` is a *relationship*. This is known as an *RDF triple*, which we will often abbreviate to just triple.
