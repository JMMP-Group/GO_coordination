# JMMP GO model development meeting minutes

*Thursday 17th September 2026*

*Participants:* 

Ana Aguiar (Chair), Isabella Ascione (IA), Adam Blaker (AB), Diego Bruciaferri (DB), Daley Calvert (DC), Margarita Choulga (MC), Catherine Guiavarc’h (CG), Emma Fiedler (EF), Sarah Keeley (SK), Matthew Martin (MM), Alex Megann (AM), Kristian Mogensen (CM), Charles Pelletier (CP), Oliver Tooth (OT), Kalli Furtado (KF), Matt Martin (MM).

*Apologies:* 

Ed Blockley (EB), David Schroeder (DSc), Davi Mignac (DM), Daniel Lea (DL), Dave Storkey (DS).


*The rota for minuting follows JMMP people surname in alphabetical order: IA, AB, DC, DB, EF, CG, AM, <ins>EB</ins>, DSc, DS, OT. (This was listed in the minutes of Nov 2024).  We only minute the quarterly meetings, not the monthly ones.*

----------

## Actions from last meeting:

  * (AB)  Check eddy viscosity files and share with DS.
  * (DS)  Test corrected eddy viscosity files (provided by AB) with the acceleration spin-up. 
  * (DC)  Cherry pick NEMO5.0.2 bug fixes into GOSI10 branch for GOSI10.beta.6 release. _Closed_
  * (CG)  Discuss rivers and iceberg with EF. _Closed_
  * (DC/AA) Share RK3/MLF results with Sybille. _Closed_

----------

## Agenda:
- Sea-ice dynamics updates in GOSI10.beta.5 and beta.6 and their unexpectedly large impact on AMOC.
- GOSI10.beta.6-ORCA12 development and stability testing. 
- Bathymetry and inland water body (lakes and closed seas) representation.  
- Modification and testing of the JRA-55 river runoff dataset.  


## Minutes:

#### Sea-Ice Dynamics Updates and AMOC Response (EF and CG) 

The GOSI10.beta.5 sea-ice updates included: 
- Rothrock ice-strength scheme replacing the Hibler scheme. 
- Transition from EVP to Adaptive EVP (aEVP) rheology. 
- Landfast-ice representation. 
- Additional tuning changes and bug fixes.  

Key Findings 
- The updates produced a much larger-than-expected increase in AMOC strength. 
- Individual changes (Landfast Ice, Rothrock strength, aEVP) produced only modest impacts. 
- The AMOC response emerges primarily when the changes are combined. 
- NEMO 5.0.2 and RK3 upgrades were ruled out as the primary cause.  

Working Hypothesis from analysis:
- Salinity increases in the Greenland-Iceland-Norwegian (GIN) Seas trigger deeper convection. 
- This strengthens AMOC through positive feedbacks involving heat and salt transport. 
- Labrador Sea changes appear to be a response to AMOC changes, rather than the initial trigger. 
- The system may be near an AMOC stability threshold where relatively small perturbations generate large responses.  

Agreed Next Steps: 
- Investigate the source of early salinity increases in the GIN Seas. 
- Analyse temperature and salinity changes throughout the water column. 
- Examine Fram Strait and East Greenland Current transports. 
- Calculate regional sea-ice budgets rather than Arctic-wide budgets. 
- Assess sensitivity to initial conditions and alternative start files.  

Release Decision:
The group agreed to proceed with the beta.6 release because no fundamental scientific error had been identified in the sea-ice updates.  

#### Closed Seas and Bathymetry Updates (KM, MC)

ORCA12 simulations experienced instabilities associated with: 
- Caspian Sea bathymetry. 
- Unrealistic salinity values. 
- Legacy closed-sea treatments inherited from earlier bathymetry datasets.  

ECMWF Contribution: KM and MC presented improved bathymetry products.
- Cleaned Caspian Sea bathymetry. 
- Improved Azov Sea bathymetry. 
- Updated Great Lakes bathymetry. 
- Removal of problematic hypersaline basins unsuitable for NEMO simulation. 
- Availability of high-resolution datasets (~1 km source resolution).  

Outcomes 
- ECMWF is willing to share the updated datasets. 
- The Met Office team plans to incorporate improved bathymetry into future GOSI10 developments. Dave Storkey will patch the bathymetry soon.
- AB suggested that Corrections may eventually be reported back to GEBCO.  

#### ORCA12 Experiments (IA) 

Testing: 
- Modified Leapfrog (MLF). 
- RK3 time-stepping. 
- Longer timesteps to improve efficiency.  

Two major issues affected experiments: 
- Negative ice-shelf melt values in runoff datasets. 
- Caspian Sea bathymetry instability.  

Current Status 
- Most experiments completed successfully once issues were mitigated. 
- RK3 at 15-minute timestep remained unstable. 
- A 12-minute RK3 test is running.  

Preliminary Conclusions 
- RK3 and MLF produce very similar scientific results. 
- RK3 appears capable of supporting longer timesteps for ORCA12.
- Computational performance is promising: 
   - MLF (~7.5 min timestep): ~35 min/month simulation. 
   - RK3 (10 min timestep): ~24 min/month simulation.  

#### River Runoff Forcing (JRA55) (CG)

CG tested replacing existing climatological runoff fields with JRA55-derived runoff forcing. See relevant issue for detailed findings.

Findings 
- Global freshwater budgets differ by less than 1%. 
- Significant local differences occur due to different river spreading methodologies. 
- Changes are evident in: 
   - Greenland freshwater distribution. 
   - Salinity fields. 
   - Mixed-layer depth patterns. 

Recommendation 
More testing is needed before any operational adoption.  
Continue investigating runoff spreading parameters and stability before considering inclusion in a future release. 

----------
 
## Actions 
- Investigate GIN Seas salinity source and AMOC trigger mechanism.  (Met Office)
- Analyse Fram Strait and East Greenland freshwater/ice transport. (Met Office) 
- Evaluate regional sea-ice budgets and transport diagnostics. (EF)  
- Proceed with GOSI10.beta.6-ORCA025 release. (IA)
- Obtain and assess ECMWF bathymetry products.  (DS)
- Continue RK3 timestep optimisation studies in ORCA12.  (Met Office)
- Further evaluate JRA55 runoff forcing and spreading methods. (CG)
