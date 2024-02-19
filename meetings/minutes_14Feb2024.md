# JMMP global ocean model development meeting minutes

14th February 2024.

Participants: Ana Aguiar (AA - chair), Alex Megann (AM), Amber Walsh (AW), Catherine Guiavarci'h (CG), Dave Storkey (DS - minutes), Diego Bruciaferri (DB), Ed Blockley (EB), Isabella Ascione (IA, Kristian Mogensen (KM)), Richard Renshaw (RR), Sarah Keeley (SK).

Apologies: Adam Blaker, Mike Bell, Chris Wilson.

----------

## Agenda:
1. Review actions/approve minutes of the previous meeting 
   - AB said that Jérôme Chanut had told him about some small modifications to the ORCA025 grid around t north fold. Action: DS to email Jérôme about ts.
   - Action: AA to circulate list of current issues a few days in advance of the 3-monthly meetings - not necessary as issues are mostly up-to-date, thanks to all contributors! _Closed._
   - Action: AA to create JMMP GitHub repository for GO coordination - done. _Closed._
2. NOC-Southampton new member: Amber Walsh
3. GOSI10:
   - Highlight updates on progress or blockers regarding JMMP GO work plan
   - Initial conditions: Move from EN4 to WOA? Average over which time period? Update runoff inputs?
   - Atmospheric forcing: given issues with ERA5 should we use JRA (discontinued after Jan 2024) for GOSI10 to avoid hassle of not working with a standardised product?
   - Length of simulations: Should we run two cycles, e.g. run from 1958 (ICs) until 2023, then take restart from the end of first cycle and to start another run from 1958 to have a fully spun-up ocean? Tobias Schulzki's poster at DRAKKAR suggests that long runs (2 cycles, 40-50 yrs each) are needed to have a stable AMOC
      - Alex to show plot with AMOC and T drift in ensemble with different forcing
      - Catherine to show plot with GO6 and GOSI9 T drift using CORE
4. AOB
   - Move series of meetings to Thu 10:30-12:00am going forward if convenient?

----------

## New member

1. AM introduced AW who will be mainly working with Jenny Mecking but has some time (25%?) under National Capability.

----------

## Actions from previous meetings

1. **Action: DS to get updated ORCA grids from Mercator. DS can't remember where he got with this -- to check.**

2. Other two actions _closed_ (see Agenda above).

----------

## GO development general updates:

1. _OSMOSIS_:
   - CG has completed tests with ORCA1 and it is now running (fairly) stably in ORCA025 after switching from the vector invariant formulation to the flux-form formulation with UBS advection. Southern Ocean looks worse.

2. _Optimisation_:
   - DC showed results with NEMO 5 (ORCA2 with no sea ice), showing up to a 3x speed up with XIOS3 and RK3 timestepping. The beta release is currently pencilled in for March with the full release in June. AM pointed out that you need shorter timesteps with some numerical choices and to resolve short timescale processes like tides. KM said that the next ECMWF benchmarking configuration will be based on NEMO 5. EB recommended consulting the coupled model team to ensure the time-step in forced and coupled runs is consistent.[Note post-meeting: KM and Tim Graham now added to #2.]

3. _Numerical mixing_: 
   - AM has an ensemble of coupled GC5 N216-ORCA025 runs for which he is generating mixing metrics. So far the sensitivity results look consistent with his forced tests. 

4. _GC5-Clim_:
   - Alejandro Bodas-Salcedo has asked for help in assessing the ocean in his tuning ensemble for GC5-Clim (and also potentially suggestions for ocean parameters to perturb - so far only perturbed atmosphere parameters). DS and CG will support this work. **Action: AA will pass AM contact to Alejandro.** 

5. _Diagnostic tools_: 
   - RR and Matt Menary are interviewing this week for an industrial placement student to work on diagnostic tools including ESMValTool starting in the summer.

6. _ORCA36_: IA has managed to run 1 day on the ECMWF machine but with no diagnostics as yet (XIOS turned off). 

----------

## Discussion of new protocol for standard forced experiments:

1. Initial conditions: Consensus is that we should move to a version of WOA in line with the OMIP protocol. Adam (email communication) favours the WOA2023 "Climate Normal" dataset. **Action: DB will check at the next Clivar Ocean Model Development Panel meeting what period will be adopted.** DS pointed out that we should use the same climatology to assess the model that we use to initialise the model. RR made the alternative suggestion of initialising from a reanalysis but DB/CG pointed out that these often have the same biases as the model in the nordic overflows region.

2. Choice of forcing dataset: General agreement that we should stick with JRA55-do for now until the corrected ERA5 (ERA5-do) product is available. (ERA5 can still be used for OSMOSIS tests. AM pointed out that the GOSI9 drifts with JRA and ERA5 are larger than the drifts with CORE2, so we may need to retune the model. DS said that CORE2 has been detrended to remove the climate signal (to do the repeat 50-year protocol) and we don't think the other forcing data has been detrended so we need to bear this in mind.

There was some discussion of whether we should have an ensemble of tests based on different forcing data (and possibly different choices of bulk formulae). Agreed that this is useful to provide information on uncertainty but impractical to do it for every sensitivity test. One option would be to use the ensemble that AM has already run to provide estimates of uncertainty in the model fields based on the uncertainty in the forcing. We could produce a new ensemble with the final GOSI10, in order to inform error bars/spread due to differences in forcing. DB showed the impact of model numerics choices on the overturning at OSNAP East to show how sensitive the results can be; upstream of OSNAP, overflows biases are much smaller.
                 
It was noted that we still need coupled tests to see what happens with the coupled feedbacks. **Action: We should work with Tim Graham's team to try to make these happen.**
                 
3. Length of standard tests. With JRA and ERA we can run longer periods and agreed that we should but not agreed the exact period: 1958 or 1976 until 2023? Initial conditions from present-day are not valid for either of those start years but probably cause less drift than model biases. AM says that ORCA12 runs on ARCHER2 have a turnaround of 2 SYPD. 
                 
4. Freshwater input from land. DB said that the rivers data tended to be specific to particular forcing data sets. Sarah Wakelin has looked at the impact of the JRA rivers in the Arctic. KM said that we ERA5 we should use a separate ECMWF river discharge product rather than the one packaged with ERA5: [ESSD - GloFAS-ERA5 operational global river discharge reanalysis 1979-present](https://essd.copernicus.org/articles/12/2043/2020/). The Antarctic freshwater input is a separate beast and we are currently using climatologies based on Rignot et al (year?) created by Pierre Mathiot. **Action: Someone (CG?) to Contact Sarah Wakelin to get more information.**

----------

## AOB:

1. AA will move series of future meetings from Wed to Thu 10:30am-12:00am so that NOC-S group can attend the full meeting.

2. [Post-meeting] Sam Hatfield (ECMWF) will present his work on the use of NEMO with mixed precision at the next meeting: 9th May.

3. [Post-meeting] DS suggested we could add NWP forcing as another ensemble member: we will need to  choose a period when this would be useful. The atmospheric forcing ensemble that AM showed uses CORE2, DFS, JRA-55 and ERA5.

4. [Post-meeting] Chris Wilson informed that Sarah Wakelin has been using river inputs from dataset derived from a catchment model and JRA55 for SE-NEMO.
