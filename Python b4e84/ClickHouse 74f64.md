# ClickHouse

## Connect to the database

```python
from clickhouse_driver import Client as ClickHouseClient

CLICK_HOUSE_CONFIG = {
    'host': os.getenv('CLICKHOUSE_HOST'),
    'database': os.getenv('CLICKHOUSE_DB'),
    'user': os.getenv('CLICKHOUSE_USERNAME'),
    'password': os.getenv('CLICKHOUSE_PASSWORD'),
}

print("Connecting to ClickHouse DB ...")
ch_client = ClickHouseClient(**CLICK_HOUSE_CONFIG)
print("Finished.")
```

## Execute a command

```python
def get_previous_ride(ride: Ride, looking_range):
    query_str = """
        SELECT 
            passenger_id, origin_lat, origin_lng, destination_lat, destination_lng, created_at, created_date
        FROM 
            rides_view AS r
        WHERE 
            r.latest_ride_status = 5
            AND r.created_date > toDate(%(last_ride_created_date)s) - %(looking_range)s
            AND passenger_id = %(passenger_id)s
        """
    
    result = ch_client.execute(query_str, params={
        'passenger_id': ride.passenger_id,
        'last_ride_created_at': ride.created_at,
        'last_ride_created_date': ride.created_date,
        'looking_range': looking_range
    })
    
    return result
```