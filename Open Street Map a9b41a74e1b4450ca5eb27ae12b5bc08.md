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

## Relation

A relation is a group of elements. In more technical terms, it is one of the core data elements and consists of one or more tags and an ordered list of one or more nodes, ways and/or relations as members which is used to define logical or geographic relationships between other elements. A member of a relation can optionally have a role which describes the part that a particular feature plays within a relation.

### Usage

They are not designed to hold loosely associated but widely spread items. It would be inappropriate, for instance, to use a relation to group 'All footpaths in East Anglia'.

### Size

It is recommended to use not more than about 300 members per relation. Reason: The more members are stuffed into a single relation, the harder it is to handle, the easier it breaks, the easier conflicts can show up and the more resources it consumes on the database and server. If you have to handle more than that amount, some recommend creating several relations and combining them in a Super-Relation (a good concept on paper but software support is poor)

### Roles

A role is an optional textual field describing the function of a member of the relation. For example, in a multipolygon relation, role inner and role outer are used to specify whether a way forms the inner or outer part of that polygon. In some countries, role east can indicate that a way carries only the eastbound lanes of a route, as an alternative to a separate relation representing the eastbound route direction.

### Types ([https://wiki.openstreetmap.org/wiki/Types_of_relation](https://wiki.openstreetmap.org/wiki/Types_of_relation))

[`Multipolygons`](https://wiki.openstreetmap.org/wiki/Multipolygon) are one of two methods to represent  [areas](https://wiki.openstreetmap.org/wiki/Area) in OpenStreetMap. While most areas are represented as a single  closed way, almost all area features can also be mapped using multipolygon relations. This is needed when the area needs to exclude inner rings (holes), has multiple outer areas (exclaves), or uses more than ~2000 nodes. In the [multipolygon relation](https://wiki.openstreetmap.org/wiki/Relation:multipolygon), the *role* [inner](https://wiki.openstreetmap.org/wiki/Role:inner) and *role* [outer](https://wiki.openstreetmap.org/wiki/Role:outer) roles are used to specify whether a member [way](https://wiki.openstreetmap.org/wiki/Way) forms the inner or outer part of that polygon enclosing an area. For example, an inner way could define an island in a lake (which is mapped as relation).

`Bus Routes` : Each variation of a bus route itinerary is represented by a relation with type=route, route=bus and ref=* and operator=* tags. The first members in the route relation are the nodes representing the stops. These are ordered in the way the vehicles travel along them. Then the ways are added. In PT v2 the ways form an ordered sequence, along the stop nodes. The ways don't get roles. If they form a continuous sequence this is apparent from the continuous line along them (in JOSM's relation editor).

## Tag

Tags describe the meaning of the particular element to which they are attached.

