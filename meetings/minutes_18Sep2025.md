# JMMP GO model development meeting minutes

*Thursday 18th September 2025*

*Participants:* Ana Aguiar (AA - chair), Mike Bell (MB), Adam Blaker (AB), Daley Calvert (DC - minutes), Catherine Guiavarc'h (CG), Sarah Keeley (SK), Oliver Tooth (OT), Amber Walsh (AW)

*Apologies:* Isabella Ascione (IA), Diego Bruciaferri (DB), Emma Fiedler (EF), Alex Megann (AM), Kristian Mogensen (KM), Charles Pelletier (CP), Richard Renshaw (RR), David Schroeder (DSc), Dave Storkey (DS)

*The rota for minuting follows JMMP people surname in alphabetical order: IA, AB, <ins>DC</ins>, EB, DB, EF, CG, AM, DSc, DS, OT, AW. (This was listed in the minutes of Nov 2024).  We only minute the quarterly meetings, not the monthly ones.*

----------

## Agenda:

1. Licensing of JMMP-generated software/public repositories (AA)
2. Update on latest experiments and results with NEMOv5 RK3 60 min vs MLF 30 min (CG)
3. GOSI workflows - Monsoon3 (DC)
4. Update on nemo_cookbook and long-term integration plan with `MARINE_VAL` (OT)
5. JRA55 river runoff results (AW)

## Actions from last meeting:

- [ ] (AB) Check and send any details on strong -ve temperatures at ice shelf to SK and CP.
- [x] (AW) Liaise with MO staff to run marine_assess on JRA runoff test suite.
- [ ] (All) Be proactive about transferring workflows to Monsoon3.
- [x] (AA) Send out agenda ~1 week ahead of the next quarterly meeting (Aug 14th, pending quorum) and ask EB to minute the meeting.

----------

## Minutes:

#### Licensing of JMMP-generated software/public repositories (AA)

<ul>

AA raised the issue of assigning appropriate licenses and copyright notices in publicly-distributed work developed under the JMMP.

  * The Met Office is taking an increasingly strict approach to this- anything developed jointly under the JMMP must be licensed before being made public
  * AA cited issues with `MARINE_VAL`, which uses the GPL3 license
    * This is less flexible than e.g. CeCILL, BSD3, Apache or MIT licenses
    * If code from a GPL3-licensed repository is included in a different code, the entirety of this other code must also use the GPL3 license
  * Someone with the appropriate knowledge or legal expertise would potentially need to review work for us
  * AB noted that NOC is also having to consider this for software they produce

<br>

AB asked whether everything produced under JMMP should have the CeCILL license.

  * AA was not sure- e.g. for assessment tools, it depends on the packages they use; we would have to ask Met Office Science IT/legal team.

</ul>

#### Update on latest experiments and results with NEMOv5 RK3 60 min vs MLF 30 min (CG)

<ul>

