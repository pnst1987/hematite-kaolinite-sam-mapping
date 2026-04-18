# hematite-kaolinite-sam-mapping
hyperspectral mapping of hematite and kaolinite using SAM

# Classical Hyperspectral Mapping of Hematite and Kaolinite Using Spectral Angle Mapper (SAM)

**Author:** Peyman Namdarsehat  


## Overview

This repository presents a hyperspectral workflow for mapping **Hematite** and **Kaolinite** from airborne hyperspectral imagery using the **Spectral Angle Mapper (SAM)** method. The workflow is designed to compare pixel spectra with reference mineral spectra and generate mineral similarity maps for each target endmember.

## Data

The workflow uses an **AVIRIS hyperspectral dataset** provided in **ENVI format**, including the binary image file and its associated **`AVIRIS.HDR`** header file. The hyperspectral cube is read from the ENVI pair, while spectral wavelength information is extracted directly from the HDR metadata for band-wise analysis.

Reference mineral spectra are provided as external text files and resampled to the wavelength positions of the hyperspectral image before spectral matching. In the present implementation, the workflow uses the following mineral endmembers:

- **Hematite**
- **Kaolinite**

## Method

The implemented methodology consists of the following processing steps:

1. **ENVI hyperspectral data loading**  
   The hyperspectral cube is read from the ENVI binary image together with its associated `.HDR` file.

2. **Wavelength extraction from HDR metadata**  
   Spectral wavelength information is parsed from the ENVI header and used to define the spectral domain of the image cube.

3. **Data cleaning and preprocessing**  
   Invalid values are handled and negative reflectance values are removed to improve numerical stability during spectral analysis.

4. **Bad-band removal**  
   Spectral regions commonly affected by atmospheric absorption are excluded from the analysis in order to retain more reliable bands for mineral discrimination.

5. **Reference endmember loading**  
   Mineral reference spectra for **Hematite** and **Kaolinite** are read from text files.

6. **Spectral resampling**  
   The reference spectra are interpolated to match the wavelength positions of the hyperspectral image bands, ensuring direct spectral comparability between image pixels and endmembers.

7. **Spectral Angle Mapper (SAM) computation**  
   SAM is applied pixel-wise by calculating the spectral angle between each image spectrum and the corresponding mineral reference spectrum in multidimensional spectral space. Smaller spectral angles indicate stronger similarity between the pixel and the target mineral.

8. **Output generation**  
   The resulting mineral similarity maps are exported as:
   - **PNG** files for visualization
   - **GeoTIFF** files for georeferenced analysis and GIS integration

## Inputs

The workflow requires the following input files:

- ENVI header file (`.HDR`)
- ENVI binary hyperspectral image
- `Hematite.txt`
- `Kaolinite.txt`

## Outputs

The workflow generates the following products:

- `SAM_Hematite.png`
- `SAM_Hematite.tif`
- `SAM_Kaolinite.png`
- `SAM_Kaolinite.tif`

The GeoTIFF outputs preserve the spatial referencing of the original hyperspectral dataset.

## Usage

```bash
python hyperspectral_sam_mapper.py \
  --hdr AVIRIS.HDR \
  --binary AVIRIS \
  --kaolinite Kaolinite.txt \
  --hematite Hematite.txt
