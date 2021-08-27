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
# Resources

[Getting started with OpenStreetMap](https://medium.com/@jinalfoflia/getting-started-with-openstreetmap-7f29abb2998c)