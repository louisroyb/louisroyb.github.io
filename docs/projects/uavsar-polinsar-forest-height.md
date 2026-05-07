# Forest Height Estimation Using UAVSAR PolInSAR Data

![Forest height map of Pongara National Park, Gabon, derived from UAVSAR PolInSAR data](../assets/images/SAR.png)

## Overview

Coursework assessment for *Synthetic Aperture Radar in Forest Application* at the Technical University of Munich, applying polarimetric SAR interferometry (PolInSAR) and the random volume over ground (RVoG) model to NASA UAVSAR data over Pongara National Park, Gabon to derive a forest canopy height map. The analysis used the open-source Kapok Python library and processed a representative subset of the AfriSAR 2016 campaign data.

**Study Area:** Pongara National Park, Gabon (mangrove and inland forest)
**Duration:** October 2024 – March 2025
**Role:** Solo project (course assessment)
**Status:** Completed

---

## Methods & Tools

**Data Sources**

- NASA UAVSAR L-band repeat-pass acquisitions, AfriSAR 2016 campaign — tracks 0 and 1 of the Pongara stack
- Auxiliary geometry (vertical wavenumber, incidence angle, DEM) distributed with the UAVSAR SLC stack

**Processing Steps**

1. Subset the Pongara stack to a tractable area of interest (`azbounds=[15000, 25000]`, `rngbounds=[1000, 5000]`) and select tracks 0 and 1 from the five available.
2. Multi-look the SLC stack with a (20, 5) window in azimuth and slant range to suppress speckle.
3. Compute the polarimetric coherency matrix and run coherence optimization to separate canopy volume scattering from ground scattering.
4. Invert the random volume over ground (RVoG) model on the optimized coherences to estimate forest canopy height per pixel.
5. Export results to HDF5 and visualize coherence magnitude and phase, Pauli RGB, complex coherence, the DEM, and the final canopy height map.

**Tools Used**

| Tool | Purpose |
|------|---------|
| Kapok | Open-source PolInSAR forest height estimation library (Denbina & Simard, JPL/Caltech) |
| Python (Jupyter Notebook) | Interactive development with incremental debugging on small code chunks |
| h5py | Reading and inspecting HDF5 outputs (covariance, kz, lat/lon, optimized coherence) |
| RVoG model | Physical scattering model relating interferometric coherence to canopy height |

---

## Key Findings

- Derived a forest canopy height map for the Pongara mangrove subset, with retrieved heights distributed up to roughly 70 m and clear structural discrimination of tidal channel networks against dense canopy.
- Coherence magnitude and phase rasters showed the expected pattern: high, vibrant coherence over stable mangrove canopy, and degraded, desaturated coherence along water channels and inundated zones where decorrelation dominates.
- Pauli RGB and complex coherence imagery confirmed separation of scattering mechanisms between canopy volume and ground/water surfaces, supporting the validity of the RVoG inversion inputs.
- Demonstrated that UAVSAR PolInSAR is a credible ground-truthing complement to spaceborne missions such as BIOMASS, NISAR, and GEDI for mangrove and tropical forest height monitoring.
- Compute constraint: the full Pongara stack (~237 GB) exceeded local processing capacity, so the analysis was intentionally scoped to a ~10,000 × 4,000 pixel window. Scaling this workflow to basin-wide, long-term monitoring of biomass or deforestation would require HPC or cloud-based processing.

---

## Links

[View Code on GitHub](https://github.com/simard-landscape-lab/kapok){ .md-button }
[NASA UAVSAR Data Portal](https://uavsar.jpl.nasa.gov/){ .md-button }
[AfriSAR Campaign (NASA Earthdata)](https://www.earthdata.nasa.gov/data/catalog/ornl-cloud-polinsar-canopy-height-1589-1){ .md-button }
