# JMMP GO model development meeting minutes

*Thursday 15th May 2025*

*Participants:* Ana Aguiar (AA - chair), Isabella Ascione (IA - minutes), Mike Bell (MB), Adam Blaker (AB - minutes), Daley Calvert (DC), Emma Fiedler (EF), Catherine Guiavarc'h (CG), Alex Megann (AM), Kristian Mogensen (KM), Richard Renshaw (RR), David Schroeder (DSc), Dave Storkey (DS), Oliver Tooth (OT), Amber Walsh (AW), Sarah Keeley (SK), Charles Pelletier (CP)

*Apologies:* Diego Bruciaferri (DB)

----------

## Agenda:

1. Review and approve minutes_13Feb2025 (there are no actions to review this time!) (All)
2. ECMWF: updates on issues with the iceberg/ice shelf climatologies in 4.0.6 (SK, CP)
3. Progress on GOSI10/review developments listed on https://github.com/JMMP-Group/GO_coordination/issues/30 (IA)
4. JRA-55 river forcing (AW)
5. Smagorinsky viscosity (AM)
6. Renault et al (AB)
7. Updates on GSI (EF)
8. fcm_make_pp failures (DC)
9. Migration to new Met Office/NERC HPC, Monsoon2->Monsoon3 (ancil files and workflows) (AB)

----------

## Minutes:

### •	Review and approve minutes_13Feb2025 (there are no actions to review this time!)

Minutes of [13 Feb 2025](https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/minutes_13Feb2025.md?plain=1) approved. No amendments required.

----------

### •	ECMWF: updates on issues with the iceberg/ice shelf climatologies in 4.0.6

**(SK)** ECMWF have noticed strong -ve values at several points where Merino et al ice shelf flux is input in NEMO v4.0 eORCA025 and wondered if anyone else has seen similar. The input files came from CG in 2021 when the iceberg code wasn’t stable. Looking at the email exchange there was code in v4.0.3 where a temperature mask was applied.  
**(CG)** We created an iceberg FW flux computed from GOSI6 climatology for GOSI8.  
**(AB)** I saw strong -ve temperature and fluxes in my ORCHESTRA Southern Ocean simulations (1/12 degree). The ice shelf flux is injected at depth and as FW it causes strong convection in the water column – I’ll check my notes and forward anything useful.  
**(CP)** It is the removal of latent heat from iceberg melting which can theoretically create waters with unrealistic temperatures. The code with the temperature mask that will distinguish between iceberg runoff and river runoff, and then potentially extract the latent heat depending on which type of flux is not in the Nemo trunk anymore.   

----------

### •	Progress on GOSI10/review developments listed on https://github.com/JMMP-Group/GO_coordination/issues/30

