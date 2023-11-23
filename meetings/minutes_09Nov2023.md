# JMMP global model development meeting minutes

9th November 2023.

Participants: Ana Aguiar (AA - chair), Alex Megann (AM), Adam Blaker (AB), Sarah Keeley (SK), Mike Bell (MB), Diego Bruciaferri (DB), Catherine Guiavarci'h (CG), Isabella Ascione (IA), Matt Menary (MM), Ed Blockley (EB), Dave Storkey (DS - minutes).

Apologies: Kristian Mogensen, Richard Renshaw.

----------

## Agenda:
1. Review draft JMMP GO workplan
2. Agree on ways of working, e.g. frequency of our meetings, regular task reports (to the wider group) on work-in-progress or blockers
3. AOB

----------

## Introduction:
- AA will be taking over from DS as JMMP global development lead. 

- AA is also working on a GANTT chart to keep track of progress towards goals. The next major release of GOSI will be GOSI10 in mid 2026, aiming for inclusion in GC7 (package trials starting Jan 2027). Note that:
   - a. There will be no ocean or sea ice model update in GC6. This is focussed on the inclusion of the new atmosphere model LFRic. 
   - b. Data assimilation needs to be working with GOSI10 prior to the beginning of GC7 package trials. 

- Baseline development: NEMO 5 release is pencilled in for mid 2024. We will aim to base GOSI10 on NEMO 5. 

----------

## JMMP GO work plan review:
1. _New bathymetries_:
   - AB said that Jérôme Chanut had told him about some small modifications to the ORCA025 grid around the north fo. **Action: DS to email Jérôme about this.**

2. _Optimisation_:
   - a.	Andrew Coward has tested XIOS3 with NEMO 4.2.1 and got good performance writing to a single output file with ORCA025. Hasn't tried ORCA12 yet. 
   - b.	Daley Calvert is also testing XIOS3 with GOSI9 -- just ORCA025 so far.
   - c.	ECMWF have a lot of experience with mixed precision that we can draw on. EB pointed out that we need to be careful with this for climate integrations. 

3. _Vertical mixing_:
   - a.	Still some way to go with OSMOSIS -- not certain for GOSI10.
   - b.	AB and CG have both tested the IWM scheme.

4. _Numerical mixing_: AM has updated the plans with the latest on this. 

5. _High resolution and AGRIF_: 
   - a.	IA has now set up GOSI9 to run on the ECMWF machine via Rose/Cylc. We will aim to run ORCA36 on the ECMWF machine but this resource will be useful for other integrations and collaboration as well. 
   - b.	DB has incorporated AGRIF into a Rose job with NEMO 4.2.1 (compiling with fcm build). A null test shows nonzero differences between having key_agrif turned on or off (with no child configuration) and DB is investigating this. 

6. _SE-NEMO_: The ORCA025 configuration is now tuned and working well and a paper is being drafted. One of the tunings was to adjust the bottom friction. This configuration doesn't include z~.

7. _Assessment tools_: The Met Office is transitioning its assessment of climate integrations to use ESMValTool. At the Met Office, we hope to recruit an industrial placement student starting Sep 2024 to work on developing ocean diagnostics in ESMValTool. NOC are interested in this as well, and there is a group (name?) of software engineers at NOC who could help with this. Lee de Mora at PML has done some work on ocean metrics in ESMValTool that we can build on.

----------

## Ways of working going forward:
1. We agreed to aim for longer 3-monthly meetings to review progress with the work plan, and shorter meetings in between (monthly?) to stay in touch. The list of current issues should be circulated a few days in advance of the 3-monthly meetings so that we spend the time discussing possible solutions. **Action: AA to ensure this is done.**
2. There was some discussion of the best way to track progress. The Gantt chart started by AA is one option. We could use the JMMP Github site. AB said there is a _roadmap_ functionality in Github that we could explore. **Action: AA to explore options with input from others. (Post-meeting note: AA is likely to use the Roadmap option in Github).**
3. JMMP Github would be a good place to track big issues, especially issues raised by stakeholders. There is a question as to whether we need to also continue to make use of GMED tickets on the Met Office Science Repository. 

----------

## AOB:

AB gave a presentation on the problems with the EN4 climatology (currently used for initial conditions, relaxation and assessment for GOSI configurations) in the Arctic.
a. There is an odd radial pattern radiating from the north pole, especially in the salinity. Perhaps to do with EN4 trying to interpolate between sparse observations. 
b. There are also odd patches of SST at exactly 0 degC north of Greenland. 
c. WOA looks better but it still has issues. DB pointed out that WOA is specified as the initial conditions for the OMIP protocol, so would be good to switch to this to align ourselves with that. 
d. There are also spurious salinity values in the region of the Gulfstream extension. This may be because some ARGO floats with strong vertical density gradients get marked with bad QC flags and therefore would be left out of the EN4 analysis. 