CG [presented results](https://github.com/JMMP-Group/GO_coordination/issues/22#issuecomment-3306820191) from sensitivity experiments using NEMO 5.0 (i.e. not using the GOSI10 code or namelist settings specific to this code):

  * In eORCA025 with RK3, the large-scale subsurface temperature changes (mainly in the Pacific) observed when doubling the timestep (from 30m to 60m) persist when `nn_etau` (ad-hoc parameterisation of missing wind-driven mixing) is disabled
  * In eORCA1 with both RK3/MLF and with `nn_etau` off, these temperature changes are much weaker but there is an impact on ACC transport
  * In summary, neither the use of `nn_etau` nor the GOSI10 code/namelist settings seem to explain the timestep sensitivity
  
<br>

AA later presented these results (and those from the `nn_fsbc` experiments) to the NEMO Systems Team. The main outcomes from this were:
  * All agreed that we need to understand the reason for the differences and that the NEMO ST should follow-up on this; discussion TBC in the next meetings - see initial work plan below (Actions)
  * Andrew Coward (NOC) suggested running with a 45m timestep to see if the magnitude of the differences are proportional to the timestep size
  * Sibylle Techene (CNRS) mentioned that it should be possible to run eORCA1 with a 2h timestep (90m at least)
  * Jerome Chanut (Mercator) mentioned that the TKE scheme itself has a timestep-dependence (in the dissipation term) and that we should try running with a non-prognostic vertical mixing scheme e.g. Richardson number scheme or constant diffusivity.
  * Sebastien Masson (CNRS) asked for the namelists and/or `ocean.output` files for CG's experiments

<br>

How long should we continue to try and understand this? What should we try next? 

  * AB suggested running with a 15m timestep
  * MB thinks the advection scheme is now the most likely culprit
    * DC suggested that the trends diagnostics may be useful, and that it might be worth trying a simpler tracer advection scheme 
    * <ins>We should run an eORCA025-RK3 experiment with 4th-order centered tracer advection instead of 4th order FCT and with the trends diagnostics activated. See Actions.</ins>

</ul>
    
#### GOSI workflows - Monsoon3 (DC)

<ul>

DC has ported the GOSI10.beta.4 eORCA025 suite ([u-dq383](https://code.metoffice.gov.uk/trac/roses-u/log/d/q/3/8/3/trunk)) to Monsoon ([u-ds535](https://code.metoffice.gov.uk/trac/roses-u/log/d/s/5/3/5/trunk)).
  
AB asked about the changes required to port the suite (he needs to port one of Diego's NEMO 4.2 suites).

  * The changes required for this suite are shown [here](https://code.metoffice.gov.uk/trac/roses-u/changeset/331415/d/s/5/3/5/trunk) (with some [bug fixes](https://code.metoffice.gov.uk/trac/roses-u/changeset/331419/d/s/5/3/5/trunk) potentially needed, and note that [this more recent change to fcm_make_pp](https://code.metoffice.gov.uk/trac/roses-u/changeset/331539/d/s/5/3/5/trunk) should be used instead).
  * The main challenge has been getting the required input files approved and onto Monsoon

<br>

CG asked whether there was a formal procedure in place for getting files approved and onto Monsoon.

  * Not yet to our knowledge- it's still rather ad-hoc.  See Actions.

<br>

Some other remarks on the Monsoon system:
  
  *  Queue times have been fairly long so far (30m to 2hr for eORCA025)
  *  All work is submitted from within a cylc server, but currently there is only one of these and it sometimes goes down
  
</ul>

#### Update on nemo_cookbook and long-term integration plan with `MARINE_VAL` (OT)

<ul>

OT introduced the [NEMODataTree class](https://noc-msm.github.io/nemo_cookbook/nemodatatree/) to be used for recipes in the NEMO cookbook.

  * At the in-person NEMO developers meeting in June, there was uncertainty as to whether we could leverage the established [xgcm](https://xgcm.readthedocs.io/en/latest/) package for this purpose. In the end, concerns over its long-term maintainability meant that a different approach was needed.
  * The `NEMODataTree` class has various fundamental [analytical and functional methods](https://noc-msm.github.io/nemo_cookbook/reference/) that are [used in the cookbook recipes](https://noc-msm.github.io/nemo_cookbook/recipe_moc_z/#calculating-the-amoc-vertical-overturning-stream-function).

<br>

It was noted that members of the NEMO Systems Team have expressed interest in testing this.

</ul>

#### JRA55 river runoff results (AW)

<ul>

AW presented results from an eORCA025 GOSI10p2 experiment using daily mean river runoff from JRA55- see the [ocean](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ocean/u-dn902_ocean_vs_u-dm348_ocean/assess.html) and [ice](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ice/u-dn902_si3_vs_u-dm348_si3/assess.html) validation notes.

  * The [JJA mean sea surface salinity](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ocean/u-dn902_ocean_vs_u-dm348_ocean/u-dn902_ocean_u-dm348_ocean_jja_20081201_20181130_20081201_20181130_mean_sss_abs_vs_WOA13v2_salinity_teos10_global.png) shows large differences in the Amazon outflow and Kara Sea, as well as large-scale differences elsewhere.
  * The [AMOC](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ocean/u-dn902_ocean_vs_u-dm348_ocean/u-dn902_ocean_u-dm348_ocean_1y_ts_amoc_26n_atlantic.png), [global sea surface salinity](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ocean/u-dn902_ocean_vs_u-dm348_ocean/u-dn902_ocean_u-dm348_ocean_1y_ts_sss_abs_global.png) and [global volume-averaged salinity](https://gws-access.jasmin.ac.uk/public/jmmp/valid_ocean/u-dn902_ocean_vs_u-dm348_ocean/u-dn902_ocean_u-dm348_ocean_1y_ts_s_abs_full_global.png) show significant changes

<br>

MB asked whether monthly averages of the daily JRA55 runoff were similar in magnitude to the monthly climatology used in the reference experiment.

  * AW noted that there wasn't much difference in annual averages, so the differences may be emerging at shorter timescales
  * MB and OT noted that as a result, it might be interesting to look at differences in the seasonal cycle of the runoff

<br>

AB noted that in climatological runoff datasets there is usually some effort to distribute the runoff over several grid points, whereas in the JRA55 runoff dataset it is all put into the nearest coastal grid point.

  * AW may perform a follow-up experiment in which the JRA55 forcing is instead spread out over several grid points
  
</ul>

----------

## Actions:
  * Carried forward from previous meeting:
    * (AB) Check and send any details on strong -ve temperatures at ice shelf to SK and CP.
    * (All) Be proactive about transferring workflows to Monsoon3.
  * (AA) Ask Met Office Science IT
    * Replace current ad hoc method with a formal procedure to review ancillary files licenses and get these onto Monson
    * For development configurations, where the ancil files are not finalised, can we agree on terms to share these files (with NOC) without approval, or using a generic licence/agreement?
  * Met Office team to run further experiments looking into the timestep dependence of NEMO 5
    * eORCA025 + RK3
      * 15m timestep
      * 45m timestep
      * Richardson number vertical mixing scheme
      * Centered tracer advection scheme
    * eORCA1 + RK3
      * 2hr timestep
      * Richardson number vertical mixing scheme
  * DC to provide Sebastien Masson with the namelists and `ocean.output` files for CG's experiments
