# JMMP GO model development meeting minutes

*Thursday 20th November 2025*

*Participants:* 

Ana Aguiar (AA - chair), Isabella Ascione (IA), Adam Blaker (AB), Daley Calvert (DC), Emma Fiedler (EF - minutes), Catherine Guiavarc'h (CG), Sarah Keeley (SK), Alex Megann (AM), Kristian Mogensen (KM), Charles Pelletier (CP), Dave Storkey (DS), Oliver Tooth (OT),  Emma Young (EY)

*Apologies:* 
Mike Bell (MB),  Diego Bruciaferri (DB),  Richard Renshaw (RR), David Schroeder (DSc), Amber Walsh (AW), Matt Martin (MM), Dan Lea (DL), Davi Mignac (DM). 


*The rota for minuting follows JMMP people surname in alphabetical order: IA, AB, DC, EB, DB, <ins>EF</ins>, CG, AM, DSc, DS, OT, AW. (This was listed in the minutes of Nov 2024).  We only minute the quarterly meetings, not the monthly ones.*

----------

## Agenda:

1. Ongoing investigations on the dependency of NEMO-RK3 on time-step (Daley Calvert)
2. GOSI10 sea ice updates (Emma Fiedler)
3. Key messages of GC5 numerical mixing paper submitted to J. Clim (Alex Megann)
4. AOB


## Actions from last meeting:

- [x] (AB) Check and send any details on strong -ve temperatures at ice shelf to SK and CP.
- [ ] (All) Be proactive about transferring workflows to Monsoon3.
    * Mostly done, IA is finishing review of ancils for GOSI9, suite requires install app update. Daley involved too
- [x] (AA) Ask Met Office Science IT:
    - [x] Replace current ad hoc method with a formal procedure to review ancillary files licenses and get these onto Monsoon
        * Ben Fitzpatrick working on formal agreement.
    - [x] For development configurations, where the ancil files are not finalised, can we agree on terms to share these files (with NOC) without approval, or using a generic licence/agreement?
        * Ben Fitzpatrick advised to license and review the ancil files upfront, before copying these to Monsoon; no need to review these once the ancil files are finalised, unless there are significant changes to content that used new/distinct data sources that should trigger an update of either the attribution or licence file.
- [x] (Met Office team) To run further experiments looking into the timestep dependence of NEMO 5
  * eORCA025 + RK3
      - [x] 15m timestep
      - [x] 45m timestep
      - [x] Richardson number vertical mixing scheme
      - [x] ~~Centered~~ MUSCL tracer advection scheme
  * eORCA1 + RK3
      - [x] 2hr timestep
      - [x] Richardson number vertical mixing scheme
- [x] (DC) to provide Sebastien Masson with the namelists and `ocean.output` files for CG's experiments.

- [x] (AA) Send out agenda ~1 week ahead of the next quarterly meeting (20th November) and ask EB to minute the meeting.



----------

## Minutes:

#### Ongoing investigations on the dependency of NEMO-RK3 on time-step (DC)

<ul>

DC presented his latest findings on NEMO-RK3.

Response to increased time step

* Increasing the time step from 30 to 60 minutes in ORCA025 using RK3 causes pronounced subsurface warming in the tropics, especially in the Equatorial Pacific. 
* RK3 allows doubling of the time step without crashing, but the temperature departure wrt standard time step varies by resolution: significant in ORCA025; minimal in ORCA1 and ORCA12.
* ORCA025 seems to be a singular case in that doubling the time step pushes the stability very close to its threshold without crashing. Attempts to further increase the time step led to instability and crashes for ORCA1 and ORCA12. ORCA025 can run with 45 min time step and no temperature anomalies but this time step cannot be used in ocean-atmosphere coupled systems.
<br>

Advection and Momentum Schemes (all these experiments use a GOSI10-like NEMO 5.0 configuration and are documented on [issue #34](https://github.com/JMMP-Group/GO_coordination/issues/34))

* Changing the tracer advection scheme (from 4th order FCT to 2nd order) did not resolve the warming issue.
* Switching to the MUSCL tracer advection scheme increased diffusion and worsened biases.
* Changing the momentum advection scheme from vector form to UP3 removes the timestep-dependence of RK3 with ORCA025. This is probably because UP3 includes an implicit biharmonic viscosity which caps the largest velocities and prevents the tracer advection scheme from getting close to its stability limit. The group discussed whether UP3 should be adopted more widely, pending further validation and feedback from the NEMO systems team.
<br>

Validation and Further Experiments

* More experiments are needed, especially with GOSI10.beta.4.0-ORCA025, to test UP3 with multi-envelope coordinates in the Nordic Seas, but also with ORCA12 and AGRIF. The group agreed to test UP3 in these configurations and review validation notes, including AMOC transport, velocity, and other marine diagnostics. DC noted that validation notes are available on the GitHub issue. [Actions: OM team will meet again to define which experiments to run first and what validation is needed for UP3 acceptance.]
* The ORCA025 configuration is likely close to the CFL limit so small changes affect the stability.

<br>

</ul>

#### GOSI10 sea ice updates (EF)

<ul>

EF presented the latest updates on sea ice model development for GOSI10. [(slides)] (https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/slides/slides_JMMP_GOSI10_seaice_updates.pptx)

* Sea ice developments for the GOSI10.beta.5 release aim to improve effective sea ice model resolution and the representation of landfast ice.
* An issue was identified and fixed regarding instability along the north fold when using the landfast ice scheme, related to the updating of non-halo grid points along the north fold as part of lbc_lnk calls.
* Another ongoing issue involves supercooled ocean points. This is likely an ocean model problem rather than sea ice, possibly linked to steep bathymetry and non-conservation.
* Further investigation is planned, including generating ocean trends diagnostics and time step-level output.

<br>


</ul>
    
#### Key messages of GC5 numerical mixing paper submitted to J. Clim (AM)

<ul>

AM summarized his recent paper submission to Journal of Climate, which quantifies numerical mixing in HadGEM3-GC5 (configured with ORCA025 and an N96 atmosphere) and its influence on the ocean and atmosphere.

* Reduced numerical mixing can be achieved by adopting z~ coordinates, improving tracer advection accuracy, and increasing viscosity - either by scaling or using the Smagorinsky scheme.
* Reduced numerical mixing was associated with cooler sea surface temperatures (SST), though a warming occurred below the pycnocline. It also increased sea ice cover. Large-scale transports were improved, seen in improvements to the spinup/down[?] of the Antarctic Circumpolar Current (ACC).
* Both local and global atmospheric responses to the changes were found, with local latent heat changes and a global increase in shortwave radiation into the ocean, consistent with the reduced ice cover and cloud changes.
* Implications of these findings should be considered for future GOSI and GC configurations.

<br>
  
</ul>


----------

## Actions:

  * Carried forward from previous meeting:
    * (All) Be proactive about transferring workflows to Monsoon3. - Only GOSI9 workflow pending ancil files review which IA has passed to Ben Fitzpatrick for review.

  * (AM) Share numerical mixing paper draft.
  * (AA) Organise meeting to discuss ORCA1 GOSI10 release.
  * (All Met Office) Meet to discuss RK3.
  * (All speakers) Upload slides to the meeting minutes.
  * (AA) Send out agenda ~1 week ahead of the next quarterly meeting (Feb 5th, pending quorum) and ask DB or EB to minute the meeting.

