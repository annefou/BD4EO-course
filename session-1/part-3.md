# Part 3: EOPF Sample Service Demonstration

## Introduction to EOPF

The Earth Observation Processing Framework (EOPF) represents ESA's next-generation approach to EO data processing:

**Key Innovations:**
- **Zarr format**: Cloud-optimized array storage
- **Harmonized data access**: Consistent APIs across missions
- **Scalable processing**: From laptop to HPC
- **Open source**: Community-driven development

**The Chunking Revolution:**
```python
import zarr
import xarray as xr

# Traditional approach: Load entire array into memory
# data = np.load('huge_satellite_image.npy') # Fails if larger than RAM!

# Zarr approach: Chunked, compressed arrays
ds = xr.open_zarr('https://eopf-sample/sentinel2-data.zarr')
print(f"Dataset shape: {ds.B04.shape}") # (time, y, x): (365, 10980, 10980)
print(f"Chunk structure: {ds.B04.chunks}") # ((30, 30, ...), (1024, 1024, ...), (1024, 1024, ...))
print(f"Memory per chunk: {ds.B04.chunksizes}") # Only load what you need!

# Efficient subset access - only loads relevant chunks
subset = ds.B04.sel(time='2023-06-15', 
                   x=slice(500000, 510000), 
                   y=slice(4500000, 4510000))
```

**Performance Benefits:**
- **Chunked storage**: Access only needed data pieces
- **Compression**: Reduced storage and transfer costs (often 50-90% reduction)
- **Parallel I/O**: Concurrent access to chunks across workers
- **Cloud-native**: Works efficiently with object storage (S3, etc.)
- **Lazy evaluation**: Build computation graphs, execute when needed

## 🎯 Demo Break: Explore EOPF Services
*15 minutes*

**Activity 1: EOPF Sample Notebooks - Focus on Core Concepts** (8 minutes)

**Instructions:**
1. Navigate to: https://eopf-sample-service.github.io/eopf-sample-notebooks/gallery/
2. Focus on these notebooks that demonstrate our key concepts:
   - **"Deforestation Monitoring using Sentinel 2"**: Shows chunking and Dask parallelization
   - **"Random Forest Based Land-Cover Mapping"**: Cloud-native workflow example

**Key Learning Points from Real EOPF Code:**

**Understanding Chunking with Real Examples:**
```python
# From "Deforestation Monitoring" notebook
r10m = xr.open_mfdataset(
    paths,
    engine="zarr",
    chunks={}, # Automatic chunking
    group="/measurements/reflectance/r10m",
    preprocess=extract_time
)
print(f"Dataset shape: {r10m.dims}")
print(f"Chunk structure: {r10m.chunks}")
# Output shows: (time: 109, y: 10980, x: 10980) with chunking
```

**Why Zarr is Relevant:**
```python
# From "Deforestation" notebook - efficient cloud access
bucket = "e05ab01a9d56408d82ac32d69a5aae2a:sample-data"
http_url = f"https://objects.eodc.eu/{bucket}/tutorial_data/..."

# Opens directly from cloud storage without downloading
ds = xr.open_dataset(http_url, engine='zarr', chunks="auto")
# Zarr enables: chunk-level access, compression, cloud-native storage
```

**Dask for Parallelization:**
```python
# From notebooks - processing large datasets
# Set up Dask cluster
cluster = LocalCluster()
client = cluster.get_client()

# Lazy operations build computation graphs
coarsened = r10m.coarsen(x=10, y=10, boundary="trim").mean()
ndvi = (nir - red) / (nir + red) # Still lazy!

# Parallel execution when needed
result = ndvi.compute() # Triggers parallel processing
```

**Cloud-Native Benefits You'll Observe:**
- **No downloads**: Data accessed directly from EODC cloud storage
- **Memory efficiency**: Only active chunks loaded into memory
- **Scalable processing**: Same code works on small or massive datasets
- **Standardization**: Consistent APIs across different satellite missions

**Activity 2: EOPF STAC Browser** (7 minutes)

**Instructions:**
1. Navigate to: https://stac.browser.user.eopf.eodc.eu
2. Try these explorations:
   - Use the interactive map to browse available datasets
   - Filter by temporal range for your area of interest
   - Examine metadata for different satellite missions
   - Follow links to access actual data

**Features to Highlight:**
- Visual data discovery interface
- Temporal filtering capabilities
- Rich metadata browsing
- Direct links to processing environments

**Try This:**
- Search for data over your area of interest
- Compare metadata between different satellite missions
- Note how the STAC interface standardizes discovery across different data types

## Share Your Discoveries
*2 minutes*

**Quick Insights:**
- What impressed you most about the EOPF approach?
- How does this compare to your current data access methods?
- Which chunking strategies seem most relevant to your work?
