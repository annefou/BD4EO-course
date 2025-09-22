# Part 2: FAIR Implementation Examples in Earth Observation 

:::{tip} Exercise
 
During the session-1, we learned that using STAC catalog makes it easier to find resources in Python. 
In small groups, explore and discuss the Findability of different STAC catalogs e.g. try to assess if the metadata and data have persistent identifiers (F1), metadata (F2), etc:

- CDSE STAC API: https://browser.stac.dataspace.copernicus.eu/?.language=en
- EOPF Sample Service STAC API: https://stac.browser.user.eopf.eodc.eu/?.language=en
- EODAG STAC API: https://radiantearth.github.io/stac-browser/#/external/hda.data.destination-earth.eu/stac
- ESA Open Science Catalog Example: https://opensciencedata.esa.int/workflows/delta-nbr-workflow-example/record
:::

## Example 1: EarthCODE Comprehensive Platform (7 minutes)

**Live Demo:** https://opensciencedata.esa.int/

ESA's EarthCODE represents the most advanced FAIR implementation in EO:

**Findable - Advanced Data Cataloging:**
- Open Science Data Catalogue with STAC-compliant metadata
- Searchable by themes, variables, projects, products, and EO missions
- Community-driven curation through GitHub pull requests

**Accessible - Multi-Platform Integration:**
- Single sign-on (SSO) across partner platforms
- Network of Resources (NoR) sponsorship for cloud computing
- Open source catalog with API access

**Interoperable - Standards-Based Architecture:**
- STAC metadata standards for data discovery
- Cloud-native formats: COG, Zarr for optimal performance
- CF conventions for climate and forecast data

**Reusable - Multi-Level Workflow Sharing:**
- Complete experimental packages with data, workflows, and documentation
- Cross-platform portability via GitHub
- Version control for full provenance tracking

## Example 2: SeasFire Cube Case Study (8 minutes)

**Live Navigation:** Find the SeasFire dataset in EarthCODE catalog

**FAIR Implementation Analysis:**
- **F1**: DOI assigned (10.5281/zenodo.13834057)
- **F2**: Rich metadata (temporal: 2001-2021, spatial: global, 8-day resolution)
- **A1**: Direct cloud access via HTTP/HTTPS
- **A1.1**: CC-BY-4.0 license, no authentication barriers
- **I1**: Zarr format, STAC JSON metadata
- **I2**: GCMD keywords, CF conventions
- **R1**: Complete methodology documentation
- **R1.2**: Full provenance from ESA Future EO-1 project

**Business Impact:**
- **Time Savings**: Researchers can find and access data in minutes, not weeks
- **Collaboration**: Multiple research groups can build on the same validated dataset
- **Innovation**: Enables rapid development of wildfire monitoring applications

## Example 3: Quick Look at Other ESA Services (5 minutes)

**Cubes & Clouds Educational Initiative:**
- DOI: 10.5281/zenodo.10869466
- Multiple access methods: web platform, GitHub, Zenodo
- Modular structure enables reuse of individual lessons

**EOPF Processing Framework:**
- STAC catalog: https://stac.browser.user.eopf.eodc.eu
- Open source with comprehensive documentation
- Standard APIs compatible with Python ecosystem

