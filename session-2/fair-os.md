# Session 2: FAIR Principles Through EarthCODE Exploration
## Interactive Workshop: Understanding FAIR Through Real Earth Observation Data

**Duration:** 60 minutes  
**Target Audience:** BD4EO Course participants  
**Prerequisites:** Basic understanding of Earth observation data  

---

## Learning Objectives

By the end of this session, you will:
- **Understand** FAIR principles through practical exploration of the EarthCODE Open Science Catalog
- **Navigate** and search the catalog effectively using multiple discovery methods
- **Access** real Earth observation datasets programmatically using STAC APIs
- **Evaluate** data quality, licensing, and reusability in practice
- **Connect** FAIR principles to your own research workflows

---

## Part 1: The FAIR Challenge (10 minutes)

### Scenario: You're a researcher studying wildfire impacts

Imagine you need to:
1. Find satellite data covering European wildfires from 2020-2021
2. Access burned area datasets with good temporal resolution
3. Understand the methodology and validation
4. Use the data in a reproducible workflow
5. Properly cite and attribute the data sources

**Without FAIR principles, this could take weeks of emails, website searching, and format conversions.**

Let's see how EarthCODE's FAIR implementation solves these challenges.

---

## Part 2: **F**indable - Discovering Data in the EarthCODE Catalog (15 minutes)

### Exercise 2.1: Theme-Based Discovery

1. **Navigate to the EarthCODE Open Science Catalog**
   - Go to: https://opensciencedata.esa.int/
   - Observe the six thematic research domains on the landing page

2. **Explore the "Land" theme**
   - Click on the "Land" theme
   - Note how many products are associated with this theme
   - Browse through some of the available products

3. **Find wildfire-related data**
   - Use the search functionality to look for "fire" or "burned"
   - Locate the "SeasFire Cube" dataset
   - Click through to examine its full metadata

**Discussion Questions:**
- How does the thematic organization make data more **Findable**?
- What metadata elements help you quickly assess dataset relevance?

### Exercise 2.2: Multi-faceted Search

