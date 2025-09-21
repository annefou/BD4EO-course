# Part 2: Python Ecosystem for EO

## Core Python Libraries Overview

Python has become the lingua franca of EO processing through powerful libraries:

**Xarray: N-dimensional Labeled Arrays**
```python
import xarray as xr
# Open a NetCDF file with coordinate information
ds = xr.open_dataset('sentinel2_data.nc')
# Select data for specific time and location
subset = ds.sel(time='2023-06-15', 
                lon=slice(20, 25), 
                lat=slice(55, 60))
```

**Key Features:**
- Label-based indexing and selection
- Automatic alignment of datasets
- Integration with Pandas and NumPy
- Built-in visualization capabilities

**Dask: Parallel Computing**
```python
import dask.array as da
# Create chunked arrays for parallel processing
data = da.from_array(large_satellite_image, chunks=(1024, 1024))
# Lazy evaluation - computation happens when needed
result = data.mean(axis=0).compute()
```

**Benefits:**
- Process datasets larger than RAM
- Automatic parallelization
- Familiar NumPy-like interface
- Scales from laptops to clusters

**STAC: SpatioTemporal Asset Catalog**
```python
import pystac_client
# Connect to EarthCODE/EODC STAC catalog
catalog = pystac_client.Client.open("https://stac.eodc.eu/")
search = catalog.search(
    collections=["sentinel-2-l2a"],
    bbox=[23.5, 56.0, 24.5, 57.0],
    datetime="2023-06-01/2023-06-30"
)
```

**STAC Benefits:**
- Standardized metadata across providers
- Efficient discovery of relevant datasets
- Interoperable catalogs between platforms
- API-based access for automation

## 🎯 Demo Break: Understanding Chunking and Cloud-Native Formats
*12 minutes*

**Core Concepts Behind Cloud-Native EO Processing**

**Setup** (2 minutes):
- Open: https://colab.research.google.com/ (or any Python environment)
- Focus: Understanding why chunking and cloud-native formats matter

**Follow-Along Demo** (10 minutes):

**Step 1: The Chunking Revolution** (4 minutes)
```python
import xarray as xr
import numpy as np
import dask.array as da

# 1. Load tutorial dataset - observe memory usage
ds = xr.tutorial.open_dataset("air_temperature")
print(f"Dataset size: {ds.air.nbytes / 1e6:.1f} MB")
print(f"Data type: {type(ds.air.data)}") # numpy array - all in memory

# 2. Simulate larger EO dataset challenges
print("\n--- Simulating Large EO Dataset ---")
# Create synthetic "satellite" data like Sentinel-2
large_shape = (365, 1000, 1000) # 1 year, daily, 1000x1000 pixels
print(f"Simulated dataset size: {np.prod(large_shape) * 8 / 1e9:.1f} GB")
print("This would crash most laptops if loaded as numpy array!")

# 3. Chunked solution
chunked_data = da.random.random(large_shape, chunks=(30, 500, 500))
print(f"\nChunked data type: {type(chunked_data)}")
print(f"Chunk structure: {chunked_data.chunks}")
print(f"Memory per chunk: {30 * 500 * 500 * 8 / 1e6:.1f} MB") # Manageable!
```

**Visual: Data Chunking Concepts**

