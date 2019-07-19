======================
highRES model overview
======================

The high spatial and temporal resolution electricity system model (highRES) minimises power system costs to meet hourly demand subject to a number of technical constraints. The core focus of highRES is a detailed representation of variable renewable energy technologies (VREs). We model VRE generation at a grid cell level of 0.5°x0.5° (about 50km x35 km). The VRE component of highRES can be run in two modes: either at the grid cell level (Figure 1) or with the cells aggregated to the zones, which are based on those used in the National Grid’s Seven Year Statements10 (NG SYS, see Figure 2), prior to execution. In both cases supply-demand balancing occurs at the zonal level via the transmission grid but in the former case the model is able to deploy VRE capacity in whichever cells within a zone are optimal. In the latter case, an hourly zonal average capacity factor for each technology is computed from all cells in that zone before running the model. Only cells with annual capacity factors above a certain threshold are included in this average. In this study we exclusively run the model in its aggregated VRE mode. HighRES finds the optimal location for generation capacities as well as the optimal capacities and locations of VRE integration options. These are transmission grid extension, flexible generation (modelled based on an open cycle gas turbine) and electricity storage. 
The model is implemented in the General Algebraic Modeling System (GAMS)11 and solved using the solver CPLEX.

[Figure 1]

Mathematical description
========================

Indices
-------

Variables
---------

Parameters
----------

Objective function
------------------

Balancing equation
------------------

VRE equations
-------------

Non-VRE equations
-----------------

Transmission system equations
-----------------------------

Storage equations
-----------------

Features added to highRES (-> SPOT-RES/highRES_v2 model?)
=========================================================

Frequency response/reserve
--------------------------

Hydrogen storage(?)
-------------------

Variable renewable energy generation
====================================

Mapping resource potentials using GIS
-------------------------------------
Rooftop PV
We use the CORINE land cover map14 and assume that rooftop PV can be deployed in the following areas: "continuous urban fabric", "discontinuous urban fabric" and "industrial and commercial units". These three categories give us the total building area. We derive the usable PV rooftop area using the same assumptions as in a study for London15: Proportions of buildings facing approximately South, East, West and North are equally distributed. 20% of domestic and 80% of non- domestic roofs are flat. For pitched roofs we assume a ratio of footprint area to roof area of 1.2 and for flat roofs of 115. We assume that PV is only installed on South facing roofs.

Ground mounted PV
The UK Department for Communities & Local Government16 states that for greenfield developments preference should be given to poorer quality agricultural land. Using GIS datasets on agricultural land classification for England17, Scotland18 and Wales19 we exclude grade 1-3 for England and Wales and grades 1-5 for Scotland as they are of good agricultural quality. We use the OS Terrain 50 DTM20 dataset to calculate the slope and for technical exclude all areas steeper than 15 degrees21. We use the World Database on Protected Areas22 to exclude all protected areas. Supplementary Figure 3 shows all areas exclude from ground mounted PV development.
   
Figure 3 - Areas (in grey) excluded from ground-mounted PV development, from left to right: slope greater than 15degrees, good agricultural land, environmentally protected areas

Onshore wind
For Great Britain, there is no minimum distance to settlements or other rules set by the government defining where wind energy can be deployed. Wind farms under 50MW are decided on by the relevant local planning authority23. Some authorities have implemented minimum distances between residential housing and wind turbines (e.g. 800 metres by Allerdale Borough Council Local Plan)23. Other scientific studies use distances between 500 – 1500 metres24–27. We apply a buffer of 800 metres around all buildings (residential, commercial and industrial). Similar to other studies we exclude areas which are steeper than 15%25–27 using the OS Terrain 50 DTM dataset20 and nature protected areas24–27 using the World Database on Protected Areas22. We apply a buffer of 150 metres around highways according to the Highways Agency28 and 150 metres around the railway network using the Meridian 2 dataset29. Other studies use a distance between 150- 200 metres 24,26,27. We implement a buffer of 5 kilometres around airports30 using the Strategi Dataset31. Other studies use 2500-5000 metres24–26. Supplementary Figure 4 shows all areas excluded from onshore wind development.
    
Figure 4 - Areas (in grey) excluded from onshore wind development, from left to right: slope greater than 15degrees, environmentally protected areas, buildings infrastructure (train, airports, highways)

Offshore wind
We attribute each of the offshore grid cells to the closest onshore zone using the centre of each grid cell (Supplementary Figure 5). 
 
Figure 5 - Allocation of offshore grid cells to closest zones