1. **Advanced filtering**
   - Use the Search interface (https://opensciencedata.esa.int/search)
   - Apply multiple filters: Theme (Land), Variable (burned area), temporal range
   - Draw a spatial area of interest on the map

2. **Examine search results**
   - Note how results show temporal extent, spatial coverage
   - Compare different datasets' metadata quality
   - Look at the persistent identifiers (DOIs) assigned to datasets

**FAIR Principle in Action:**
- **F1**: Global unique identifiers (DOIs, URLs)
- **F2**: Rich metadata (spatial/temporal extent, variables, methodology)
- **F3**: Metadata clearly references data identifiers
- **F4**: Indexed in searchable catalog with multiple discovery paths

---

## Part 3: **A**ccessible - From Discovery to Data (10 minutes)

### Exercise 3.1: Understanding Access Patterns

1. **Examine the SeasFire Cube product page**
   - Look at the "Source" metadata section
   - Note the different access methods provided
   - Check the licensing information (CC-BY-4.0)

2. **Follow access links**
   - Click through to the external data repository
   - Examine how the data is stored (Zarr format on cloud storage)
   - Note the persistent storage location

### Exercise 3.2: Programmatic Access

Let's see how to access this data programmatically:

```python
# Example: Accessing EarthCODE catalog data via STAC
from pystac_client import Client
import xarray as xr

# Connect to EarthCODE catalog
catalog = Client.open("https://catalog.osc.earthcode.eox.at/")

# Search for wildfire data
search = catalog.search(
    collections=["seasfire-cube"],
    bbox=[-8.5, 41.5, -7.5, 42.0],  # Portugal region
    datetime="2020-01-01/2021-12-31"
)

# Access the data directly
items = list(search.items())
print(f"Found {len(items)} datasets")
```

**FAIR Principle in Action:**
- **A1**: Retrievable via standardized HTTP/HTTPS protocols
- **A1.1**: Open, free protocols (no proprietary software required)
- **A1.2**: Clear authentication procedures where needed
- **A2**: Metadata remains accessible even if data moves

---

## Part 4: **I**nteroperable - Standards in Action (15 minutes)

### Exercise 4.1: Examining Metadata Standards

1. **STAC Compliance**
   - Look at the raw STAC metadata for the SeasFire dataset
   - Identify standard fields: geometry, datetime, assets
   - Note the use of extensions (EO extension, OSC extension)

2. **Variable Standards**
   - Navigate to the Variables section of the catalog
   - Examine how "burned area" is defined
   - Look at the GCMD keyword connections

### Exercise 4.2: Cross-Platform Compatibility

1. **Multiple platform access**
   - Note how the same dataset appears in different platforms:
     - EarthCODE catalog
     - Zenodo repository
     - Direct cloud access
   - Examine how STAC enables this multi-platform approach

2. **Format Standards**
   - Look at the data formats used (Zarr, NetCDF, COG)
   - Discuss why these cloud-native formats improve interoperability

**Let's examine a real STAC item:**

```json
{
  "type": "Feature",
  "stac_version": "1.1.0",
  "id": "seasfire-cube-v.0.4",
  "geometry": {...},
  "properties": {
    "datetime": "2001-01-01T00:00:00Z/2021-12-31T23:59:59Z",
    "title": "SeasFire Cube v0.4",
    "description": "Global dataset for seasonal fire modeling..."
  },
  "assets": {
    "data": {
      "href": "s3://earthcode-bucket/seasfire/v0.4.zarr",
      "type": "application/x-zarr",
      "roles": ["data"]
    }
  }
}
```

**FAIR Principle in Action:**
- **I1**: Formal, shared language (STAC JSON, GeoJSON)
- **I2**: FAIR vocabularies (GCMD keywords, CF conventions)
- **I3**: Qualified references to related datasets and projects

---

## Part 5: **R**eusable - Complete Research Packages (10 minutes)

### Exercise 5.1: Documentation and Provenance

1. **Complete research context**
   - Examine how datasets link to their source projects
   - Look at associated workflows and experiments
   - Check the documentation links to scientific publications

2. **Licensing clarity**
   - Review the CC-BY-4.0 license
   - Understand attribution requirements
   - Note how to properly cite the dataset

### Exercise 5.2: Workflow Reusability

1. **Executable workflows**
   - Navigate to associated workflows in the catalog
   - See how Jupyter notebooks are linked and documented
   - Examine the environment specifications and dependencies

2. **Reproducibility information**
   - Look at the methodology documentation
   - Check processing parameters and algorithms used
   - Note validation and quality assessment information

**Example: Complete attribution chain**
```
Data: Alonso, L. et al. (2022). SeasFire Cube v0.4. Zenodo. https://doi.org/10.5281/zenodo.13834057
Project: ESA Future EO-1 Science for Society Call
Workflow: Available at https://github.com/deepesdl/seasfire-datacube
Processing: GWIS burned area algorithm, 8-day temporal resolution
```

**FAIR Principle in Action:**
- **R1**: Rich metadata with relevant attributes
- **R1.1**: Clear, accessible usage license
- **R1.2**: Detailed provenance information
- **R1.3**: Community standards compliance (CF conventions, STAC)

---

## Part 6: Putting It All Together - FAIR Assessment (10 minutes)

### Exercise 6.1: FAIR Scorecard

Using the SeasFire Cube as an example, rate each FAIR principle (1-5 scale):

| Principle | Score | Evidence |
|-----------|-------|----------|
| **Findable** | ___/5 | Multiple discovery paths, rich metadata, DOI |
| **Accessible** | ___/5 | Open license, multiple access methods, persistent storage |
| **Interoperable** | ___/5 | STAC compliance, standard formats, vocabulary alignment |
| **Reusable** | ___/5 | Complete documentation, clear license, linked workflows |

### Exercise 6.2: Comparison with Traditional Data Sharing

**Reflection Questions:**
1. How does this compare to typical data sharing in your field?
2. What would be different if you accessed this data via email request?
3. Which FAIR principle provides the most value for your research?
4. What challenges might you face implementing similar practices?

---

## Key Takeaways

### FAIR is Not Just Theory
- EarthCODE demonstrates FAIR principles in a working system
- Standards like STAC make data discovery and access scalable
- Proper metadata and documentation save researchers significant time

### The Network Effect
- FAIR data becomes more valuable when connected to other FAIR resources
- Cross-platform compatibility increases data reuse
- Community standards enable tool interoperability

### Your Role as a Researcher
- **As a data user**: Understand how to efficiently find and access FAIR data
- **As a data producer**: Learn how to make your own data FAIR
- **As a community member**: Contribute to standards and best practices

---

## Next Steps

### Immediate Actions
1. **Bookmark key resources:**
   - EarthCODE Open Science Catalog: https://opensciencedata.esa.int/
   - STAC specification: https://stacspec.org/
   - EarthCODE documentation: https://earthcode.esa.int/

2. **Practice data discovery:**
   - Search for datasets relevant to your research
   - Examine their FAIR compliance
   - Try programmatic access via STAC APIs

### Session 3 Preview
In the next session, we'll use the SeasFire dataset we explored today in a complete Pangeo workflow:
- Load data directly from the catalog
- Perform wildfire analysis using cloud-native tools
- Create reproducible, shareable results
- Publish findings back to EarthCODE

---

## Discussion and Q&A

**Questions for group discussion:**
1. What surprised you most about the EarthCODE catalog?
2. How could FAIR principles benefit your current research workflow?
3. What barriers do you see to implementing FAIR practices in your organization?
4. How does the EarthCODE approach compare to other data platforms you've used?

---

## Additional Resources

### FAIR Guidelines and Best Practices
- GO FAIR Initiative: https://www.go-fair.org/
- FAIR Principles: https://doi.org/10.1038/sdata.2016.18
- Research Data Alliance: https://rd-alliance.org/

### Technical Standards
- STAC Specification: https://stacspec.org/
- OGC Standards: https://www.ogc.org/standards
- CF Conventions: https://cfconventions.org/

### Community
- EarthCODE Discourse Forum: https://discourse.earthcode.esa.int/
- Pangeo Community: https://pangeo.io/
- Earth Science Information Partners: https://www.esipfed.org/
