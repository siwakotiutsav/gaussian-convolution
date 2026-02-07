# **README.md**

## **Gaussian Smoothing and Regridding Pipeline for JWST/MIRI–MRS Line Cubes**

This repository contains a Python workflow for smoothing JWST/MIRI–MRS line cubes using a wavelength‑dependent **Gaussian kernel**. The method follows the approach used in early JWST/MIRI studies, where each spectral slice is convolved to match the spatial resolution at the longest wavelength in the dataset.

The pipeline is designed for users who have already extracted **line subcubes** using CASA.  
If you need help generating those subcubes, see:  
**https://github.com/siwakotiutsav/casatools**

---

## **Overview**

This code performs the following operations on every `.fits` line cube in the input directory:

### **1. Gaussian PSF-Matching**
- Computes the expected MIRI PSF FWHM as a function of wavelength  
  \(\theta(\lambda) = 0.033\,\lambda + 0.106\) arcsec  
- Computes the target FWHM at the longest wavelength  
- Convolves each spectral slice with a 2D Gaussian kernel  
- Leaves slices unchanged if they already meet the target resolution

### **2. Spatial Regridding**
- Uses a reference cube (typically the longest‑wavelength line)  
- Ensures all smoothed cubes share the same spatial grid  
- Preserves WCS and header metadata accurately  
  (CASA often alters or drops important header keywords)

### **3. Masking**
- Applies a mask to remove NaNs  
- Ensures consistent spatial coverage across all cubes

### **4. Output**
- Writes each smoothed cube to the output directory  
- Appends `_smooth.fits` to filenames  
- Maintains original metadata and WCS structure

---

## **Before Running**

Update the **USER PARAMETERS** section in the script:

```python
IN_DIR       = "path/to/line_cubes"
OUT_DIR      = "path/to/output_folder"
REF_CUBE     = "path/to/reference_line_cube.fits"
TARGET_LAMBDA = 17.8846  # example
```

Make sure:
- `IN_DIR` contains CASA‑generated line subcubes  
- `REF_CUBE` is the longest‑wavelength line cube  
- `OUT_DIR` exists or can be created  
- The directory paths are correct  

If the paths are incorrect, the script will not find any files.

---

## **Usage**

Run the script:

```bash
python gaussian_convolution.py
```

All smoothed cubes will be written to the directory specified by `OUT_DIR`.

---

## **Scientific Context**

This Gaussian‑smoothing approach reproduces the method used in several early JWST/MIRI–MRS papers, where Gaussian kernels were used to approximate the wavelength‑dependent PSF.

If you use this workflow in a publication:

- Please cite the JWST/MIRI papers that introduced this smoothing method.  
- Acknowledging this repository is appreciated but not required.

Example acknowledgment:

> “We used a publicly available Gaussian PSF‑matching and regridding pipeline (Siwakoti 2025, github.com/siwakotiutsav/gaussian-convolution).”

---

## **Related Tools**

CASA‑based line‑cube extraction pipeline:  
**https://github.com/siwakotiutsav/casatools**

---
