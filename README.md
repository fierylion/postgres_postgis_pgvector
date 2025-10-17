# PostgreSQL + PostGIS + pgvector

A Docker setup for PostgreSQL with PostGIS (geospatial data) and pgvector (vector similarity search) extensions.

## Features

- **PostgreSQL 17.6** - Latest stable version
- **PostGIS 3.5.0** - Spatial and geographic objects for PostgreSQL
- **pgvector 0.8.1** - Open-source vector similarity search
- Pre-configured extensions and sample data
- Docker Compose setup for easy deployment

## Quick Start

### Using Docker Compose

```bash
# Clone the repository
git clone <your-repo-url>
cd postgres-postgis-pgvector

# Start the database
docker-compose up -d

# Connect to the database
psql -h localhost -p 5432 -U postgres -d postgres
```

### Manual Docker Build

```bash
# Build the image
docker build -f Dockerfile.pg17 -t postgres-postgis-pgvector:17 .

# Run the container
docker run -d \
  --name postgres-gis-vector \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=postgres \
  postgres-postgis-pgvector:17
```

## Extensions Included

### PostGIS
- `postgis` - Core PostGIS functionality
- `postgis_topology` - Topology support
- `fuzzystrmatch` - Fuzzy string matching
- `postgis_tiger_geocoder` - US address geocoding

### pgvector
- `vector` - Vector similarity search
- Supports cosine, L2, and inner product distances
- IVFFLAT and HNSW index types

## Sample Data

The database includes a sample table `exemplo_dados` with:
- Geographic points (Brazilian cities)
- Vector embeddings
- Proper spatial and vector indexes

### Example Queries

**Geospatial query:**
```sql
SELECT 
    nome,
    ST_AsText(localizacao) as coordenadas,
    ST_Distance(localizacao, ST_SetSRID(ST_MakePoint(-46.6333, -23.5505), 4326)) as distancia_sp
FROM exemplo_dados
ORDER BY distancia_sp;
```

**Vector similarity search:**
```sql
SELECT 
    nome,
    embedding <=> '[0.1, 0.2, 0.3]'::vector AS similarity
FROM exemplo_dados
ORDER BY similarity
LIMIT 3;
```

## Configuration

### Environment Variables

- `POSTGRES_DB` - Database name (default: `postgres`)
- `POSTGRES_USER` - Database user (default: `postgres`)
- `POSTGRES_PASSWORD` - Database password (default: `postgres`)

### Ports

- PostgreSQL: `5432` (host) → `5433` (container)

## Development

### Testing

```bash
# Run tests
docker-compose -f docker-compose.test.yml up --abort-on-container-exit
```

### Available Dockerfiles

- `Dockerfile.pg17` - PostgreSQL 17.6 build
- `Dockerfile.pg18` - PostgreSQL 18 build (if available)

## Use Cases

- **Geospatial Applications** - Store and query location data
- **Vector Search** - Similarity search for embeddings, recommendations
- **AI/ML Applications** - Vector databases for RAG, semantic search
- **GIS Systems** - Geographic information systems

## License

This project builds upon open-source components:
- PostgreSQL (PostgreSQL License)
- PostGIS (GPL v2)
- pgvector (PostgreSQL License)