APES_Jan2016

# APES (Atmosphere-Plant Exchange Simulator)

Samuli Launiainen. Metla/Luke 2011-2015.

To GitHub 29.12.2017

************************************************************************************************************************************
# Model description:

Launiainen, S., Katul, G.G., Kolari, P., and Lauren, A. 2015. Coupling boreal forest CO2, H2O and energy flows by 
a vertically structured forest canopy – soil model with separate bryophyte layer, 
Ecol. Mod., vol. 312,  p. 385–405, doi:10.1016/j.ecolmodel.2015.06.007.
 
http://www.sciencedirect.com/science/article/pii/S0304380015002562#

Abstract:

A 1-dimensional multi-layer, multi-species soil-vegetation-atmosphere transfer model APES(Atmosphere-Plant Exchange Simulator) with a separate moss layer at the forest floor was devel-oped and evaluated for a boreal Scots pine forest situated in Hyytiälä, Southern Finland. The APES is based on biophysical principles for up-scaling CO2, H2O, heat and momentum exchange from canopy element level to a stand scale. The functional descriptions of sub-models were parametrized by literature values, previous model approaches and leaf and moss gas exchange measurements, and stand structural characteristics derived from multi-scale measurements. The model was independently tested against eddy-covariance fluxes of CO2, H2O and sensible heat measured above and within the canopy, and against soil heat flux and temperature and moisture profiles. The model was shown to well reproduce fluxesand resulting scalar gradients at diurnal and seasonal timescales. Also predictions for moss moisturecontent and soil moisture and temperature dynamics were acceptable considering the heterogeneity in soil hydraulic and thermal properties and uncertainties in boundary conditions.The model framework allows for (1) coupling above-ground with the soil domains through the feed-backs between soil water and vegetation mediated by the moss layer, (2) several vascular plant species or cohorts in a multi-species canopy, and (3) explicit treatment of bryophyte layer energy and waterbalance and bottom layer – atmosphere exchange. These features make APES well-suited for exploring feedbacks between boreal forest structure, site conditions and vegetation processes controlling ecosystem-atmosphere exchange.

***********************************************************************************************************************************
Model application to explore the role of Leaf-area index (LAI) as a control of energy fluxes and surface conductance of boreal coniferous stands:

Launiainen, S., Katul, G. G., Kolari, P., Lindroth, A., Lohila, A., Aurela, M., Varlagin, A., Grelle, A. and Vesala, T. (2016), Do the energy fluxes and surface conductance of boreal coniferous forests in Europe scale with leaf area?. Glob Change Biol, 22: 4096–4113. doi:10.1111/gcb.13497

http://onlinelibrary.wiley.com/doi/10.1111/gcb.13497/abstract

Abstract:

Earth observing systems are now routinely used to infer leaf area index (LAI) given its significance in spatial aggregation of land surface fluxes. Whether LAI is an appropriate scaling parameter for daytime growing season energy budget, surface conductance (Gs), water- and light-use efficiency and surface–atmosphere coupling of European boreal coniferous forests was explored using eddy-covariance (EC) energy and CO2 fluxes. The observed scaling relations were then explained using a biophysical multilayer soil–vegetation–atmosphere transfer model as well as by a bulk Gs representation. The LAI variations significantly alter radiation regime, within-canopy microclimate, sink/source distributions of CO2, H2O and heat, and forest floor fluxes. The contribution of forest floor to ecosystem-scale energy exchange is shown to decrease asymptotically with increased LAI, as expected. Compared with other energy budget components, dry-canopy evapotranspiration (ET) was reasonably ‘conservative’ over the studied LAI range 0.5–7.0 m2 m−2. Both ET and Gs experienced a minimum in the LAI range 1–2 m2 m−2 caused by opposing nonproportional response of stomatally controlled transpiration and ‘free’ forest floor evaporation to changes in canopy density. The young forests had strongest coupling with the atmosphere while stomatal control of energy partitioning was strongest in relatively sparse (LAI ~2 m2 m−2) pine stands growing on mineral soils. The data analysis and model results suggest that LAI may be an effective scaling parameter for net radiation and its partitioning but only in sparse stands (LAI <3 m2 m−2). This finding emphasizes the significance of stand-replacing disturbances on the controls of surface energy exchange. In denser forests, any LAI dependency varies with physiological traits such as light-saturated water-use efficiency. The results suggest that incorporating species traits and site conditions are necessary when LAI is used in upscaling energy exchanges of boreal coniferous forests.
***********************************************************************************************************************************

# APES is a modular 1-dimensional multi-layer, multi-species SVAT -model developed especially for boreal forest ecosystems. 

It combines canopy SW (PAR & NIR) and LW transfer (including multiple scattering) in horizontally homogenous porous media, a 1st-order closure model for momentum, H2O, CO2 and T with leaf energy balance and gas-exchange. The combined solution of photosynthesis (A) and stomatal conductance (gs) is obtained by combining Farquhar -model where A = min[Av, Aj] and 'unified stomatal model' gs = g0 + 1.6 x (1 + g1/D^0.5) A/Cs (Medlyn et al. 2012, Global Change Biol.).

Multiple vascular PlantTypes can exist within the canopy, each characterized by different physiology or leaf-area density profiles.
Interception of rainfall is computed using multi-layer scheme based on Tanaka (2002, Ecol. Mod.)

The above-ground domain is linked with 1-D soil water and heat flow solution (Richard's and Fourier equations, respectively) and a bryophyte layer can exist at the forest floor and represents the water, heat and CO2 exchange of living mosses. 

Coded in Matlab using object oriented approach. Conceptually this is nice but construction is bit too rigid; therefore new version is being written in Python.

\Main\ --> initalize, main program, plotting scripts

folders starting with @ are classes; all functions in a such folder belong to the specific class
e.g. @PlantType 

folders starting with + are packages that contain thematically related functions.
e.g. +Micromet, +Radi

See readme-files at each folder and code for documentation. User-guide to be written... perhaps. 

To test the model: 
* unzip the file 
* open \Main\Initialize_APES.m 
* change line 9 to match the APES root folder: 

folder = 'D:\APES_Jan2016\'; % define folder where APES is
* add with sub-folders to Matlab path
* run Initialize_APES.m, it calls main program which saves results into a mat-file.
* run script PlotSomeResult.m to see some example output from the model. Also here, change the folder as above.
* see @APES_IO_readme for output variables and format

**************
Known issues:

* Solution of Richard's equation is numerically unstable in extremely dry conditions, potentially leading to model crash.
* Soil freezing is incorrect; know and will correct the bug.
* The iterative solution of energy budget and within-canopy scalar profiles may not converge during rainfall events. In this case profiles are assumed well-mixed and set equal to forcing values at uppermost gridpoint.

* Farquhar -model used is the single-limitation version, i.e. A = min[Av, Aj]. Compared to model versions that include co-limitation this one requires slightly lower Vcmax.

Code is being currently transformed into Python as part of 'pyAPES -package' so no major updates except bug corrections are done into Matlab-version.

**************


