# Open Street Map

# Overview

[OpenStreetMap](https://www.openstreetmap.org/) is a crowdsourced project which is popularly known as the [Wikipedia of the mapping world](https://en.wikipedia.org/wiki/OpenStreetMap#cite_note-tc10-6). This is a map that is built by contributors from around the world, sharing information about places they know and love.

Most maps out there may be free for individuals to use, but use proprietary information and display a very limited view of the world, limited by the interests of the companies that make them available.

I believe that no one can know your vicinity better than you and this is the idea that OpenStreetMap is built on.

# Open Street Map Main Pieces

The code that runs [openstreetmap.org](http://openstreetmap.org/) is composed of independent components that work together to provide an API, Slippy Map, and other bits of functionality.

Slippy Map is, in general, a term referring to modern web maps which let you zoom and pan around (the map slips around when you drag the mouse).

OpenStreetMap's data, "the planet", are stored in PostgreSQL with PostGIS, and rendered into pretty map tiles with Mapnik. The Slippy Map interface for those tiles — what lets you pan and zoom the map — is powered by Leaflet.

Users can add and modify OpenStreetMap data thanks to open-source editors like iD, Potlatch 2, and JOSM.

- `The OSM website Rails Port (Ruby)`: this does the UI and API for the site. The Rails port page has plenty of useful information for getting started.
- `Search, geocoding Nominatim`: (from the Latin, 'by name') is a tool to search OSM data by name and address (geocoding) and to generate synthetic addresses of OSM points (reverse geocoding). [https://nominatim.openstreetmap.org/](https://nominatim.openstreetmap.org/)
- `Desktop map data editor JOSM (Java)`: JOSM is one of the most popular and powerful OpenStreetMap editors.
- `Online map data editor iD (Javascript)`: iD is the newest editor for OpenStreetMap.
- `Default style` at [OSM.org](http://osm.org/):  [https://github.com/gravitystorm/openstreetmap-carto](https://github.com/gravitystorm/openstreetmap-carto)
- `OSM data processing Swiss Army knife`: They are a bunch of tools in C++, Java, and ... languages. These tools help us to work with OSM data. [https://github.com/osmcode](https://github.com/osmcode)
    - A multipurpose command-line tool for working with OpenStreetMap data based on the Osmium library.
    - A fast and flexible C++ library for working with OpenStreetMap data.
    - osm2pgsql is a tool for loading OpenStreetMap data into a PostgreSQL / PostGIS database suitable for applications like rendering into a map, geocoding with Nominatim, or general analysis.
    - Osmosis is a command-line Java application for processing Open Street Map data.
- `Slippy map library Leaflet (JavaScript)`: Provides the general slippy map interface. Javascript whizzes can help us make the home page's maps even faster.
- `Map rendering with Mapnik (C++)`: The main backend for the rendering of the maps that are produced from OSM data. Mapnik is an open-source toolkit for rendering maps. Among other things, it is used to render the four main Slippy Map layers on the OpenStreetMap website. It supports a variety of geospatial data formats and provides flexible styling options for designing many different kinds of maps. [https://github.com/mapnik/mapnik/](https://github.com/mapnik/mapnik/)
- `Tile rendering system with Tirex (C++ and Perl)`: Tirex is a bunch of tools that let you run a tile server. A tile server is a web server that hands out pre-rendered map raster images to clients. [https://github.com/openstreetmap/tirex/](https://github.com/openstreetmap/tirex/)

![Untitled](Open%20Street%20Map%20a9b41a74e1b4450ca5eb27ae12b5bc08/Untitled.png)

[https://wiki.openstreetmap.org/wiki/Component_overview](https://wiki.openstreetmap.org/wiki/Component_overview)

# Elements

**Elements** are the basic components of OpenStreetMap's conceptual data model of the physical world. Elements are of three types:

- **[nodes](https://wiki.openstreetmap.org/wiki/Node)** (defining points in space),
- **[ways](https://wiki.openstreetmap.org/wiki/Way)** (defining linear features and area boundaries), and
- **[relations](https://wiki.openstreetmap.org/wiki/Relation)** (which are sometimes used to explain how other elements work together).

All of the above can have one or more associated tags (which describe the meaning of a particular element).

## Node

A **node** is one of the core [elements](https://wiki.openstreetmap.org/wiki/Elements) in the OpenStreetMap data model. It consists of a single point in space defined by its **latitude**, **longitude,** and **node id**. A third, optional dimension (altitude) can also be included `key:ele` (abbreviation for "elevation")

### Point features

A node can also be defined as part of a particular `layer=*` or `level=*`, where distinct features pass over or under one another; say, at a bridge.

### Nodes on ways

Many nodes form part of one or more ways, defining the shape or "path" of the way.

### Structure

- `id`: 64-bit integer
- `lat`: between −90.0000000 and 90.0000000
- `lon`: between −180.0000000 and 180.0000000
- `tags`: a set of tags with `k` (key) and `v` (value) attributes.

```xml
<node id="25496583" lat="51.5173639" lon="-0.140043" version="1" changeset="203496" user="80n" uid="1238" visible="true" timestamp="2007-01-28T11:40:26Z">
    <tag k="highway" v="traffic_signals"/>
</node>
```

## Way

A **way** is one of the fundamental [elements](https://wiki.openstreetmap.org/wiki/Glossary#E) of the map. In everyday language, it is a line. A way normally represents a linear feature on the ground (such as a road, wall, or river).

Technically a way is an ordered list of [nodes](https://wiki.openstreetmap.org/wiki/Node) which normally also has at least one [tag](https://wiki.openstreetmap.org/wiki/Tags) or is included within a [Relation](https://wiki.openstreetmap.org/wiki/Relations). A way can have between 2 and 2,000 nodes, although it's possible that faulty ways with zero or a single node exist. A way can be [open](https://wiki.openstreetmap.org/wiki/Way#Open_way_.28Open_polyline.29) or [closed](https://wiki.openstreetmap.org/wiki/Way#Closed_way_.28Closed_polyline.29).

> A poly line is an open or closed sequence of connected line and/or arc segments, which are treated as a single entity. Each segment of a poly line can have a width that is either constant or tapers over the length of the segment

### Open way -  Open poly line

In the database, a way always has a direction. This is true even if the ground feature it represents is 'two-way' (e.g., most roads, where traffic passes in both directions) or has no direction (e.g., a wall). See here for how to identify the direction of a 'way'.

### Closed way - Closed poly line

In a closed way the last node of the way is identical with the first node.

> A closed way may be interpreted either as a closed poly line, or as an area, or both, depending on its tags.

The following closed ways would be interpreted as closed polylines:

- [highway](https://wiki.openstreetmap.org/wiki/Key:highway)=* Closed ways are used to define roundabouts and circular walks
- [barrier](https://wiki.openstreetmap.org/wiki/Key:barrier)=* Closed ways are used to define barriers, such as hedges and walls, that go completely round a property.

A closed way that has the tag [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes should always be interpreted as an area (but the tag is not required most of the time: see 'area', below).

### Area

An [area](https://wiki.openstreetmap.org/wiki/Area) (also polygon) is an enclosed filled area of territory defined as a closed way. Most closed ways are considered to be areas even without an [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes tag (see above for some exceptions). Examples of areas defined as closed ways include:

- [leisure](https://wiki.openstreetmap.org/wiki/Key:leisure)=[park](https://wiki.openstreetmap.org/wiki/Tag:leisure%3Dpark) to define the perimeter of a park
- [amenity](https://wiki.openstreetmap.org/wiki/Key:amenity)=[school](https://wiki.openstreetmap.org/wiki/Tag:amenity%3Dschool) to define the outline of a school

For tags which can be used to define closed poly lines it is necessary to also add an [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes if an area is desired. Examples include:

- [highway](https://wiki.openstreetmap.org/wiki/Key:highway)=[pedestrian](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dpedestrian) +  to define a pedestrian square or plaza.

    [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes

Areas can also be described using one or more ways which are associated with a [multipolygon](https://wiki.openstreetmap.org/wiki/Relation:multipolygon) [relation](https://wiki.openstreetmap.org/wiki/Relation).

- For smaller areas is it often appropriate to create a single closed [way](https://wiki.openstreetmap.org/wiki/Way) with suitable [tags](https://wiki.openstreetmap.org/wiki/Tag) and in some rare cases it is also necessary to add . See  for further details.

    [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes

    [area](https://wiki.openstreetmap.org/wiki/Key:area)=*

- For larger areas and for ones which butt up to other areas or to ways is it often more appropriate to use a [multipolygon](https://wiki.openstreetmap.org/wiki/Relation:multipolygon), again tagged as required. See [relation:multipolygon](https://wiki.openstreetmap.org/wiki/Relation:multipolygon) for more details.

In this example a lake is defined by a closed way where the last node equals the first of the way. The use of [natural](https://wiki.openstreetmap.org/wiki/Key:natural)=[water](https://wiki.openstreetmap.org/wiki/Tag:natural%3Dwater) implies [area](https://wiki.openstreetmap.org/wiki/Key:area)=yes. Note that it is not possible to describe lake surfaces having islands or islets this way, as closed ways, by definition, cannot have holes.

```xml
<way id="4876027" timestamp="2008-03-12T07:59:11Z" user="MichaelCollinson">
    <nd ref="31492372"/>
    <nd ref="31492338"/>
    <nd ref="31492370"/>
    <nd ref="31492371"/>
    <nd ref="31492372"/>
    <tag k="natural" v="water"/>
    <tag k="name" v="Spegeldammen"/>
</way>
```

Areas may also be defined with [relation:multipolygon](https://wiki.openstreetmap.org/wiki/Relation:multipolygon) as a set of ways which define one or more outer boundaries, and optionally zero or more inner boundaries ('holes'). In the example below there is one outer boundary defined by a single way, and two ways as inner:

From the data fragment alone we cannot tell if these are

- two holes (both ways are closed ways, upper picture to the right) or
- one hole (both inner ways concatenated form a closed way, lower picture to the right)

```xml
<relation id="12" timestamp="2008-12-21T19:31:43Z" user="kevjs1982" uid="84075">
    <member type="way" ref="2878061" role="outer"/> <!-- picture ref="1" -->
    <member type="way" ref="8125153" role="inner"/> <!-- picture ref="2" -->
    <member type="way" ref="8125154" role="inner"/> <!-- picture ref="3" -->

    <member type="way" ref="3811966" role=""/> <!-- empty role produces
        a warning; avoid this; most software works around it by computing
        a role, which is more expensive than having one set explicitly;
        not shown in the sample pictures to the right -->

    <tag k="type" v="multipolygon"/>
</relation>
```

### Combined Closed-Poly-line and Area

It is possible for a closed way to be tagged in a way that it should be interpreted both as a closed-polylines and also as an area.

For example, a closed way defining a roundabout surrounding a grassy area might be tagged simultaneously as :

- [highway](https://wiki.openstreetmap.org/wiki/Key:highway)=[primary](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dprimary) + , both being interpreted as a polyline along the closed way, and

    [junction](https://wiki.openstreetmap.org/wiki/Key:junction)=[roundabout](https://wiki.openstreetmap.org/wiki/Tag:junction%3Droundabout)

- [landuse](https://wiki.openstreetmap.org/wiki/Key:landuse)=[grass](https://wiki.openstreetmap.org/wiki/Tag:landuse%3Dgrass), interpreted on the area enclosed by the way.

### Example

A one-way residential street, tagged as [highway](https://wiki.openstreetmap.org/wiki/Key:highway)=[residential](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dresidential) + [name](https://wiki.openstreetmap.org/wiki/Key:name)=Clipstone Street + [oneway](https://wiki.openstreetmap.org/wiki/Key:oneway)=yes

```xml
<way id="5090250" visible="true" timestamp="2009-01-19T19:07:25Z" version="8" changeset="816806" user="Blumpsy" uid="64226">
    <nd ref="822403"/>
    <nd ref="21533912"/>
    <nd ref="821601"/>
    <nd ref="21533910"/>
    <nd ref="135791608"/>
    <nd ref="333725784"/>
    <nd ref="333725781"/>
    <nd ref="333725774"/>
    <nd ref="333725776"/>
    <nd ref="823771"/>
    <tag k="highway" v="residential"/>
    <tag k="name" v="Clipstone Street"/>
    <tag k="oneway" v="yes"/>
  </way>
```

The nodes defining the geometry of the way are enumerated in the correct order, and indicated only by reference using their unique identifier. These nodes must have been already defined separately with their coordinates.
# Resources

[Getting started with OpenStreetMap](https://medium.com/@jinalfoflia/getting-started-with-openstreetmap-7f29abb2998c)

[OpenStreetMap Wiki](https://wiki.openstreetmap.org/wiki)