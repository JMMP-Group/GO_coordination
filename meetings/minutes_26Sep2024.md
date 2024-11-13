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
   4. Current feedback scheme switched on but not suitable because we don't have ocean surface currents in JRA (some products have these as mesured by scatterometer); JRA has absolute forcing, OMDP looking into this and planning to taking a fraction (70%) of JRA winds to account for the current feedback effect; this would need coding. Does ERA5 have "surface currents as seen by atmosphere"? KM said it's not clear because of bias correction. [AB] said that Sejoro et al 2018 has climatological currents that we could use and we should ask other centres. KM alerted for differences between scatterometer measurements and ocean currents.  **Further discussion needed.** 
</br>

2. _Porting to NEMO5RC: IA ported GOSI10p2 to NEMO5RC but experiment crashed after 25 yrs with sea ice problems. Daley looked into this, first running similar version but stripped off GOSI10-specific changes and got the same error. Further investigation linked the problem to FMA (Fused multiply-add operations) with their order depending on compilar flag options and therefore this issue being specific to the Cray compiler used in the Met Office. FMA were introduced to reduce the use of lbc_links. Enforcing the use of lbc_links avoids the problem and also fixes restartability issues.

----------

## AOB

1. AB presented results from AW showing that EN4 is better than WOA for initial conditions. To follow the OMIP protocol, we'll carry on using WOA for initial conditions but CG will inform Rachel Killick and copy AB about the improvements in EN4.

----------

## Actions

 * 4.1: CG to contact Rachel Killick (Cc AB) regarding EN4.