The crown estate has rights for leasing renewable energy projects within UK waters. We use their GIS data32 to determine the location of constructed and planned wind turbines32 in each of the grid cells. Using gridded bathymetric data with a spatial resolution of 30 arc-seconds33 we determine the maximum depth of all wind projects in each of the grid cells. The project with the highest water depths are the Hywind demonstration project (project status: currently awaiting consent) with a maximum depth of 118 metres, Firth of Forth (status: area of search) at 85 metres and Hornsea at 70 metres depth. The final investment decision on Hornsea was recently taken by Dong Energy so we conclude that a depth of 70 metres is technically feasible. We thus include all areas within the boundary of the UK continental shelf and with a maximum depth of 70 metres as well as the areas covered by Hywind and Firth of Forth. 6 shows all areas excluded from offshore wind deployment.
 
Figure 6 - Areas (in grey) excluded from offshore wind development: areas deeper than 70 metres with the exception of the Hywind and Firth of Forth projects

Renewable energy generation time series
---------------------------------------
To capture the spatial and temporal variability of VRE production and simultaneously model its interaction with storage, the transmission system and conventional generation at large scales (i.e. at the national or continental level), time series data with sufficiently high spatial and temporal detail and coverage are needed. “Sufficient” means trading off a number of important factors. On the one hand the data should have as high a resolution as possible to approximate the minute by minute and kilometre by kilometre spatiotemporal variability of weather, and hence VRE production, and the resultant impacts on the short term operational dynamics of the power system. On the other hand such time series should cover the entire geographical area of the region being modelled, in a uniform and homogenous manner, over a period of time that is long enough to capture the inter-annual variability of weather and assess system stability. Furthermore, both of these features are constrained by what data there is currently available and computational restrictions. For this purpose, existing renewable power production time series are of limited use because they are often too short, unavailable for locations without current installations and technology specific. Similarly, weather station data often have non-uniform spatial coverage combined with data quality issues. Here, like a number of recent studies13,34–36, we opt to use state of the art global climate reanalysis and satellite based data, both of which provide a suitable balance between temporal and spatial resolution while simultaneously maintaining broad, homogenous temporal and spatial coverage. Using these time series we are then able to simulate multiple years of output from various renewable technologies on a regular grid and as such better capture the temporal and spatial variability of VRE generation over many years and its interaction with the rest of the power system.

Photovoltaic energy
Input weather observations for our solar PV time series are taken from the Satellite Application Facility on Climate Monitoring’s (CMSAF) Surface Solar Radiation Data Set – Heliosat (SARAH)37 . This dataset is based on observations taken by the Meteosat First and Second Generation satellites between 1983-2013. CMSAF process this data and provide hourly, 0.05°x0.05° gridded surface incoming solar (SIS) radiation, which is the irradiance reaching a horizontal plane at the Earth’s surface and thus is taken here to be global horizontal irradiance (GHI), and direct normal irradiance (DNI), which is the irradiance at the surface on a plane normal to the direction of the sun. They also validate these time series against ground based measurements (see SARAH validation report37).
We extracted data for 2001-2010 from the CMSAF database and re-gridded it to match the resolution of our wind data. Occasional hourly and daily gaps (which we define as days with greater than 5 missing hours) in the time series were filled using simple linear interpolation in the former case and randomly selecting substitute days from the same month and year in the latter case. Such gaps are limited to the years 2006-2010. Of those years, all have > 95.2% of hours (the missing hours include those when the sun is down) and > 95% of days (again, including days which contain 6 or more missing hours some of which may be at night) before data gaps are filled.
Next we use the Python module PV_LIB38 to compute on module irradiance at each grid point assuming a south facing array at 45° tilt. To obtain PV panel output from the irradiance data we use the model of ref39 and assume a Crystalline Silicon module with 15% efficiency. Their model takes into account the impact of module temperature on panel performance. We use hourly 2m air temperature from CFSR as input to the model which in turn converts these to an estimate of module temperature. The PV generation data is presented to highRES as hourly capacity factors.

