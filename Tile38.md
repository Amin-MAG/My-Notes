---
title: Tile38
draft: true
tags: [databases, backend, reference]
---
# Tile 38

It is a database based on Redis. It can use both persistent and in-memory mechanisms to store data. This database is specifically designed for geospatial purposes.

You can use the `tile38-cli` to connect to the instance.

# Working with Keys

It is almost similar to the Redis CLI.

## SET

```
SET areas 1 object '{ "type": "FeatureCollection", "features": [ { "type": "Feature", "properties": {}, "geometry": { "coordinates": [ -73.57179643563684, 45.50062652320608 ], "type": "Point" } } ] }'
```

- The `area` is the key of the data
- The number `1` is the ID of this object in the `areas` key
- Here we have Object/GeoJSON. It can be a Point, Bounding Box, etc.

## DEL

To delete an object 

```
DEL areas 2
```

## KEY

To list all of the available keys you can use 

```
KEYS *
```

## SCAN

To retrieve all IDs of a key collection:

```
SCAN areas 
```

## GET

To retrieve an entity with a specific ID:

```
GET areas 12342 
```

# Searching

## INTERSECTS

Finds all objects in the key that intersect with the given object.

```
INTERSECTS camps CIRCLE 30.23123 31.2312 100
```

This command will find all of the objects that have intersection with a circle with radius of `100` meters in the `(30.23123, 31.2312)` latitude and longitude.

## NEARBY

Uses a specific algorithm to find the nearest places. You can use the `LIMIT` keyword to prevent the query from scanning all objects.

# See Also

- [Databases](Databases.md)
- [Redis](Redis.md)