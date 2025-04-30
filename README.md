# Redis

This directory provides a custom Redis container, designed as a submodule of the [Global City Streaming](https://github.com/mrjex/Global-City-Streaming) project. It is tightly integrated with the [Frontend Component](https://github.com/mrjex/Frontend-Global-City-Streaming), serving as a high-performance backend for city data retrieval.


## How It Works

When the container starts:

1. The Redis server starts as normal.
2. The entry point script checks if the `LOAD_STATIC_CITIES` environment variable is set to `true`.
3. If enabled, it looks for the `static-cities.json` file at `/data/static-cities.json`
4. If the file is found, the loader script imports the data into Redis.

## Data Structure in Redis

- Each city is stored as a Redis hash under the key `static_city:{city_name}`
- The list of all city names is stored as a Redis list under the key `static_cities:all`

## Integration & Usage

This Redis container is intended to be used as part of the Global City Streaming system, providing rapid city data access for the Frontend Component. To enable static city data loading, update your `docker-compose.yml` as follows in [Global City Streaming](https://github.com/mrjex/Global-City-Streaming):

1. Use the build context for the `redis` service:
   ```yaml
   redis:
     build:
       context: ./redis
   ```

2. Add the environment variable:
   ```yaml
   environment:
     - LOAD_STATIC_CITIES=true
   ```

3. Mount the static-cities.json file:
   ```yaml
   volumes:
     - redis_data:/data
     - ./dev-endpoints/static-cities-data/static-cities.json:/data/static-cities.json:ro
   ```
