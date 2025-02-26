# JMMP global ocean model development meeting minutes

13th Feb 2025.

Participants: Ana Aguiar (AA - chair, minutes), Isabella Ascione (IA - minutes), Mike Bell (MB), Adam Blaker (AB), Diego Bruciaferri (DB), Daley Calvert (DC), Emma Fiedler (EF), Catherine Guiavarc'h (CG), Alex Megann (AM), Kristian Mogensen (KM), Richard Renshaw (RR), David Schroeder (DSc), Dave Storkey (DS), Oliver Tooth (OT), Amber Walsh (AW)

Apologies: Chris Wilson, Sarah Keeley, Ed Blockley.

----------

## Agenda:
1. Review actions and approve the minutes from [14 Nov 2024](https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/minutes_14Nov2024.md)
2. Progress with GOSI10
3. AOB

----------

## Review actions from previous meetings

1. **Action 4.1: CG to contact Rachel Killick (Cc AB) regarding EN4 issues at the poles.** _Closed_

AB discussed this with Rachel Killick after the NA/AMOC meeting at the end of Sep 2024. Rachel is is aware of the issues with EN4, but there is no plan to improve these. The issues might still be present in the new version which is about to be released. (CG) This is not a problem for GOSI10 since we adopted WOA for ICs and validation but we (JMMP-GO) should keep track of EN4 developments. (AM) Issues in the Arctic are unclear and there are issues at the surface near Greenland.  

**Action 5.1: AA to send out agenda ~1 week ahead of the next meeting (13th Feb) and ask IA to minute the meeting** _Closed_

----------

## Progress with GOSI10

</br>

* (AM) _GOSI10 on Monsoon_: Progress was made with 30yrs control run now ready. Reduced time step to 1200 sec for stability; 1800 sec caused a crash.
 
* (AB) _River runoff_: Sarah Wakelin [SW] produced JRA river runoff. SE-NEMO runs without iceberg data as we currently use. AB asked SW to extend the dataset which presently only covers 1976 to mid 2020. JRA runoff is daily varying, unlike climatology (AB/AW).
 
* (DSc) _Improve parameterization in SI3 using satellites_: Parameterizations are present in NEMO5, not NEMO4.2. Scheme to be tested in the coupled system (DSc,AA). EF introduced ice strength parameterization in NEMO4.2 and this is stable. Form drag and EVP rheology parameterization arework in progress (EF, DSc).
 
* (KM) _ECMWF_ Testing NEMO5 with short runs at different resolutions for sanity checks.
 
* (RR) _Reanalysis_ Glosea running global reanalysis with GOSI9, using ERA5 as forcing. RR is running the same reanalysis but for a continuous period instead of ~5yrs chunks.
 
* (CG) _GOSI10p2-ORCA12_ running, along with GOSI10p3-ORCA025 since November. Waiting for access to MASS to run validation notes. IA adapting GOSI10 to run on the new HPC. GOSI10p at NEMO5rcis performing well with RK3 (CG, IA). Might need a change in the time step for ORCA12. AB ran ORCA12 with a 300 sec time step.

* (DB), AM, CG) _ORCA025 GOSI10p3_ Nikesh trying running a coupled experiment (GOSI9) with TRIADS. No impact in forced mode. AMOC increased by 1 Sv.
 
* (DS) _Southern Ocean paper_ Bathymetry release. (EF) Changes in bathymetry affect sea ice. DS intends to port ORCA1 at 4.2.2 to the new Met Office HPC to replicate a UKESM control run for his work on ocean spin-up.
 
* (DB, AM) _Validation tools_ Improve MarineVal, ValNA, ValSO software to be independent from CDFtools. MarineVal available on JMMP github.
 
* (DC) _NEMO on GPUs_ NGARCH project benchmark for Gen1 supercomputer. NEMO 4.0.4 ORCA1 runs faster on GPUs. XIOS cannot run on GPUs, causing data transfer issues.
 
*(AA) _NEMO meeting in June_ Invite to join the hackathon in June.

----------

## AOB 

Next person to take minutes: AB

----------

## Actions

* 6.1: AA to send out agenda ~1 week ahead of the next meeting (DATE HERE) and ask AB to minute the meeting.
