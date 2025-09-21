# Part 1: Modern EO Processing Landscape

## The Challenge: EO Data Growth

Earth Observation is experiencing unprecedented data growth:

**The 4 V's of EO Big Data:**
- **Volume**: Petabytes of new satellite data daily
- **Velocity**: Near real-time processing requirements  
- **Variety**: Multiple sensors, formats, and resolutions
- **Veracity**: Quality assessment and validation needs

**Traditional vs. Cloud-Native Processing:**

| Traditional EO Processing | Modern Cloud-Native Processing |
|---------------------------|--------------------------------|
| Download → Store Locally → Process | Discover → Access in Cloud → Process at Scale |
| **Limitations:** | **Advantages:** |
| Storage constraints | Elastic computing resources |
| Processing bottlenecks | Co-located data and compute |
| Version control issues | Version-controlled workflows |
| Limited collaboration | Real-time collaboration |


```{figure} ../images/cloud-processing.png
:name: Traditional vs. Cloud-Native Processing
:width: 600px
Traditional vs. Cloud-Native Processing
```

**Data Cube Concept:**
A data cube organizes EO data along multiple dimensions:
- **X, Y**: Spatial coordinates (longitude, latitude)
- **Time**: Temporal dimension
- **Bands**: Spectral dimensions
- **Attributes**: Metadata and quality indicators

```{figure} ../images/Hyperspectral_image_data_cube.jpg
:name: Hyperspectral image 'data cube'
:width: 600px
Hyperspectral image 'data cube'. *Credit: University of Texas at Austin, Center for Space Research. Licensed under [ESA Standard Licence](https://www.esa.int/ESA_Multimedia/Terms_and_conditions_of_use_of_images_and_videos_available_on_the_esa_website).*
```

## 🎯 Demo Break: Real EO Workflow Example
*10 minutes*

**Concrete Example: Computing Cloud-Free NDVI for a Farm**

**Objective**: See why cloud-native approaches are essential through a realistic use case

**The Challenge** (8 minutes):

**Step 1: The Research Question** (2 minutes)
*"Monitor crop health for a 50km × 50km agricultural region over one growing season"*

**What you need:**
- **Area**: 50km × 50km region
- **Time**: 6-month growing season (April-September)
- **Goal**: Monthly cloud-free NDVI maps
- **Data source**: Sentinel-2 (10m resolution)

**Step 2: The Data Reality** (3 minutes)

**Single Sentinel-2 Scene Coverage:**
- **Tile size**: 100km × 100km (your 50km area needs 1 full tile)
- **File size per scene**: ~1GB (L2A product, all bands)
- **Cloud coverage**: 30-70% typical in Europe
- **Revisit frequency**: Every 5 days

**Monthly Composite Requirements:**
```
To get one cloud-free monthly map:
🌤️  Need 6-12 scenes per month (due to clouds)
📊  Data per month: 6 scenes × 1GB = 6GB
📅  6-month season: 6GB × 6 months = 36GB
☁️  Plus you need extra scenes for cloud backup
📈  Total realistic need: ~50GB for one small study area
```

**Step 3: Traditional vs Cloud-Native Workflow** (3 minutes)

**Traditional Approach:**
```
1. Download scenes: 50GB download (still significant with slow connections)
2. Local storage: Need 50GB+ free disk space
3. Cloud masking: Process each 1GB scene individually
4. Geometric correction: Align all scenes precisely
5. Temporal compositing: Load multiple scenes simultaneously
6. Calculate NDVI: Process final composite

Problems:
❌ Download time: Hours on slower connections
❌ Storage management: 50GB+ for one small study
❌ Memory: Multiple 1GB scenes still challenging
❌ Processing time: Hours of computation
❌ Workflow complexity: Many manual steps
```

**Cloud-Native Approach:**
```
1. Access data: Stream directly from cloud storage
2. Spatial subsetting: Only access your 50km area (~1/4 of each scene)
3. Temporal access: Access all time steps for specific pixels
4. Cloud masking: Applied automatically in cloud-optimized format
5. On-demand processing: Compute NDVI only when/where needed
6. Parallel execution: Process all months simultaneously

Benefits:
✅ No downloads: Access data like a database
✅ No local storage: Use only what you need
✅ Smart chunking: Process optimal-sized pieces
✅ Parallel processing: 6 months in parallel
✅ Cost efficiency: Pay only for computation used
```

## Discussion & Insights
*2 minutes*

**Real-World Questions**:
- How would your organization handle 400GB downloads for a simple study?
- What happens when you need to scale to larger regions or longer time periods?
- Which bottleneck would hit your workflow first: bandwidth, storage, or processing power?
