# JMMP GO model development meeting minutes

*Thursday 5th February 2026*

*Participants:* 

Ana Aguiar (AA - chair), Isabella Ascione (IA), Daley Calvert (DC), Emma Fiedler (EF), Catherine Guiavarc'h (CG), Sarah Keeley (SK), Alex Megann (AM), Kristian Mogensen (KM), Charles Pelletier (CP), Dave Storkey (DS), Dan Lea (DL), Amber Walsh (AW), Oliver Tooth (OT),  Emma Young (EY)

*Apologies:* 
Mike Bell (MB), Diego Bruciaferri (DB - minutes), Adam Blaker (AB), Richard Renshaw (RR), David Schroeder (DSc), Matt Martin (MM), Davi Mignac (DM). 


*The rota for minuting follows JMMP people surname in alphabetical order: IA, AB, DC, EB, DB, <ins>EF</ins>, CG, AM, DSc, DS, OT, AW. (This was listed in the minutes of Nov 2024).  We only minute the quarterly meetings, not the monthly ones.*

----------

## Agenda:

1. Review minutes and pending actions of the last meeting, see https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/minutes_20Nov2025.md
2. GOSI10-ORCA1 workplan: https://github.com/JMMP-Group/GO_coordination/issues/37
3. Decision on GOSI10-RK3
4. Impacts of the Smagorinsky viscosity in numerical mixing (Alex Megann)
5. Update on SI3 developments (Emma Fiedler)
6. AOB


## Actions from last meeting:

- [x] (All) Be proactive about transferring workflows to Monsoon3.
    * All GOSI9 ancil files are on Monsoon3 as well as two (ORCA1 and ORCA025) GOSI9 workflows (IA).
* [ ] (AM) Share numerical mixing paper draft.
    * Not shared yet, AA will try to share it on the Teams page.
* [x] (AA) Organise meeting to discuss ORCA1 GOSI10 release.
    * The meeting was organised, see minutes below.
* [x] (All Met Office) Meet to discuss RK3.
    * The meeting was organised, see mintues below. It has been decided to run ORCA025 with RK3 and a timestep of 30 min, while for ORCA12 we hope to be able to use RK3 and doubling the current timestep (still to be fully tested). In addition, it has been decided to not use Smagorinsky viscosity for the moment.
* [x] (All speakers) Upload slides to the meeting minutes.
* [x] (AA) Send out agenda ~1 week ahead of the next quarterly meeting (Feb 5th, pending quorum) and ask DB or EB to minute the meeting.

----------

## Minutes:

#### GOSI10-ORCA1 workplan (AA)

<ul>

After the meeting on ORCA1 was held, a workplan was drafted (see https://github.com/JMMP-Group/GO_coordination/issues/37) and AB agreed on leading this work (likely starting after April).

<br>

</ul>

#### Decision on GOSI10-RK3 (AA)

<ul>

Regarding GOSI10 and RK3, it was decided to 

* run ORCA025 with the same timestep of 30 min currently used with MLF;
* fully test ORCA12 with a timestep twice as large as the one currently used with MLF - preliminary results appear promising, but a full assessment is still required. 

In addition, it has been decided to not use Smagorinsky viscosity for the moment.

<br>

</ul>

#### Impacts of the Smagorinsky viscosity in numerical mixing (AM)

<ul>

AM presented some results from his analysis on the impact of Smagorinsky viscosity (SMAG) on numerical mixing in GOSI10.beta.4.

* IA run 4 experiments of GOSI10.beta.4 with RK3, 5d outputs (needed for AM's analysis) and varying the timestep (20 min, 60 min or 30 min) and activating or not Smagorinsky viscosity (see [slides](https://github.com/JMMP-Group/GO_coordination/issues/36#issuecomment-3854188378) for the full table).
* AM uses the same diagnostics of GC5 paper, based on the density transformation framework (see Lee et al. 2002). 
* The same analysis is applied to a small set of GOSI9 experiments as well as to the 4 experiments of IA with GOSI10.beta.4.
* GOSI9 main results: reducing the timestep (from 30 min to 20 min) causes a small but robust reduction (~2%) in numerical mixing; activating SMAG causes a consistent reduction in spurious diapycnal mixing (~8-10%).
* GOSI10.beta.4 main results: activating SMAG and keeping the same timestep causes a reduction in numerical mixing of ~11% wrt control. Activating SMAG and reducing (20 min) or increasing (60 min) the timestep causes a reduction (16%) or increase (1%) in numerical mixing.
* AM thinks that the warming happening in GOSI10.beta.4 with RK3=30min could be due to compensating errors and wants to continue investigation on SMAG (perhaps to be implemented in GOSI11 - if so, CG suggests to do it at the early stages of the development cycle so that further tuning is not needed later). 

<br>

</ul>

#### Update on SI3 developments (EF)

<ul>

EF presented the latest upgrades on the sea ice dynamics for GOSI10, which aim at improving sea ice model effective resolution and representation of landfast (immobile) ice (see [slides](https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/slides/GOSI10_beta5_JMMP_05022026.pptx)). 

* Update on supercooled ocean issues:
  Latent heat from icebergs was driving down SST below freezing point, resulting in basal ice growth. CG and DC made the changes to implent in GOSI10 a recent fix went in NEMO for this issue (further changes required for the fix in coupled model). This fix also changes how the iceberg flux is distributed.
* Sea ice developments - results:
  a. Sea ice thickness: increase in number and definition of sea ice features; sea ice is thinner overall compared to GOSI10.beta.4.
  b. Sea ice rehology: improved definition of sea ice fracture patterns for R75 strength, aEVP gives the largest improvements.
  c. Sea ice strength: increased where there is landfast ice. R75 strength is lower than H79 in autumn, spring and summer much closer in magnitude.
  d. SIC ab drift speed: differences wrt control are mainly along sea ice edge.
  e. Landfast ice: very qualitative assessment seems to show improvements in comparison to GOSI10.beta.4.
  f. Validation notes for ice: improvements in the right direction to compare better with obs.
  d. Validation notes for oce: TBD  
* Still to do for GOSI10.beta.5 release:
  a. Validation notes for oce
  b. Impact of ICB fix on long runs -> testing switchin off basal ice growth cap in 65 years long runs:
     * ICB fix + basal growth cap
     * ICB fix + no basal growth cap
* Planned sea ice devs fpr GOSI11:
  a. from NEMO5 physics: ocean-ioce form drag and improved brine drainage and flushing param.
  b. increase num of model snow and ice layers;
  c. Reduce max southern hemisphere sea ice frac -> improves the ocean
  d. interactions melt-ponds/ridging
* GOSI10.beta.5 planned for Feb (if everything goes as expected).
* AA asked what would be the imapct of these changes on GC5; EF said it has to been seen, but she is confident we have quite a lot of flexibility for further tuning so she is not too much worried.
* SK reports ECMWF previous experience with early version of aEVP, saying that the model was unstable. EF replies that she doesn't think the code of aEVP has been updated, but all the other changes to the model code have made the aEVP's issues of the past to disappear.

<br>

</ul>

#### Update on rivers from JRA (AW)

<ul>

AW is working right now on the updates for eORCA025 and later on she will pass to eORCA12. Because of the different coastline between eORCA12 and eORCa025, CG suggested to remap directly the original JRA rivers rather than interpolating from eoRCA025.
  
<br>
  
</ul>


----------

## Actions:

  * (EF)  Update on sea ice developments.
  * (AW)  Update on rivers updates. 
  * (All) Update on GOSI10.beta.6.
  * (All speakers) Upload slides to the meeting minutes.

