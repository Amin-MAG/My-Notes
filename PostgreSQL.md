# Postgres

## Extensions

You can get the installed extensions with `\dx`.

## Tables

To show all of the tables in the database you can use `\dt` or `\dt+`.

## Switch between databases

To show all of the databases you can use `\l` or `\l+`.

For switching, you can establish a new connection using `\c gisdb`. This will create a new connection to `gisdb` database.

## Simple queries

```sql
# To get data from nodes table 
select * from nodes;

# To get 100 rows from nodes table
select * from nodes limit 100;

# To count the rows in the table with specefic condition
select count(*) from nodes where nodes.version=2;

# Some other queries
select * from nodes where nodes.version=1 limit 10;
select * from nodes where nodes.version=1 and nodes.id > 30000 limit 10;
select * from nodes where (nodes.version=1 and nodes.id > 30000) or nodes.version=13;

# Use in operator
select * from nodes where id in (2048261650,2048261632,2048261644)
```

## `Pg_dump`

Extract a PostgreSQL database into a script file or other archive file.

```bash
pg_dump --username=<YOUR_USERNAME> dbname > outfile
```

You can also store the database:

```bash
psql --set ON_ERROR_STOP=on dbname < infile
```

## Truncate

It will delete the content of table to set of tables.

```sql
TRUNCATE bigtable, fattable;
```

## Interval

To specify a range of time, for example

```sql
from comment_comment as c
where c.author_id in (
    select fu.following_user_id
    from authentication_user as u
             inner join follow_userfollowing fu on u.id = fu.user_id
    where u.id = 3
)
  and c.create_date > now() - interval '14 days';
```

# Using PostGIS With OSM

To count and find nodes that have a specific tag.

```sql
select * from nodes where nodes.tags->'amenity' = 'bank' limit 50;
select count(*) from nodes where nodes.tags->'amenity' = 'bank';

# To count the elements not having name tag
select count(*) from ways where NOT exist(ways.tags, 'name');
```

To select all Saman banks:

```sql
select *
from nodes
where nodes.tags -> 'amenity' = 'bank'
	and (
		nodes.tags -> 'name' like '%saman%' or
		nodes.tags-> 'name' like '%Saman%' or
		nodes.tags -> 'name' like '%سامان%'
	)
limit 50;
```

To see the localities

```sql
-- Find pivotal squares
select ways.id as way_id, ways.tags->'name' as name, nodes.id as node_id, nodes.tags->'name' as locality
from ways
inner join nodes on cast(ways.tags->'smapp_locality_node_ref' as bigint) = nodes.id
where exist(ways.tags, 'smapp_locality_node_ref') limit 200;
```

## Functions

for example in OSM database or precisely postgis has `st_y()` and `st_x()` that gets the geometry object and will return a double. (latitude or longitude)

So how we can define such functions?

This code is based on `nebula-clans/huddle-server` database.

```sql
-- Create function to give likes of a category
CREATE FUNCTION get_category_likes(varchar)
    RETURNS TABLE
            (
                name  text,
                count int
            )
AS
$$
SELECT $1, count(lp.id)
from posts_post as p
         inner join category_category ccc on ccc.name = p.category_id
         inner join likes_postlike lp on p.id = lp.post_id
where p.category_id = cast($1 as varchar)
group by ccc.name
$$
    LANGUAGE SQL;
-- Call the function
SELECT *
FROM get_category_likes('tech')
```

## Cursors

You can create to cursor and open and close it in function. You can process the data row by row by fetching record and apply some changes or extracting your needed data.

This code is based on `nebula-clans/huddle-server` database.

```sql
-- Creat a function to get the titles with cursor
create or replace function get_post_titles() returns text as
$$
-- Declare a cursor
declare
    titles   text default '';
    rec_post record;
    cur_post cursor for
        select *
        from posts_post;
begin
    -- open the cursor
    open cur_post;

    loop
        -- fetch row into the film
        fetch cur_post into rec_post;
        -- exit when no more row to fetch
        exit when not found;

        -- build the output
        titles := titles || ',' || rec_post.title || ':' || rec_post.date_created;
    end loop;

    -- close the cursor
    close cur_post;

    return titles;
end;
$$
    language plpgsql;
-- Call the function
select *
from get_post_titles();
```

