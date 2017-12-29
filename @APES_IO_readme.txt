Samuli Launiainen (Luke) 13.1.2016
samuli.launiainen@luke.fi

**************************************************
APES -model input / output class.


All outputs are in Matlab Struct-variables, defined as below

oMeteo = 						
								%above-ground microclimatic profiles							
								%rows: 1=ground, end=upper boundary
								%columns: timesteps
								

       time: [96x4 single]		%[YYYY jday hh mm]
          U: [200x96 single]	%wind speed, m/s
          T: [200x96 single]	%degC
        H2O: [200x96 single]	%mol/mol
        CO2: [200x96 single]	%mol/mol
    WatStor: [200x96 single]	%stand layer water storage (kg H2O /m2 ground = mm)
         df: [200x96 single]	%stand layer dry fraction (-)
     Trfall: [200x96 single]	%Throughfall rate (mm/s) at each stand layer
	 
oRadi = 						%radiation-related outputs

        time: [96x4 single]		%[YYYY jday hh mm]
        f_sl: [200x96 single]	%sunlit fraction of foliage at each stand layer (-)
        SWb1: [200x96 single]	%direct PAR (Wm-2 ground) at layer
        SWd1: [200x96 single]	%diffuse PAR 
        SWu1: [200x96 single]	%upward-reflected PAR
        SWb2: [200x96 single]	%direct NIR (Wm-2 ground) at layer
        SWd2: [200x96 single]	%diffuse NIR (Wm-2 ground) at layer
        SWu2: [200x96 single]	%upward-reflected NIR
       LWnet: [200x96 single]	%net longwave budget (Wm-2 ground at layer)
        LWup: [200x96 single]	%upward longwave (Wm-2) at layer
        LWdn: [200x96 single]	%downward -"-
      LWleaf: [200x96 single]	%net longwave (Wm-2 leaf) at leaf surface in a layer
        alb1: [1x96 single]		%Stand-level PAR-albedo (-)
        alb2: [1x96 single]		%Stand-level NIR-albedo (-)
    alb_glob: [1x96 single]		%Stand-level broadband-albedo (for global radiation, alb1 and alb2 weighted for total incoming PAR & NIR, respectively (-)
       q_sl1: [200x96 single]	%absorbed PAR at sunlit leaf in a layer (Wm-2 leaf)
       q_sh1: [200x96 single]	%absorbed PAR at shaded leaf in a layer (Wm-2 leaf)
       q_sl2: [200x96 single]	%absorbed NIR at sunlit leaf in a layer (Wm-2 leaf)
       q_sh2: [200x96 single]	%absorbed NIR at shaded leaf in a layer (Wm-2 leaf)
    q_floor1: [1x96 single]		%absorbed PAR at surface (forest floor + bare ground), (Wm-2 ground)
    q_floor2: [1x96 single]		%absorbed NIR at surface (forest floor + bare ground), (Wm-2 ground)


oFlux = 						%flux profiles and sink/source profiles in the canopy air space
								%rows: 1=ground, end=top.
								%cols: timesteps
       time: [96x4 single]		
         Fc: [200x96 single]	%NEE (umol m-2 ground s-1); <0 is net CO2 sink
         LE: [200x96 single]	%latent heat flux (Wm-2 ground), >0 upwards
          E: [200x96 single]	%H2O flux (mol m-2 ground s-1)
          H: [200x96 single]	%sensible heat flux (Wm-2 ground), >0 upwards
          G: [1x96 single]		%heat conductive flux at soil surface (Wm-2 ground); >0 is towards the soil. If moss layer is included, this is below the moss layer.
    Csource: [200x96 single]	%CO2 sink/source profile (umol m-3 s-1), <0 is net sink
    Esource: [200x96 single]	%H2O source/sink profile (mol m-3 s-1)
    Hsource: [200x96 single]	%Heat source/sink profile (W m-3 = J m-3 s-1)
	Rsource: [200x96 double]	%Heat source/sink (W m-3) due to 'non-isothermal' component, i.e. radiative conductance term. Relates to linearization of energy balance.

oFloor = 						%outputs related to weighted fluxes and state variables at forest floor
								%cols: timesteps

        time: [96x4 single]
		   T: [1x96 single]		%forest floor temperature (deg C)
           H: [1x96 single]		%sensible heat flux (Wm-2 ground), >0 upward, from the forest floor (weighted based on moss-composition and bare soil)
          LE: [1x96 single]		%latent heat flux (Wm-2 ground), >0 upward, from the forest floor (weighted based on moss-composition and bare soil)
           G: [1x96 double]		%heat conductive flux at soil surface (Wm-2 ground); >0 is towards the soil. If moss layer is included, this is below the moss layer.
		  Fc: [1x96 single]		%Forest floor NEE (umol m-2 ground s-1); <0 is net CO2 sink
       Rsoil: [1x96 single]		%Soil respiration (umol m-2 ground s-1)
		  Ef: [1x96 single]		%Evaporation rate from forest floor (mol m-2 ground s-1), from all mosses & litter), excl. soil evap.
       Esoil: [1x96 single]		%soil evaporation rate (mol m-2 ground s-1)
    Trfall_s: [1x96 single]		%throughfall rate (mm/s = kg m-2 ground s-1) that is available for soil infiltration below the moss layer
	
