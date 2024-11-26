# JMMP global ocean model development meeting minutes

26th Sep 2024.

Participants: Ana Aguiar (AA - chair), Alex Megann (AM), Amber Walsh (AW), Adam Blaker (AB),
Catherine Guiavarc'h (CG), Dave Storkey (DS - minutes), Diego Bruciaferri (DB), 
Isabella Ascione (IA), Kristian Mogensen (KM), Richard Renshaw (RR), 
Sarah Keeley (SK), David Schroeder (DSc), Emma Fiedler (EF), Daley Calvert (DC) 

Apologies: Ed Blockley, Mike Bell, Chris Wilson.

----------

## Agenda:
1. Review actions and approve the minutes from the last meeting, [9 May 2024](https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/minutes_09May2024.md)
2. GOSI10 work-in-progress:
   - (Catherine) GOSI10.p2: impacts of small science changes to bring GOSI up to date with NEMO trunk 
   - (Isabella, Daley) porting to NEMO5beta
3. (Diego) Impacts of an improved representation of the Mediterranean Outflow
4. (Alex) Latest from coupled experiments
5. AOB

----------

## Review actions from previous meetings

1. **Action 2.4: Contact Sarah Wakelin for more information on JRA rivers runoff.** _Ongoing._ AM and AW are in touch with Sarah Wakelin and will test the JRA rivers runoff. AW needs a GOSI job that runs on Monsoon.
2. **Action 2.5: AA/CG to find out which period is suitable to run with NWP forcing as another atmospheric forcing GOSI10 ensemble member.** _Closed_ This would extend the atmospheric forcing ensemble but it's largely for the benefit of OFR&D who should take on this task. [EF] For what time period? JRA covers 1958-2023: [AM] AMOC looks better with 1958 start but [AB] there's better forcing data from 1970s onwards. [DB] OMDP new protocol sets start to 1958, OMIP runs include 5-10 yrs spin-up to allow initial adjustment. 
3. **Action 3.1: AA to create a timetable for the various development releases.** _Closed_ See this slide for the [GOSI10 prototypes plan](https://github.com/JMMP-Group/GO_coordination/wiki/GOSI10-prototypes-plan), created by CG.

----------

## Progress with GOSI10

1. _GOSI10p2_: [CG] includes [small science changes](https://github.com/JMMP-Group/GO_coordination/issues/18)
   1. Reduced ACC transport due to removal of partial slip in SO
   2. Increased salinity and deepening of MLD in the Labrador Sea
   3. SST, SSS change in the Arctic due to upgrade of the internal wave mixing scheme, little change in sea ice. SK suggested looking at seasonal sea ice maps.
   4. Current feedback scheme switched on but not optimal because we don't use the ocean surface currents in JRA at the moment (some products have these as mesured by scatterometer); JRA has absolute forcing. [AB] If you follow the flow diagram in Figure 11 of Renault et al. (2020) it clearly states that "coherent surface currents with the atmospheric forcing from the reanalysis-like or from the observations are needed". JRA55-do (i.e. Tsujino et al., 2018) provides climatological surface ocean currents from Rio et al., (2014) that we should be using for running with JRA55-do. See the [user guide](https://climate.mri-jma.go.jp/pub/ocean/JRA55-do/docs/v1_5-manual/User_manual_jra55_do_v1_5.pdf) for further details. I have downloaded the uos and vos fields.  OMDP also looking into this and planning to taking a fraction (70%) of JRA winds to account for the current feedback effect; this would need coding. [AB] I guess they want to do this because many models will not have the Renault et al. (2020) scheme coded. Does ERA5 have "surface currents as seen by atmosphere"? KM said it's not clear because of bias correction. KM alerted for differences between scatterometer measurements and ocean currents.  **Further discussion needed.** 
</br>

2. _Porting to NEMO5RC_: IA ported GOSI10p2 to NEMO5RC but experiment crashed after 25 yrs with sea ice problems. Daley looked into this, first running similar version but stripped off GOSI10-specific changes and got the same error. Further investigation linked the problem to FMA (Fused multiply-add operations), with their order depending on compiler flag options, and concluded that this issue is specific to the Cray compiler used in the Met Office; not expected to reoccur in the new HPC. For detailed explanation of the issue and fix see [issue#22](https://github.com/JMMP-Group/GO_coordination/issues/22). This has been communicated to the NEMO systems team.

3. _Mediterranean Overflow_: DB presented results from two experiments using a 1/20 deg AGRIF zoom in the Gulf of Cadiz including the Strait of Gibraltar. 
   1. One experiment using zps in both the child and parent (1/4 deg) domains.
   2. Another experiment changing to MEs in the child domain.
The depth of the Mediterranean water, the density structure of the water column, the salinity and EKE at 1000 m, are closer to observations (distribution patterns and values) in the experiment with MEs in the zoom. Improvements feedback to the parent grid work well, resulting in an enhanced Azores current and overall reduction of the North Atlantic salinity bias at 550 and 2000 m depth. EKE fields of the parent show a better westward propagation of meddies when MEs are used in the child domain. Computational efficiency is good.

4. _GC5 ensemble_: AM presented results for coupled experiments N96-ORCA025, with z-tilde and TRIADS (Griffies) for isoneutral diffusion and is writing a paper (with Dan Copsey) on this. New diagnostics of integrated diapycnal upwelling in the Indo-Pacific show that, with tides, there's much more mixing but slightly less with z-tilde (less spurious mixing); Surface air temperature, SST: using 2nd order tracer advection leads to a warming of surface T, with more mixing; with cooling there's less mixing (!?), trying to understand this by looking at sea ice, strength of the thermocline and heat transport. AMOC differs when using TRIADS (smoother streamfunction and more significant results): increased mixing weakens the ACC (!?), with noticeable impacts on sea ice. DB questioned this as more mixing should lead to a stronger AMOC: depends on where this happens, mixing at depth and high latitudes, plus Indo-Pacific upwelling: "missing mixing" which is needed to close the AMOC, see Ferrari and Wunsch supplement.

----------

## AOB

1. AW has been experimenting with different WOA ICs and SSS restoration. A general conclusion is that it is better to use ICs closer to the starting date of your simulation. To follow the OMIP protocol, we'll carry on using WOA for initial conditions but CG will inform Rachel Killick (Cc AB) about EN4 issues at the poles [WOA is better than EN4 for initial conditions]. [AB: Rachel is now aware of the issues with EN4. Following the AMOC meeting at the Met Office we exchanged some emails and I shared the slides showing the IC issues. The "star-shaped" feature in the Arctic is a regridding issue and I don't think that will go away any time soon. An updated EN4 is due imminently.]

----------

## Actions

 * 4.1: CG to contact Rachel Killick (Cc AB) regarding EN4.