**(IA)** Presented an update on GOSI10 progress. See: https://github.com/JMMP-Group/GO_coordination/issues/22.  
Comparison of GOSI10p2.0 suites u-dh157 and u-dp653 run on XC40 (old HPC) and EX (new HPC). NEMO v5 is better than earlier tests with NEMO v5 RC. More ice in v5 than in v4.2.2.  
Comparison of GOSI10p3.0 suites u-dp705 and u-dp761. Both at NEMOv5 on new HPC, the latter using RK3 and dt=60 min (vs 30 min). Noted that on EX we can switch vectorisation back on.  
Merge request !865 resolved an issue with long waits in iom_init – included in GOSI10.beta.4.  
Performance on new HPC as expected. Like for like is approx. 20% faster (1 month in 32 minutes vs 40 minutes). Switching from v4.2.2 with MLF on old HPC to v5 with RK3 on new HPC is 100% faster (1 month in 20 minutes).  
**(AB)** It is worth noting that the new HPC has more cores per node, so you could run NEMO with a lot more cores (we use 1326 for our eORCA025 configuration at NOC and 1 year of v4.2.2 completes in around 100 minutes) or use fewer nodes. The CPU architecture comprises four 16 core NUMA nodes, so you want to distribute the cores evenly. On Archer2 (similar CPUs) I found leaving several cores free per NUMA node gives the best performance, otherwise jobs become memory bandwidth limited.  
**(DC)** I'm running something very similar to Isabella's ORCA025 GOSI10 NEMO 5 suite with 96 NEMO tasks per node, and 8 XIOS servers on one node only (using XIOS 2). Trying to run with 128 NEMO tasks per node caused an out-of-memory error. We could probably use more XIOS servers depending on performance.  
The issue with comparing MLF and RK3 in NEMO5 was resolved between 5.0-RC and 5.0 (https://forge.nemo-ocean.eu/nemo/nemo/-/compare/5.0-RC...5.0).  

----------

### •	JRA-55 river forcing

**(AW)** Run GOSI10p2 with Sarah’s modified JRA55 river runoff. Discovered a bug in NEMO if using the option to read in the iceberg climatology as a separate field. I have a fix, but need to return this to the NEMO repository. I need to perform some analysis (global means, seasonal, etc).   
It would be good to pass the suite through marine_assess to create validation notes. It might be easier for someone from the Met Office to do this.  
**(AB)** We have a copy of Sarah’s Matlab code for producing JRA55-do. Need to read/translate to produce forcing for the rest of the time series. Currently only have 1976-2020. We were concerned there might be stability issues since all the runoff is located in the coastal points, i.e. there’s no distribution away from the coast. Looking at the JRA55 runoff I found it strange to see river input around Antarctica.   
Following discussion we think this is likely to be surface runoff, rather than rivers. Need to check how big it is.  

----------

### •	Smagorinsky viscosity 

**(AM)** Run GOSI10p2 with Smagorinsky. It is unstable with 1800s timestep, so reduced to 1200s. All tests indicate a consistent improvement. I also want to try tuning the lateral mixing with Griffies in the coupled model, but will wait until Diego is back from paternity leave.

----------

### •	Renault et al (Adam)

**(AB)** NOC routinely use the Renault et al scheme now. JRA55-do supply the Rio et al. (2014) surface current climatology. I exchanged emails with KM, and we believe ERA5 does not require surface currents to be supplied, but ERA6 will. For GOSI the decision was made to align with OMIP protocol – there was discussion of a 70:30 fixed ratio between absolute and relative winds, but this needs to be confirmed. Ollie and I did some testing at 1 degree. Large scale differences weren’t huge, and mainly confined to the region of the equatorial refinement. I didn’t look at the EKE field, and since it was decided not to use it for GOSI we didn’t invest any more time in evaluating it. The code for specifying a ratio between absolute and relative was removed between v4.0 and v4.2.  
**(IA)** I back ported that code so it is in GOSI10.  
**(CG)** GOSI released since 10p2.0 have all used Renault et al scheme.  
There was some discussion about what ocean forecasting and DA will want to do. Use of Renault et al or a fixed ratio is dependent on the forcing – any coupled model or models using forcing from a coupled system will almost certainly want to use absolute forcing, since the effect of the surface ocean currents is (or has already been) taken into account.   

----------

### •	Updates on GSI

**(EF)** Using NEMO v5 – done tests (5 year) with the Rothrock scheme at eORCA025. Scheme changes sea icea strength and rheology. Adaptive EVP really improves convergence. Now tuning and testing sensitivity before trying a 30-year simulation.  


----------

### •	fcm_make_pp failures that are likely to affect all GO8/GOSI suites

**(DC)** Changes to MOCI result in GOSI suite failures in the fcm_make_pp step. There is a simple one line fix, but looks like a bug that needs to be reported. To summarise, the suite error will be in fcm_make_pp and the message will be:  

&nbsp;&nbsp;&nbsp;&nbsp;*[FAIL] jdma.py: don't know how to build specified target*  

To fix this, set config_rev=@4416 in app/fcm_make_pp/rose-app.conf  

----------

### •	Migration to new Met Office/NERC HPC, Monsoon2->Monsoon3 (ancil files and workflows)

**(AB)** We have just been given access to the new Monsoon3. Monsoon2 will be withdrawn from service on June 30th, so we have ~6 weeks to transfer all data/files we need. It is up to the individual to do this. Ancillary files are (mostly) central, but I have the JRA55-do forcing with river runoff that Amber is testing, and Alex has a copy of ERA5 from Richard Renshaw. It is a good opportunity to centralise files that are not already, to avoid duplicate copies.  

There is a shared account on the new system, though it is unclear whether it is necessary or possible for non Met Office staff to access it.  

GOSI10 suites on Monsoon3 are cylc8.  

Users will need to transfer across their .moose directory to continue to be able to access MASS.  

----------

**(AA)** Next meeting is June 12th. Catherine is presenting an update on GOSI10 to the Momentum workshop, and someone from UKESM will present the hybrid resolution: physics at ¼ degree, BGC at ¾ degree.  

----------

## Actions:

 •	**(AB)** Check and send any details on strong -ve temperatures at ice shelf to SK and CP.

 •	**(AW)** Liaise with MO staff to run marine_assess on JRA runoff test suite.

 •	**(All)** Be proactive about transferring workflows to Monsoon3.

 •	**(AA)** Send out agenda ~1 week ahead of the next meeting (June 12th) and ask ?? to minute the meeting.