Wind energy
NCEP CFSR (National Centre for Climate Prediction Climate Forecast System Reanalysis)40 provides a global time series for a range of climate variables, on a gridded basis, at a number of different altitudes. Like all of the latest generation of climate reanalyses, this utilises a core of conventional data, including wind speed, temperature, moisture and air pressure, as well as other data including precipitation. Data sources change, due to new technologies being introduced; current platforms include, but are not limited to, radiosonde, satellite, buoys, aircraft and ship reports41. Data are run through a Global Circulation Model (GCM) in hindsight. The convention is to produce an analysis of the GCM every 6 hours; NCEP CFSR provide forecasts from these 6-hourly analyses at an hourly resolution. This study uses NCEP – CFSR v1, which provides data from 1979 – 2010 at a resolution of 0.5o x 0.5o (data are provided as grid point values in the centre of each grid square).
Wind speed at hub height was converted to power using manufacturer wind turbine curves, assuming that the capacity within each grid square experiences the conditions represented by the grid point value. Onshore the Nordex N80 2.5MW turbine is assumed to be representative of capacity 80 m, offshore the Siemens SWT 107 3.6MW. CFSR wind speed data are provided at 10 m above the surface, whereas turbine curves represent the relationship at hub height (onshore assumed to be 80 m, offshore 100 m). The wind profile law was used to interpolate wind speed, which follows a power law and is sometimes referred to as the Hellman equation. This law encapsulates the atmospheric and surface factors which affect wind into a single exponent. Here value of 1/7 (~ 0.142) is used onshore, to represent neutral stability conditions, which results in reasonable conversion over large areas42 and 1/9 (0.1’) offshore43 These values can lead to conservative estimates of wind speed44, particularly at higher altitudes45,46. Other methods exist which may improve the accuracy of the method, particularly where other atmospheric data are available, however in this case the method was deemed to be suitable given the uncertainty surrounding how wind speed changes across each grid square. The wind generation data is presented to highRES as hourly capacity factors.

Correlation between weather and electricity demand
==================================================
As a demonstration of how weather and electricity demand correlate, in Figure 7 we plot national electricity demand anomaly against the anomalies of three weather variables. These weather anomalies are derived from the same reanalysis data used to drive our VRE production. The raw reanalysis data are aggregated to a national-level hourly average based on population weighting and cover the period 2002-2010 inclusive (to match our demand data). To determine the hourly correlation between each weather variable and demand we first convert the data into anomalies. That is, we derive a 9 yearly average for each hour of the year and then subtract this off the hours in each year. This removes the seasonal and diurnal cycles. In general we see essentially no correlation between demand and wind speed or solar irradiance but a weak anti-correlation with temperature. As a result of this analysis, we run the model using the respective demand year to the weather year.
 
Figure 7 - Correlation between wind speed, solar irradiance and temperature (from left to right) and electricity demand

Electricity demand
==================
In highRES we model the electricity system of Great Britain. UKTM models the whole energy system of the UK. North-Ireland's share of electricity demand amounted to 2.6% of total UK demand in 201147. We deduct 2.6% from the UKTM annual electricity demand for 2050 (516,882GWh). This gives us a total GB electricity demand for 2050 of 503,443 GWh. We distribute this demand across the 20 GB NG SYS zones according to ref12 but adjust this for zones Z1_1 and zone Z1_2 based on more up to date data. The demand shares per zones which we use can be seen in Table 1.

Table 1 - Demand shares per zone

Historical, measured half-hourly demand data from the National Grid are available from 2002-2010. We scale the hourly demand load profile from the National Grid48 for each year from 2001-2010 (using 2002 to represent 2001 demand) by the total annual demand of each UKTM scenario. This gives us an estimate of hourly electricity demand for 2050.

Transmission grid
=================
As discussed previously, we use the zones based on the National Grid Seven Year Statement10 with a minor adjustment, Z1 is split into 4 sub zones (Z1_1 to Z1_4) as used by ref13. We aggregate the high voltage transmission system into a more simplified version connecting NG SYS zones. To do so we use the line capacities from the system technical data which is an appendix to the 10 year grid statement49. The system technical data specifies line capacities for each season. Summer has the lowest line capacity (20% lower compared to the winter rating). We aggregate all lines crossing a zonal boarder using line capacities for 2015 and assume lines connect to the centroid of each zone. In the UK the security standard requires the electricity transmission network to withstand a loss of two circuits (n-2 rule) without causing overloads of any other circuit and such outages must not threaten the integrity of system operation50. In the SWISSMOD51 and ELMOD52 model a security margin of 25% and 20% line capacity unused in each line is assumed, i.e. calibrating lines to 75% and 80% of line capacity respectively. This is not an exact parametrisation of the security standard but serves to better represent real system operation in the model51. Supplementary Figure 8 shows the aggregated line capacity for the summer rating reduced by 25% to approximate the security standard. Both of the links Z1_1 and Z1_2 to Z1_4 are set according to ref13 but we derated the lines to approximate the n-2 security constraint. Ref53 uses the winter rating in their representative GB network model which is less restrictive. Other studies13,27,54,55 do not represent grid security standards. Further, we assume 1% transmission losses per 100km55. Supplementary Table 2 shows capacities (75% of summer rating) and distances between the centroid of each zone as used in the model13
