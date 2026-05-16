# AERMET Automation – User Guide

## Overview

The AERMET system is responsible for processing surface and upper-air meteorological data required for atmospheric dispersion modeling. The meteorological variables processed include temperature, cloud cover, pressure, dew point, wind speed, and wind direction. According to the US EPA AERMET user manual, the preprocessor is divided into three editable INP input files, which must be executed sequentially to generate the final meteorological outputs (`AERMET.PFL` and `AERMET.SFC`).

![AERMET Workflow](images/FluxogramaAERMET.png)

**Figure – AERMET workflow.** General workflow of the AERMET preprocessing system, including upper-air data, surface meteorological data, and generation of output files for AERMOD.

---

# INP Input File 1

The first editable INP file defines the radiosonde file used to estimate atmospheric turbulence and vertical profiles. For the case study, the file `FOZ.FSL` was used.

![Radiosonde Configuration](images/AERMETRadiossondagem.png)

**Figure – Radiosonde file configuration.** Configuration interface for defining upper-air meteorological data in AERMET.

The radiosonde dataset was obtained from the Integrated Global Radiosonde Archive (IGRA) provided by the National Oceanic and Atmospheric Administration (NOAA).

- Source: https://www.ncei.noaa.gov/data/integrated-global-radiosonde-archive

The selected station was located in Foz do Iguaçu (PR), approximately 155 km from the study area. Although this distance may not be suitable for formal regulatory applications, the dataset was used for research purposes to demonstrate the technical feasibility of file integration and workflow automation.

The `XDATES` keyword defines the extraction period for the meteorological data. The station code, coordinates, and local time conversion must also be configured.

![Radiosonde File](images/AERMETRadiossondagemFile.png)

**Figure – Radiosonde file.** Example of the FSL radiosonde file used for upper-air meteorological processing.

The FSL file is generated as a structured text file according to the desired simulation period.

---

# Surface Meteorological File

Another file required in the first INP configuration is `SAMSOM.SAM`, which contains hourly surface meteorological data.

![Surface Meteorological Configuration](images/AERMETMetSuperficie.png)

**Figure – Surface meteorological configuration.** Configuration of surface meteorological input files in AERMET.

The surface meteorological dataset includes:

- Solar radiation
- Cloud cover
- Dry bulb temperature
- Relative humidity
- Precipitation
- Wind speed and direction

The meteorological dataset in CSV format was obtained from the Brazilian National Institute of Meteorology (INMET).

- Source: https://bdmep.inmet.gov.br/

![INMET Meteorological File](images/AERMET_INMET.png)

**Figure – INMET meteorological file.** Example of the meteorological dataset obtained from INMET.

The CSV file is manually converted to the SAMSON format required by AERMET.

![SAMSOM File](images/AERMET_SAMSOM.png)

**Figure – SAMSOM.SAM file.** Example of the SAMSON-format meteorological file used in AERMET.

The SAM file structure requires strict formatting rules, including character positions, variable definitions, and missing value indicators. Details regarding each variable can be found in the WRPLOT View user manual.

- Source: https://www.weblakes.com/software/freeware/wrplot-view/

---

# Meteorological Data Conversions

Several meteorological variables require unit conversion before generating the SAM file.

## Global Horizontal Radiation

The global horizontal radiation must be converted from kilojoules per square meter to watts per hour per square meter:

```math
VLR = RHG / 3.6
```

Where:

- `VLR` = converted radiation value
- `RHG` = global horizontal radiation value

---

## Cloud Cover Estimation

Cloud cover can be estimated from relative humidity:

```math
CC = 1 - \sqrt{\frac{1 - UR/100}{1 - 0.7}}
```

Where:

- `CC` = cloud cover
- `UR` = relative humidity

---

## Precipitation Conversion

Precipitation values must be converted from millimeters to inches and hundredths:

```math
PP = (PM * 0.0393701) * 100
```

Where:

- `PP` = precipitation value in inches and hundredths
- `PM` = precipitation value in millimeters

---

## Wind Speed Conversion

Wind speed values from the MESONET database are converted from knots to meters per second:

```math
VM = VN * 0.5144
```

Where:

- `VN` = wind speed in knots
- `VM` = wind speed in meters per second

---

## Temperature Conversion

Temperature values must be converted from Fahrenheit to Celsius:

```math
C = (F - 32) / 1.8
```

Where:

- `C` = temperature in Celsius
- `F` = temperature in Fahrenheit

---

# INP Input File 2

The `AERMET_2.INP` configuration reads the processed data generated in the first stage and creates an intermediate file corresponding to the simulation period.

The `XDATES` keyword defines the period to be modeled.

---

# INP Input File 3

The `AERMET_3.INP` file reads the intermediate data and generates the final meteorological output files.

The following outputs are defined:

- `AERMET.SFC`
- `AERMET.PFL`

The modeling period must also be specified using the `XDATES` keyword.

---

# Surface Characteristics and Sectors

AERMET allows the domain to be divided into sectors to better represent land-use characteristics.

![Sector Definition](images/AERMET_SECTORS.png)

**Figure – Sector definition in AERMET.** Definition of sector divisions and surface characteristics in the AERMET_3.INP configuration.

The surface parameters include:

- Albedo
- Bowen ratio
- Surface roughness

The variation frequency can be configured as:

- Annual
- Seasonal
- Monthly

The sectors must cover the full 360° domain and are defined clockwise from north.

---

# AERMET Output Files

## PFL Output File

The `AERMET.PFL` file contains upper-air vertical profile data used by AERMOD.

![AERMET.PFL](images/AERMET_PFL.png)

**Figure – AERMET.PFL file.** Vertical profile output generated by AERMET.

---

## SFC Output File

The `AERMET.SFC` file contains processed surface meteorological data.

![AERMET.SFC](images/AERMET_SFC.png)

**Figure – AERMET.SFC file.** Surface meteorological output generated by AERMET.

---

# References

- USEPA. *AERMET User Guide*. United States Environmental Protection Agency, 2022.
- NOAA. *Integrated Global Radiosonde Archive (IGRA)*, 2022.
- INMET. *Brazilian National Institute of Meteorology Database*, 1992.
- WRPLOT View User Manual. Lakes Environmental Software, 2018.
- MESONET. *Meteorological Data Archive*, 2024.