![Data Cube Chunking](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAwIiBoZWlnaHQ9IjMwMCIgdmlld0JveD0iMCAwIDYwMCAzMDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSI2MDAiIGhlaWdodD0iMzAwIiBmaWxsPSIjZjhmOWZhIi8+Cjx0ZXh0IHg9IjE1MCIgeT0iMzAiIGZvbnQtZmFtaWx5PSJBcmlhbCIgZm9udC1zaXplPSIxNiIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMzNzQxNTEiPkRhdGEgQ3ViZSB3aXRob3V0IGNodW5raW5nPC90ZXh0Pgo8dGV4dCB4PSI0NTAiIHk9IjMwIiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTYiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMzc0MTUxIj5EYXRhIEN1YmUgd2l0aCBjaHVua2luZzwvdGV4dD4KPCEtLSBMZWZ0IHNpZGU6IFNvbGlkIGN1YmUgLS0+CjxyZWN0IHg9IjUwIiB5PSI2MCIgd2lkdGg9IjIwMCIgaGVpZ2h0PSIxNTAiIGZpbGw9IiNlZjQ0NDQiIG9wYWNpdHk9IjAuNyIgc3Ryb2tlPSIjZGMxZTE4IiBzdHJva2Utd2lkdGg9IjIiLz4KPHN2ZyB4PSI1MCIgeT0iNjAiIHdpZHRoPSIyMDAiIGhlaWdodD0iMTUwIj4KICA8cGF0aCBkPSJNMjAgMjBMMTgwIDIwTDE4MCAzMEwxOTAgMzBMMTkwIDEzMEwzMCAxMzBMMzAgMTIwTDIwIDEyMFoiIGZpbGw9IiNkYzFlMTgiIG9wYWNpdHk9IjAuOCIvPgo8L3N2Zz4KPCEtLSBSaWdodCBzaWRlOiBDaHVua2VkIGN1YmUgLS0+CjxyZWN0IHg9IjM1MCIgeT0iNjAiIHdpZHRoPSIyMDAiIGhlaWdodD0iMTUwIiBmaWxsPSJub25lIiBzdHJva2U9IiNkYzFlMTgiIHN0cm9rZS13aWR0aD0iMiIvPgo8IS0tIENodW5rIGdyaWQgLS0+CjxnIHN0cm9rZT0iIzM3OWFiYiIgc3Ryb2tlLXdpZHRoPSIxIiBvcGFjaXR5PSIwLjciPgogIDxyZWN0IHg9IjM1MCIgeT0iNjAiIHdpZHRoPSI2NiIgaGVpZ2h0PSI1MCIgZmlsbD0iIzNmOGRiZiIgb3BhY2l0eT0iMC40Ii8+CiAgPHJlY3QgeD0iNDE2IiB5PSI2MCIgd2lkdGg9IjY3IiBoZWlnaHQ9IjUwIiBmaWxsPSIjM2Y4ZGJmIiBvcGFjaXR5PSIwLjQiLz4KICA8cmVjdCB4PSI0ODMiIHk9IjYwIiB3aWR0aD0iNjciIGhlaWdodD0iNTAiIGZpbGw9IiMzZjhkYmYiIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjM1MCIgeT0iMTEwIiB3aWR0aD0iNjYiIGhlaWdodD0iNTAiIGZpbGw9IiNlZjQ0NDQiIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjQxNiIgeT0iMTEwIiB3aWR0aD0iNjciIGhlaWdodD0iNTAiIGZpbGw9IiNlZjQ0NDQiIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjQ4MyIgeT0iMTEwIiB3aWR0aD0iNjciIGhlaWdodD0iNTAiIGZpbGw9IiNlZjQ0NDQiIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjM1MCIgeT0iMTYwIiB3aWR0aD0iNjYiIGhlaWdodD0iNTAiIGZpbGw9IiNmZWY5ZTciIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjQxNiIgeT0iMTYwIiB3aWR0aD0iNjciIGhlaWdodD0iNTAiIGZpbGw9IiNmZWY5ZTciIG9wYWNpdHk9IjAuNCIvPgogIDxyZWN0IHg9IjQ4MyIgeT0iMTYwIiB3aWR0aD0iNjciIGhlaWdodD0iNTAiIGZpbGw9IiNmZWY5ZTciIG9wYWNpdHk9IjAuNCIvPgo8L2c+CjwhLS0gTGVnZW5kIC0tPgo8dGV4dCB4PSIzNTAiIHk9IjI0MCIgZm9udC1mYW1pbHk9IkFyaWFsIiBmb250LXNpemU9IjEyIiBmaWxsPSIjMzc0MTUxIj5TcGF0aWFsIGNodW5raW5nOiB0aW1lIGxheWVycyDinIU8L3RleHQ+Cjx0ZXh0IHg9IjM1MCIgeT0iMjU1IiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTIiIGZpbGw9IiMzNzQxNTEiPlRlbXBvcmFsIGNodW5raW5nOiBzcGF0aWFsIGJsb2NrcyDinIU8L3RleHQ+Cjx0ZXh0IHg9IjM1MCIgeT0iMjcwIiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTIiIGZpbGw9IiMzNzQxNTEiPkJveCBjaHVua2luZzogYm90aCBkaW1lbnNpb25zIOKchTwvdGV4dD4KPC9zdmc+)

**Step 2: Lazy Evaluation Benefits** (3 minutes)
```python
# 4. Demonstrate lazy evaluation
print("\n--- Lazy Operations (No Computation Yet) ---")
chunked_ds = ds.chunk({'time': 10, 'lat': 25, 'lon': 53})
# These operations are lazy - build computation graph
seasonal_mean = chunked_ds.air.groupby('time.season').mean()
anomaly = chunked_ds.air - chunked_ds.air.mean(dim='time')
print("Operations defined but not computed!")
print(f"Task graph size: {seasonal_mean.data.npartitions} chunks")

# 5. Parallel execution
print("\n--- Parallel Execution ---")
import time
start = time.time()
result = seasonal_mean.compute() # Now it runs in parallel!
end = time.time()
print(f"Computed in {end-start:.2f} seconds using all CPU cores")
```

**Step 3: Cloud-Native Storage Benefits** (3 minutes)
```python
# 6. Cloud-native format advantages
print("\n--- Why Cloud-Native Formats Matter ---")
print("Traditional approach:")
print(" - Download entire 100GB satellite image")
print(" - Load into memory")
print(" - Process subset you actually need")
print("\nCloud-native approach (Zarr, COG):")
print(" - Access only the chunks you need")
print(" - Stream directly from cloud storage")
print(" - Process in parallel across chunks")
print(" - 10-100x faster for typical analysis!")

# 7. Real-world impact
print("\n--- Real EO Processing Impact ---")
print("Traditional: 2 hours to download + process Sentinel-2 scene")
print("Cloud-native: 2 minutes to process same analysis")
print("Scalability: Same code works on 1 scene or 10,000 scenes")
```

## Q&A: What Did You Discover?
*3 minutes*

**Key Concepts Participants Should Understand:**
1. **Memory Efficiency**: Chunking enables processing datasets larger than RAM
2. **Parallel Processing**: Dask automatically uses all CPU cores  
3. **Lazy Evaluation**: Build complex operations without immediate computation
4. **Cloud-Native**: Stream only needed data chunks from remote storage
5. **Scalability**: Same patterns work from laptop to supercomputer

**Questions for Understanding:**
- "How does chunking solve the 'big data' problem?"
- "Why is lazy evaluation important for satellite data analysis?"
- "What makes cloud-native formats different from traditional file formats?"