oBryo = 						%BryophyteType -specific outputs
								%access fields as: oBryo(1).Mdry --> dry mass of Moss_1, oBryo(2).Mdry --> dry mass of Moss_2.
								%1st dimension of oBryo refers to BryophyteType -object
       time: [96x4 single]
       Mdry: [1x96 single]		%dry mass (g m-2 ground) of BryophyteType
        LAI: [1x96 single]		%LAI (m2m-2) 
    f_cover: [1x96 single]		%fractional cover (0...1), (-)
          T: [1x96 single]		%temperature (degC)
          W: [1x96 single]		%water content (g H2O / g dry mass)
      Theta: [1x96 single]		%volumetric water content (m3/m3), water retention curve (pF-curve) gives relationship between Theta and h
          h: [1x96 single]		%water potential (m). Note -1 m = -10 kPa
        GPP: [96x1 single]		%gross-primary productivity of specific moss object (umol m-2 ground  s-1); i.e. moss phosynthetic rate (umol m-2 ground s-1) * f_cover
      Respi: [1x96 single]		%autotrophic respiration  -"- (umol m-2 ground s-1), i.e. moss respiration rate (umol m-2 ground s-1) * f_cover
         Rn: [1x96 single]		%net radiation (Wm-2), >0 net gain by moss
      LWout: [1x96 single]		%upwelling long-wave radiation (Wm-2) from moss 
         LE: [1x96 single]		%latent heat flux (Wm-2) from moss 
          H: [1x96 single]		%sensible heat flux (Wm-2) from moss
          G: [1x96 single]		%heat conduction between moss and soil (Wm-2), <0 is downwards
         dQ: [1x96 single]		%change in moss energy storage (Wm-2) during timestep
      Fcapi: [1x96 single]		%capillary rise from soil to moss object (mm s-1).

	  
 oPlant = 							%PlantType-specific outputs
									%1st dimension refers to PlantType -object, i.e. oPlant(1).LAI is LAI of PlantType_1

			 time: [96x4 single]	
			  LAI: [1x96 single]	%leaf-area index (m2m-2) of PlantType, 1-sided (half-total)
			  WAI: [1x96 single]	%woody-area index (m2m-2)
			  lad: [200x96 single]	%leaf-area density (m2m-3)
			  wad: [200x96 single]	%woody-area density (m2m-3)
			  GPP: [1x96 single]	%gross-primary productivity of PlantType (umol m-2 ground  s-1);
			Respi: [1x96 single]	%above-ground autotrophic respiration of PlantType (umol m-2 ground  s-1).
			   Tr: [1x96 single]	%Transpiration rate (m s-1); multiply by 1000*dt to get in mm/timestep
RelPhoto_Seasonal: [1x96 single]	%Seasonal cycle modifier of photosynthetic capacity (-) ]0, 1]
		   h_root: [1x96 single]	%root-zone effective water potential (m), updated now at midnigt, Note -1 m = -10 kPa
		   h_leaf: [200x96 single]	%leaf water potential (m), assumed to be in hydrostatic equilibrium with h_root as h_leaf=hroot - z, Note -1 m = -10 kPa
		 RootSink: [33x96 single]	%root water sink distibution (s-1). row 1 = uppermost soil layer. sum(Rootsink.*dsz)=Tr  
		 
oStand = 							%outputs related to Stand-object

            time: [96x4 single]		
             LAI: [1x96 single]		%total LAI (m2m-2) of all PlantTypes
             WAI: [1x96 single]		%total WAI (m2m-2) of all PlantTypes
             lad: [200x96 single]	%total leaf-area density (m2m-3) of all PlantTypes
             wad: [200x96 single]	%total woody-area density (m2m-3) of all PlantTypes
    NrPlantTypes: [1x96 single]		%number of active PlantTypes
     NrBryoTypes: [1x96 single]		% -"- of active BryophyteTypes
     f_bryophyte: [1x96 single]		% total bryophyte coverage of the soil (-)
        f_litter: [1x96 single]		% coverage of litter layer
     Degree_days: [1x96 single]		%degree-day sum (degC)

