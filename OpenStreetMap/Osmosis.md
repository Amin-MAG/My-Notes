# Osmosis

## Extract OSM file to DB

Let's extract a `.osm.pbf` to the Postgres database and execute some queries.

So first you should download the ProtoBuf Binary File for your country or the region you want. 

You need to run some SQL scripts to provide some tables and make the DB instance ready before using Osmosis. I put my scripts in the `script` directory and my `osm.pbf` files in the `osm_files`:

```bash
amin@amin-MacBook-Pro osm_parser % tree
.
├── osm_files
│   ├── hamburg-latest.osm.pbf
│   └── iran-latest.osm.pbf
└── scripts
    ├── pgsnapshot_load_0.6.sql
    ├── pgsnapshot_schema_0.6.sql
    ├── pgsnapshot_schema_0.6_action.sql
    ├── pgsnapshot_schema_0.6_bbox.sql
    ├── pgsnapshot_schema_0.6_changes.sql
    ├── pgsnapshot_schema_0.6_linestring.sql
    ├── pgsnapshot_schema_0.6_upgrade_4-5.sql
    └── pgsnapshot_schema_0.6_upgrade_5-6.sql
```

Then we need a Postgres database with a PostGIS plugin to inject these data into it. You can create the instance using docker. Here is my docker-compose:

```yaml
version: '3'
services:
  postgis:
    image: postgis/postgis:latest
    shm_size: "3gb"
    environment:
      # If you need to create multiple database you can add coma separated databases eg gis,data
      POSTGRES_DB: gisdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin
#      - ALLOW_IP_RANGE=0.0.0.0/0
      # Add extensions you need to be enabled by default in the DB. Default are the five specified below
      POSTGRES_MULTIPLE_EXTENSIONS: postgis,hstore,postgis_topology,postgis_raster,pgrouting
    container_name: PostGIS
    ports:
      - "5432:5432"
    volumes:
      - ./scripts/:/scripts/
```

You should download `postgis/postgis` image. Set environment variables for extensions. As you see I make the `/scripts` directory volume, so that I can use that scripts in my PostGIS instance.

Start the docker instance:

```bash
sudo docker-compose up -d
```

Then jump into the container.

```bash
sudo docker exec -it PostGIS /bin/bash
```

Log in to the Postgres DB, Then create the extensions.

```bash
# In the container
psql admin -d gisdb
> CREATE EXTENSION postgis;
> CREATE EXTENSION hstore;
> \q
```

Then navigate to the scripts directory and run these: (It also depends on your usage)

```bash
# In the container
cd scripts
psql admin -d gisdb -f pgsnapshot_schema_0.6.sql
psql admin -d gisdb -f pgsnapshot_schema_0.6_action.sql
psql admin -d gisdb -f pgsnapshot_schema_0.6_bbox.sql
psql admin -d gisdb -f pgsnapshot_schema_0.6_linestring.sql
exit
```

It makes the database ready for Osmosis.

Then you should install Osmosis and run the command to extract the data and inject it to the database.

```bash
# For mac installation
brew install osmosis

# Go to the root directory and run the osmosis command
# This will take a while to execute
osmosis --read-pbf ./osm_files/iran-latest.osm.pbf --log-progress \
	--write-pgsql host="localhost:5432" database="gisdb" user="admin" password="admin"
```

## From Database to OSM file

```bash
osmosis --read-pgsql host="localhost:5432" user="admin" password="admin" database="cobbler_db" outPipe.0=pg \
    --dd inPipe.0=pg outPipe.0=dd --write-xml inPipe.0=dd file=enhancement.osm
```

## From Database to OSM PBF file

```bash
osm2pgsql -c -d osm -U postgres -H localhost -S C:\default.style C:\bangkok.osm.pbf
```

## Convert

Convert the `.osm` file to the `.osm.pbf` file.

```bash
osmosis --read-xml file=map.osm --write-pbf map.osm.pbf

# Or vice versa
osmosis --read-pbf file=this_is_for_test.osm.pbf --write-xml map.osm
```

## Cropping

```bash
osmosis --read-xml file="YOUR-REGION-latest.osm" \ 
	--bounding-polygon file="CITY-NAME_STATE.poly" \
	--write-xml file="CITY-NAME_STATE.osm"
```

## Applying changes

```bash
osmosis --read-xml-change file="my_update.osc" --read-xml file="tehran-copy.osm" \
	--apply-change --write-xml "applied.osm"
```

## Grab small region

```bash
$ osmosis --rri --simc --rx current.osm --ac --bb \
				left=42 right=42 top=42 bottom=42 \
				clipIncompleteEntities=yes --wx new.osm
```
