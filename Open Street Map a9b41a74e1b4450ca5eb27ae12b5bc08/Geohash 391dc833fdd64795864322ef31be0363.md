# Geohash

There is a concept converts the latitude and longitude to a specific string. The length of this string shows how exactly this location is.

The inputs are latitude, longitude and the precision. The Geohash represents a square tile on the map. if we increase the precision the square will break into other squares and it can continued.

```python
# Geohash
lat, lon = 35.65541, 51.34073
lat2, lon2  = 35.65687, 51.34088
a = geohash.encode(lat, lon, 7)
b = geohash.encode(lat2, lon2, 7)

print(f"a: {a}")
print(f"b: {b}")
```

Each one of the squares has 8 neighbors and you can get the neighbors just like below.

```python
print(f"a: {a} neighbors: {geohash.neighbours(a)}")
print(f"b: {b} neighbors: {geohash.neighbours(b)}")
```