## Run sql files from remote

```bash
#!/bin/bash
PGPASSWORD=$POSTGRES_PASSWORD psql -h osm_db -U $POSTGRES_USER -d $POSTGRES_DB -a -f scripts/pgsnapshot_schema_0.6.sql
PGPASSWORD=$POSTGRES_PASSWORD psql -h osm_db -U $POSTGRES_USER -d $POSTGRES_DB -a -f scripts/pgsnapshot_schema_0.6_action.sql
PGPASSWORD=$POSTGRES_PASSWORD psql -h osm_db -U $POSTGRES_USER -d $POSTGRES_DB -a -f scripts/pgsnapshot_schema_0.6_bbox.sql
PGPASSWORD=$POSTGRES_PASSWORD psql -h osm_db -U $POSTGRES_USER -d $POSTGRES_DB -a -f scripts/pgsnapshot_schema_0.6_linestring.sql
```

# Arrays

Arrays start from 1 in Postgre. Using `Array` you can define a new array.

```sql
select Array [st_y(n.geom), st_x(n.geom)] as coordinates from nodes;
```

You can get the value using `[x]` sign.

# Joins

Here I join the tables using `inner join` and by aliasing the `relation_members` as `rm`. Then, I select some columns from the joined tables. 

```sql
select relations.id,
       relations.tags,
       rm.member_id
from relations
         inner join relation_members rm on relations.id = rm.relation_id
limit 20;
```

We have other kinds of joins too:

![Untitled](Postgres/Untitled.png)

## Joining more that one table

```sql
select Array [st_y(n.geom), st_x(n.geom)] as coordinates
from relations
         inner join relation_members rm on relations.id = rm.relation_id
         inner join ways w on rm.member_id = w.id
         inner join nodes n on w.nodes[1] = n.id
where relations.id = 8321779;
```

## Pagination

If you want to have pagination you can use `OFFSET` and `LIMIT`. `OFFSET` is going to skip `n` rows and fetch the data. `LIMIT` also specifies the number of rows that should be fetched.

```sql
select * from users order by users.username offset 4 limit 6;
```

# PostGIS Extension

```sql
select *, st_y(n.geom) as coor_y, st_x(n.geom) as coor_x
from nodes n
where n.id = 467789995;
```

# Hstore Extension

You can use `->` to get the value of a key. For example:

```sql
select count(*) from nodes where nodes.tags->'amenity' = 'bank';
```

To add a new key-value item:

```sql
UPDATE books SET attributes = attributes || 'includes=>"maps, plates, and pictures"' WHERE id = 9;
```

## Query

Query tags that contain a specific string

```bash
select * from nodes where id > 21000061060 and tags->'smapp_source' LIKE '%StalinPOIsMatcher%';;
```

# Miscellaneous

## **Enable `pg_stat_statements` in your Postgres container configuration**

To get query statistics in Postgres, we need to modify the Postgres config setting called `shared_preload_libraries`.

You can do this by stopping your Postgres container, and then starting it again with the correct configuration as [part of the command line arguments](https://docs.docker.com/samples/library/postgres/#database-configuration):

```bash
docker run -d --name my-pg postgres -c "shared_preload_libraries='pg_stat_statements'"
```

### **Verify that `pg_stat_statements` returns data**

As a superuser, run the following statements:

```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
SELECT calls, query FROM pg_stat_statements LIMIT 1;
```

If you have configured your database correctly, this will return a result like this:

```sql
 calls | query
-------+-------
     8 | SELECT * FROM t WHERE field = ?
(1 row)
```

## Window

```sql
SELECT client,
       rate,
       startdate,
       enddate,
       lag(rate) over client_window as pre_rate,
       lag(startdate) over client_window as pre_startdate,
       lag(enddate) over client_window as pre_enddate
FROM the_table
WINDOW client_window as (partition by client order by startdate)
ORDER BY client, stardate;
```

## Privileges

To grant privileges on a specific table to a user.

```sql
GRANT ALL PRIVILEGES ON TABLE <TABLE_NAME> TO <USERNAME>;
```

# See more

[Snippet](PostgreSQL%20Snippet.md)