oSoil = 							%Soil-related outputs (SoilProfile state and soil fluxes)
									%rows: soil layers, 1=uppermost, end =lowest
         time: [192x4 single]
            h: [33x192 single]		%soil water potential (m), -1m = -10kPa
         Wliq: [33x192 single]		%volumetric liquid water content (m3m-3)
         Wice: [33x192 single]		%volumetric ice content (m3m-3)
            T: [33x192 single]		%temperature (degC)
          KLh: [33x192 single]		%liquid hydraulic conductivity (ms-1)
       Ktherm: [33x192 single]		%thermal conductivity (Wm-1K-1)
          GWL: [1x192 single]		%Ground water level (m). if GWL below profile bottom, it is computed from h at lowest node assuming hydrost. equilibrium
         Fliq: [33x192 single]		%liquid phase vertical water flux (ms-1), <0 downwards  
         Fvap: [33x192 single]		%vapor phase vertical water flux (ms-1), <0 downwards 
        Fheat: [33x192 single]		%heat conduction (Wm-2), >0 downwards
        Infil: [1x192 single]		%infiltration to soil (mm) during dt
        Esoil: [1x192 single]		%evaporation from soil (mm) during dt
        Drain: [1x192 single]		%drainage (mm), to ditch and/or through lower boundary during dt
         Roff: [1x192 single]		%surface runoff (mm) during dt
       h_pond: [1x192 single]		%height of ponding water at the surface (m)
     RootSink: [33x192 single]		%root sink (s-1). Rootsink(1) includes potential capillary rise to mosses. >0 is sink; if <0 then either 'input' or hydraulic lift (depends if included in root uptake model)
    HorizFlux: [33x192 single]		%horizontal sink/source term (s-1), e.g. due to drainage to ditch.
         qtop: [1x192 single]		%upper boundary water flux (m s-1), balance of infiltration and evaporation, >0 is upward.
        Rsoil: [1x96 single]		%soil total respiration rate (umol m-2 s-1)
          MBE: [1x192 double]	 	%mass balance error of soil water budget (mm) during dt
	 
%******************OUTPUTS OF LEAF-SCALE FLUXES ETC.******************************

oDLF(1)						%dry-leaf fluxes for each PlantType (by LeafModule)

ans = 

    time: [96x4 single]
      sl: [1x1 struct]	%sunlit leaves
      sh: [1x1 struct]	%shaded leaves 
oDLF(1).sl					%rows: 1=ground, end=uppermost layer. Access fields as oDLF(1).sl.An --> photos. rate of sunlit leaves of PlantType_1

ans = 

       An: [200x96 single]	%photosynthetic rate (umol m-2 leaf s-1)
       Rd: [200x96 single]	%leaf dark respiration rate (umol m-2 leaf s-1)
        E: [200x96 single]	%leaf transpiration rate (mol m-2 leaf s-1)
        H: [200x96 single]	%leaf sensible heat flux (W m-2 leaf s-1)
    Tleaf: [200x96 single]	%leaf temperature (degC)
       Ci: [200x96 single]	%CO2 mixing ratio (ppm) inside the leaf (stomatal cavity) 
       Cs: [200x96 single]	%CO2 mixing ratio (ppm) at leaf surface 
      gsv: [200x96 single]	%stomatal conductance (mol m-2 leaf s-1) for H2O
      gbv: [200x96 single]	%leaf boundary layer conductance (mol m-2 leaf s-1) for H2O
       Fr: [200x96 double]	%leaf 'radiative heat flux', (W m-2 leaf s-1), related to radiative conductance and linearization of energy balance
	   
oWLF = 					%fluxes from 'wet' leaves; i.e. values that the leaves would have if they are wet (computed by WetLeafModule)

     time: [96x4 single]
       Rn: [200x96 single]	%leaf isothermal net radiation (Wm-2)
        H: [200x96 single]	%sensible heat flux rate (W m-2 leaf) from wet leaf
       LE: [200x96 single]	%latent heat flux rate (W m-2 leaf) from wet leaf
    Tleaf: [200x96 single]	%wet leaf temperature (degC)
    Inter: [200x96 single]	%interception at a layer (mm) during dt
     Evap: [200x96 single]	%evaporation at a condensation (mm) during dt 
      MBE: [1x96 single]	%mass-balance error (mm) during dt (of total water storage in foliage)
       Fr: [200x96 double]	%leaf 'radiative heat flux', (W m-2 leaf s-1), related to radiative conductance and linearization of energy balance   