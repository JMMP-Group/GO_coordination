# JMMP GO model development meeting minutes

*Thursday 21st May 2026*

*Participants:* 

Ana Aguiar (AA - chair), Isabella Ascione (IA), Daley Calvert (DC), Diego Bruciaferri (DB), Catherine Guiavarc'h (CG - minutes), Alex Megann (AM), Adam Blaker (AB), Richard Renshaw (RR), Dave Storkey (DS), Amber Walsh (AW)

*Apologies:* 

Emma Fiedler (EF), Ed Blockley (EB), Mike Bell (MB), Sarah Keeley (SK), Kristian Mogensen (KM), Charles Pelletier (CP), David Schroeder (DSc), Matt Martin (MM), Davi Mignac (DM), Daniel Lea (DL).


*The rota for minuting follows JMMP people surname in alphabetical order: IA, AB, DC, DB, EF, <ins>CG</ins>, EB, AM, DSc, DS, OT, AW. (This was listed in the minutes of Nov 2024).  We only minute the quarterly meetings, not the monthly ones.*

----------

## Actions from last meeting:

We did not discuss actions from the last meeting, although I can see that they have been acted on.
 - [x] (EF) Update on sea ice developments. _Closed_
    - GOSI10.beta.5 including the sea ice development has been released and results have been shared at developer meetings
 - [x] (AW) Update on rivers updates. _Closed_
   - It has been agreed that AW will share her progress with CG and handover her work on rivers. Meeting planned on 21/05/26
 - [x] (All) Update on GOSI10.beta.6. _Closed_
   - AA presented the latest results with GOSI10.beta.6 see discussion below.
 - [ ] (All speakers) Upload slides to the meeting minutes.

----------

## Minutes:

#### GOSI10-ORCA1 discussion (DS)

<ul>

DS has recently realised that there was a bug in the eddy viscosity coefficients used for eORCA1. The coefficients currently used for GOSI10 are inherited from GOSI9.
2 coefficients are required for the rotational and irrotational parts, there should be a reduction of these coefficients with high latitudes and near the equator. See https://github.com/JMMP-Group/GO_coordination/issues/19.
<br>

Issues with current files:
 - reduction of coefficients with high latitude is missing (already known).
-  reduction of coefficients near the equator has only been applied to one coefficient (bug)
<br>

UKESM has been using different coefficients as the reduction at high latitudes is required for ice shelf cavities. AB has produced a file with corrected coefficients (both at high latitude and near the equator) but Colin Jones was worried about using those coefficient as it meant retuning the model.   

DS suggests testing the accelerated spin up method with AB file. AB will check that the file has the reduction both at high latitude and near the equator and share the file with DS to test. 

DS also mentioned that he has started work to test GEOMETRIC, he has no result yet but was able to run the model successfully. 
<br>
</ul>

#### GOSI10.beta.6 and RK3 (AA)

<ul>
At GOSI10.beta.6, RK3 shows large differences compared to MLF (reduced AMOC, divergence in global temperature). This was not the case at GOSI10.beta.4. DC has run a set of sensitivity experiment to understand which change between GOSI10.beta.4 and GOSI10.beta.6 explain the different sensitivity to RK3. 
AA presented the results from those experiments (see https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/slides/20260521GOSI10.beta.6MLFRK3.pdf).

<br>

The findings are:

- Changes to SI3 namelist parameter ( linked to GOSI10.beta.5 ) cause a significant AMOC strengthening as well as a divergence in global temperature.
- No technical change (code change) has led to this change
- The only simulations showing a divergence between RK3 and MLF, are the simulations including the SI3 namelist changes and showing rapid change in AMOC. If anything, RK3 mitigates the abrupt change in AMOC
- Abrupt change in AMOC, SPG salinity, temperature and strength is more likely to be affected by the time stepping which explain the larger impact of RK3 linked to the SI3 namelist changes.
- EF, not present, previously mentioned that some of the sea ice developments planned for GOSI10.beta.7 should improve the AMOC
- GOSI10.beta.6 should go ahead, it will be a technical release (not to be used for science purposes), it should include cherry picked bug fixes from NEMO5.0.2 release (including Ob estuary bugfix)
- DB (supported by AB) suggests that the change in temperature and salinity with RK3 in GOSI10.beta.6 should be investigated. We should at least share the results above with Sybille.

  <br>

We also discussed AMOC in forced model and agreed that this should be discussed further. Here are few points discussed briefly: 

-	Since moving to SI3 the ocean seems to be more sensitive to sea ice changes
-	Should we focus less an AMOC and more on the change in temperature and salinity? 
-	What should the AMOC look like in a forced model? (highly dependent to forcing)
-	OMIP type experiments are useful for process understanding but should not be used to tune the model.
-	Any AMOC tuning should be done with the coupled model.

<br>
</ul>

### Plan for future releases
<ul>
  
-	GOSI10.beta.6: technical release will includes cherry picked bug fixes from  5.0.2 (including the Ob estuary bug fix)
-	GOSI10.beta.7 :  sea ice developments led by EF. CG and EF should discuss if the new rivers forcing (JRA55) and the interactive icebergs should be included in GOSI10.beta.7  or in a sparate release.
-	GOSI10.beta8: GOSI tuning with the coupled system (GC7/LFRIC). 
<br>
Current feedback (Renault et al): should be reviewed once OMIP has agreed on the protocol, not expected before corrected ERA5 forcing is available. Once available, we should aim at running an official OMIP simulation with GOSI10 and a GOSI10 ensemble with different forcings.
<br>
</ul>

----------

## Actions:

  * (AB)  Check eddy viscosity files and share with DS.
  * (DS)  Test corrected eddy viscosity files (provided by AB) with the acceleration spin-up. 
  * (DC)  Cherry pick NEMO5.0.2 bug fixes into GOSI10 branch for GOSI10.beta.6 release.
  * (CG)  Discuss rivers and iceberg with EF.
  * (DC/AA) Share RK3/MLF results with Sybille.
  
