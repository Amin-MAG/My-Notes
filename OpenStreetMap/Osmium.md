# Osmium

## Calculate the difference

```bash
# Human readable
osmium diff [OPTIONS] OSM-FILE1 OSM-FILE2

# To create OSC file
# use -o for output
# use --progress
osmium derive-changes [OPTIONS] OSM-FILE1 OSM-FILE2
```

To have modified items use:

```bash
osmium derive-changes tehran_sauron_nodes.osm tehran_cobbler_nodesv2.osm -o diff_c2_s.osc
```

## Sort

Merge and sort the result of OSM files.

```bash
osmium sort --progress tehran-edited.osm -o sorted.osm
```