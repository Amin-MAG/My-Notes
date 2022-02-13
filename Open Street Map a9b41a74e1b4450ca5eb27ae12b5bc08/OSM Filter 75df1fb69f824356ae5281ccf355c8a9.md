# OSM Filter

## Install

```bash
wget -O - http://m.m.i24.cc/osmfilter.c |cc -x c - -O3 -o osmfilter
```

## Filter

```bash
./osmfilter norway.osm --keep="amenity=restaurant" > restaurant.osm
```

To only keep the nodes

```bash
./osmfilter tehran_base_cropped.osm --drop-ways --drop-relations > tehran_base_nodes.osm
```