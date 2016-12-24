---
menu: main
title: Examples
weight: 5
---

### Turtle/TTL File Example

```turtle
@prefix bf: <http://buildsys.org/ontologies/BrickFrame#> .
@prefix brick: <http://buildsys.org/ontologies/Brick#> .
@prefix btag: <http://buildsys.org/ontologies/BrickTag#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix mybuilding: <http://buildsys.org/ontologies/building_example#> .
@prefix xml: <http://www.w3.org/XML/1998/namespace> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

mybuilding:ahu_1 a brick:AHU ;
    bf:feeds mybuilding:vav_1 .

mybuilding:ztemp_1 a brick:Zone_Temperature_Sensor ;
    bf:isPointOf mybuilding:vav_1 .

mybuilding:floor_1 a brick:Floor .

mybuilding:room_1 a brick:Room ;
    bf:isPartOf mybuilding:floor_1,
        mybuilding:hvaczone_1 .

mybuilding:hvaczone_1 a brick:HVAC_Zone ;
    bf:hasPart mybuilding:room_1 ;
    bf:isFedBy mybuilding:vav_1 .

mybuilding:vav_1 a brick:VAV ;
    bf:feeds mybuilding:hvaczone_1 .
```

### Links JSON File Example

```json
{
    "http://buildsys.org/ontologies/building_example#temp_sensor_hvac_zone_R739": {
        "UUID": "3a038c7c-c7e6-11e6-bfc1-1002b58053c7"
    }
}
```
