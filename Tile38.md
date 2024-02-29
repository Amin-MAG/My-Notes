# Tile 38

It is a database which is based on the Redis database. It can have both persistent or in-memory mechanism to store the data. This database is specifically for geospacitial purposes.

You can use the `tile38-cli` to connect to the instance.

# Working with Keys

It is almost similar to the Redis CLI.

## SET

```
SET areas 1 object '{ "type": "FeatureCollection", "features": [ { "type": "Feature", "properties": {}, "geometry": { "coordinates": [ -73.57179643563684, 45.50062652320608 ], "type": "Point" } } ] }'
```

- The `area` is the key of the data
- The number `1` is the ID of this object in the `areas` key
- Here we have Object/GeoJson, I can be Point , Bounding Box, etc.

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

# Searching

## INTERSECTS

Finds the all of objects in the key that have an intersection with the given object.

```
INTERSECTS camps CIRCLE 30.23123 31.2312 100
```

This command will find all of the objects that have intersection with a circle with radius of `100` meters in the `(30.23123, 31.2312)` latitude and longitude.

## NEARBY

Use a specific algorithm to find the nears places. You can use `LIMIT` keyword to avoid the query to be continued for all objects. 