A tag consists of two free format text fields; a 'key' and a 'value'. Each of these are Unicode strings of up to 255 characters. For example, [highway](https://wiki.openstreetmap.org/wiki/Key:highway)=[residential](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dresidential) defines the way as a road whose main function is to give access to people's homes. An element cannot have 2 tags with the same 'key', the 'key's must be unique.

# OSM XML/XDS

An XSD (XML Schema) file defines the format of an XML file, in terms of expected elements, how they are ordered, nested and repeated, and what attributes they have. Many downstream programs that consume OSM data will require, or behave better, when supplied with a well constructed and comprehensive `.xsd` file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<osm version="0.6" generator="CGImap 0.0.2">
 <bounds minlat="54.0889580" minlon="12.2487570" maxlat="54.0913900" maxlon="12.2524800"/>
 <node id="298884269" lat="54.0901746" lon="12.2482632" user="SvenHRO" uid="46882" visible="true" version="1" changeset="676636" timestamp="2008-09-21T21:37:45Z"/>
 <node id="261728686" lat="54.0906309" lon="12.2441924" user="PikoWinter" uid="36744" visible="true" version="1" changeset="323878" timestamp="2008-05-03T13:39:23Z"/>
 <node id="1831881213" version="1" changeset="12370172" lat="54.0900666" lon="12.2539381" user="lafkor" uid="75625" visible="true" timestamp="2012-07-20T09:43:19Z">
  <tag k="name" v="Neu Broderstorf"/>
  <tag k="traffic_sign" v="city_limit"/>
 </node>
 ...
 <node id="298884272" lat="54.0901447" lon="12.2516513" user="SvenHRO" uid="46882" visible="true" version="1" changeset="676636" timestamp="2008-09-21T21:37:45Z"/>
 <way id="26659127" user="Masch" uid="55988" visible="true" version="5" changeset="4142606" timestamp="2010-03-16T11:47:08Z">
  <nd ref="292403538"/>
  <nd ref="298884289"/>
  ...
  <nd ref="261728686"/>
  <tag k="highway" v="unclassified"/>
  <tag k="name" v="Pastower Straße"/>
 </way>
 <relation id="56688" user="kmvar" uid="56190" visible="true" version="28" changeset="6947637" timestamp="2011-01-12T14:23:49Z">
  <member type="node" ref="294942404" role=""/>
  ...
  <member type="node" ref="364933006" role=""/>
  <member type="way" ref="4579143" role=""/>
  ...
  <member type="node" ref="249673494" role=""/>
  <tag k="name" v="Küstenbus Linie 123"/>
  <tag k="network" v="VVW"/>
  <tag k="operator" v="Regionalverkehr Küste"/>
  <tag k="ref" v="123"/>
  <tag k="route" v="bus"/>
  <tag k="type" v="route"/>
 </relation>
 ...
</osm>
```

## **Contents**

- an XML suffix introducing the [UTF-8](https://en.wikipedia.org/wiki/UTF-8) character encoding for the file
- an osm element, containing the version of the [API](https://wiki.openstreetmap.org/wiki/API) (and thus the features used) and the generator that distilled this file (e.g. an [editor](https://wiki.openstreetmap.org/wiki/Editor) tool)
    - a block of [nodes](https://wiki.openstreetmap.org/wiki/Nodes) containing especially the location in the [WGS84](https://wiki.openstreetmap.org/wiki/WGS84) reference system
        - the tags of each node
    - a block of [ways](https://wiki.openstreetmap.org/wiki/Ways)
        - the references to its nodes for each way
        - the tags of each way
    - a block of [relations](https://wiki.openstreetmap.org/wiki/Relations)
        - the references to its members for each relation
        - the tags of each relation

## **Certainties and Uncertainties**

If you [develop](https://wiki.openstreetmap.org/wiki/Develop) tools using this format, you can be certain that:

- blocks come in this order
- bounds will be on [API](https://wiki.openstreetmap.org/wiki/API) and [JOSM](https://wiki.openstreetmap.org/wiki/JOSM) data

You can not be certain that:

- blocks are there (e.g. only nodes, no ways)
- blocks are sorted
- element IDs are non negative (Not in all osm files. Negative ids are used by editors for new elements)
- elements have to contain tags (Many elements do not. You will even come across [Untagged unconnected nodes](https://wiki.openstreetmap.org/wiki/Untagged_unconnected_node))
- visible only if false and not in [Planet.osm](https://wiki.openstreetmap.org/wiki/Planet.osm)
- id or user name present (Not always, due to [anonymous edits](https://wiki.openstreetmap.org/wiki/Anonymous_edits) in a very early stage)
- [Changesets](https://wiki.openstreetmap.org/wiki/Changesets) have an attribute num_changes (This was abandoned from the history export tool because of inconsistencies)
- version ordering is sequential (doesn't have to be)

# Planet.osm

**Planet.osm** is the OpenStreetMap data in one file: all the [nodes, ways and relations](https://wiki.openstreetmap.org/wiki/Elements) that make up our map. A new version is released every week. It's a big file (on 2021-06-01, the plain OSM XML variant takes over 1467.2 GB when uncompressed from the 106.5 GB bzip2-compressed or 59.1 GB [PBF](https://wiki.openstreetmap.org/wiki/PBF_Format)-compressed downloaded data file).

There are also files called **Extracts** which contain OpenStreetMap Data for individual continents, countries, and metropolitan areas.

## Format

The two main formats used are [PBF](https://wiki.openstreetmap.org/wiki/PBF_Format) or `bzip2-compressed` [OSM XML](https://wiki.openstreetmap.org/wiki/OSM_XML). PBF (Protocol Buffer Format) is a compact binary format that is smaller to download and much faster to process and should be used when possible. Most common tools using OSM data support PBF.

For an overview over all `osm` file formats and conversion tools have a look at [OSM file formats](https://wiki.openstreetmap.org/wiki/OSM_file_formats).

If you are using traditional GIS tools you may want to look at [Processed data providers](https://wiki.openstreetmap.org/wiki/Processed_data_providers).

### **Unpacking `.bz2` files**

Osmosis and `osm2pgsql` allow you to use the files in `bz2-compressed` form. If you need to unpack it from `bz2` format, use [7-zip](http://www.7-zip.org/) on Windows; on Linux just type:

```bash
$ bzip2 -d planet.osm.bz2;
```

or your OS may support double-click unpacking. See Wikipedia's [list of compression programs](https://en.wikipedia.org/wiki/Comparison_of_file_archivers#Archive_format_support).

# Database

The main database is a key component of OpenStreetMap, because obviously it's where we keep all our data.

# Osmosis

Grab small region:

```bash
$ osmosis --rri --simc --rx current.osm --ac --bb \
				left=42 right=42 top=42 bottom=42 \
				clipIncompleteEntities=yes --wx new.osm
```

## **Import/Export standard files**

As PostGIS is the most reliable tool for handling shape files (shp-to-PostGIS and PostGIS-to-shape), the other commom task is to convert OSM files, that is accomplished by Osmosis, [eg.](https://stackoverflow.com/a/8947417/287948)

```bash
$ osmosis --read-apidb host="x" database="x" user="x" password="x" --write-xml file="planet.osm"

```

## OSM Change

**osmChange** is the file format used by [osmosis](https://wiki.openstreetmap.org/wiki/Osmosis) (and [osmconvert](https://wiki.openstreetmap.org/wiki/Osmconvert)) to describe differences between two dumps of OSM data. However, it can also be used as the basis for anything that needs to represent changes. For example, bulk uploads/deletes/changes are also changesets and they can also be described using this format. We also offer [Planet.osm/diffs](https://wiki.openstreetmap.org/wiki/Planet.osm/diffs) downloads in this format.

For other ways of describing differences between datasets, see [Change File Formats](https://wiki.openstreetmap.org/wiki/Change_File_Formats) ([planetdiff](https://wiki.openstreetmap.org/wiki/Planetdiff) and the [JOSM file format](https://wiki.openstreetmap.org/wiki/JOSM_file_format)).

### Example

```xml
<osmChange version="0.6" generator="acme osm editor">
    <modify>
        <node id="1234" changeset="42" version="2" lat="12.1234567" lon="-8.7654321">
            <tag k="amenity" v="school"/>
        </node>
    </modify>
</osmChange>
```

This is the [changeset](https://wiki.openstreetmap.org/wiki/Changeset) to modify that single node. The outermost tag is osmChange. Within that are three possible types of nodes:

- create
- modify
- delete

The contents of these nodes are the same as the contents of the osm tag returned from the server. It may be any collection of nodes, ways, or relations. Every element must include the changeset ID and a version number. For deletions only the node is necessary but traditionally the actual data is included to make it more obvious to humans reading the file what is actually being deleted. However, for deletion the tags of a node are not included.

Note: The create/modify/delete "action" is applied at the node/way/relation level. There is no way of applying changes at a tag level. So in other words if you wish to add tags you need to include all existing tags in your modified entry.

Note: Some OsmChange output may include generator and version tags on the outermost osmChange element to identify the source of the file. These tags are not necessary, and are ignored when uploading the OsmChange document to the server.

### Placeholder

One common issue when uploading new data to the server is that IDs are allocated by the server and thus are not known at the time the file is created. The way to deal with this is to use numbers less than zero. These function as placeholders and when the node/segment/way is created, for the rest of the file the placeholder is replaced by the ID of the node created.

The [JOSM file format](https://wiki.openstreetmap.org/wiki/JOSM_file_format) has a similar feature, [planetdiff](https://wiki.openstreetmap.org/wiki/Planetdiff) is purely designed to diff/patch planet files and does not support this. As of this writing [osmosis](https://wiki.openstreetmap.org/wiki/Osmosis) does **not** support this feature when applying changes to a database, all other tasks should support negative identifiers. However, there is a bulk uploader ([bulk_import.pl](https://wiki.openstreetmap.org/wiki/Bulk_import.pl)) that does.

A quick example of how this works:

```xml
<osmChange version="0.6" generator="acme osm editor">
    <create>
        <node id="-1" changeset="43" version="1" lat="-33.9133123" lon="151.1173123" />
        <node id="-2" changeset="43" version="1" lat="-33.9233321" lon="151.1173321" />
        <way id="-3" changeset="43" version="1">
            <nd ref="-1"/>
            <nd ref="-2"/>
        </way>
    </create>
</osmChange>
```

This creates two nodes at the specified positions and joins them with a way. This will never clash with any existing data.

To implement this programs need a cache file to track what has been uploaded and what hasn't.

Note: The upload API call supports placeholders - in fact, **all** id attributes in create elements are treated as placeholders whether negative or not. However, you should stick to negative numbers to ensure that placeholder IDs do not clash with any other IDs in the OsmChange document. Note that the Rails port substitutes negative placeholder ids only. This affects both nodes in ways, as well as relation member ids in relations. Thus, using negative placeholder ids is essentially mandatory for the placeholder replacement to take place.

# Resources

[Getting started with OpenStreetMap](https://medium.com/@jinalfoflia/getting-started-with-openstreetmap-7f29abb2998c)

[OpenStreetMap Wiki](https://wiki.openstreetmap.org/wiki)

[Osmosis](Open%20Street%20Map%20a9b41a74e1b4450ca5eb27ae12b5bc08/Osmosis%20de12cd054dbe4b918998e77837446785.md)