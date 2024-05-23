# JMMP global ocean model development meeting minutes

9th May 2024.

Participants: Ana Aguiar (AA - chair), Alex Megann (AM), Amber Walsh (AW), Adam Blaker (AB),
Catherine Guiavarc'h (CG), Dave Storkey (DS - minutes), Diego Bruciaferri (DB), 
Isabella Ascione (IA), Kristian Mogensen (KM), Richard Renshaw (RR), 
Sarah Keeley (SK), Tim Graham (TG), Martin Price (MP), Samuel Hatfield (SH)

Apologies: Daley Calvert, Ed Blockley (2nd part of the meeting), Mike Bell, Chris Wilson.

----------

## Agenda:
1. Samuel Hatfield: using NEMO with ~~mixed~~single precision. [Recorded for those who cannot attend]
2. Review actions and approve the minutes from 14 Feb 2024
3. GOSI10:
   - Highlight updates on progress or blockers regarding JMMP GO work plan
   - WOA initial conditions: Average over which time period? Update runoff inputs?
4. NEMO grid (re)alignment for coarsening-using-AGRIF
5. AOB

----------

## Using NEMO with single precision (Sam Hatfield)

SH gave a talk on his experience with testing ECMWF coupled configurations using single 
precision (wp=sp) to improve NEMO performance: 
 - They find a good improvement in performance. The overall gain in the coupled model depends on the percentage of the total wallclock time that the ocean model takes.
 - It is difficult to show that this kind of change is scientifically neutral so they have packaged this change up with science changes and aimed to show that the effect of the combined package is positive.
 - ECMWF are focussed on fairly short integrations (up to seasonal timescales) with data assimilation, so are less worried about conservation. Centres looking at climate timescales (eg. BSC) worry about conservation more and might take a different approach (adopt mixed precision) to optimisation. IFS and waves model in use at ECMWF run on single precision. 

----------

## Review actions from previous meetings

1. **Action 1.1: DS to contact Mercator to get updated ORCA grids: small modifications to the ORCA025 grid around the north fold?** DS and AB have now received the adjusted ORCA grids from Clement Bricaud at Mercator, together with some explanatory slides from Jerome Chanut. The motivation for this change is to make it easier to use AGRIF to do online coarsening of fields. Among the customers for GOSI configs it is only UKESM who are interested in this functionality and they are currently testing a coarsening approach using OASIS. So given that it would be a lot of work to move to new grids we should not progress this at this stage. _Closed_
2. **Action 2.1: AA to pass AM contact to Alejandro re GC5 tuning.** _Done. Closed_
3. **Action 2.2: DB to ask/check at the next Clivar Ocean Model Development Panel meeting what version and period of WOA will be adopted to generate ICs/climatology for OMIP runs.** _Done._ DB has updated the relevant issue https://github.com/JMMP-Group/GO_coordination/issues/3 _Closed_
4. **Action 2.3: AA/CG/DS We should work with Tim Graham's team to try to make sure we test feedbacks in coupled mode.** TG's group are planning to create coupled configurations with the updated GOSI configuration. They also have plans to test OSMOSIS in the version of GC with wave coupling. _Closed_ 
5. **Action 2.4: CG to contact Sarah Wakelin for more information on JRA rivers runoff.** _Ongoing_ Need a reformulated action to make a plan as to what to do with this. As an aside with the push to document and release model input files there is a project at the MetO to check the T&Cs of the source datasets to check that there are no obstacles to doing this.
6. **Action 2.5: AA/CG to find out which period is suitable to run with NWP forcing as another atmospheric forcing GOSI10 ensemble member.** _Ongoing._

----------

## Progress with GOSI10 (and AGRIF)

1. _Prototypes_
- CG is running various tests for the next development releases which will be released shortly:
   1. GOSI10p0 ORCA12 test - at NEMO 4.0.4, close to GOSI9 settings. 
   2. GOSI10p1 will include the new bathymetries.
   3. GOSI10p1.1 switches from CORE2 to JRA-55 forcing. 
   4. GOSI10p2 includes [small science changes](https://github.com/JMMP-Group/GO_coordination/issues/18) prior to the move to NEMO 5.
   5. The next step will be to test NEMO 5&beta;.
- AM asked which versions will be available on Monsoon and CG said that she will create a Monsoon version of p1.1.


2. _NEMOv5_
- DB and Daley have identified potential issues and opportunities from moving to NEMO 5 and documented them [here](https://github.com/JMMP-Group/GO_coordination/issues/2).
   1. AB said that at NEMO 5 there are two versions of the adaptive vertical advection scheme. It automatically chooses the appropriate one depending on the choice of momentum advection scheme.
   2. Data assimilation not working in NEMO 5 yet.  

4. _AGRIF_ 
- DB has created a 1/20&deg; AGRIF zoom in the Labrador Sea in ORCA025 and finds a big improvement
in the Labrador Sea water masses and mixed layer depths due to better representation of the west Greenland
boundary current and eddy system. Promising as a way of improving the North Atlantic and AMOC representation
in ORCA025 at relatively cheap cost. 

5. _Timetable_
- AA will create a timetable for the various development releases. 

----------

## Actions

 * 2.4: AA/CG: Create a scoping action to decide what to do about river input.
 * 2.5: AA/CG to find out which period is suitable to run with NWP forcing as another atmospheric forcing GOSI10 ensemble member.
 * 3.1: AA to create a timetable for the various development releases.

