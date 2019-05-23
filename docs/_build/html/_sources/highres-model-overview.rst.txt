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

