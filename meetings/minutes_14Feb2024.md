# JMMP global model development meeting minutes

14th February 2024.

Participants: Ana Aguiar (AA - chair), Alex Megann (AM), Amber Walsh (AW), Sarah Keeley (SK), Mike Bell (MB), Diego Bruciaferri (DB), Catherine Guiavarci'h (CG), Isabella Ascione (IA), Matt Menary (MM), Ed Blockley (EB), Dave Storkey (DS - minutes), Richard Renshaw (RR).

Apologies: Adam Blaker.

----------

## Agenda:
1. Review actions/approve minutes of the previous meeting 
   - AB said that Jérôme Chanut had told him about some small modifications to the ORCA025 grid around the north fo. Action: DS to email Jérôme about this?
   - Action: AA to circulate list of current issues a few days in advance of the 3-monthly meetings - not necessary as issues are mostly up-to-date. _Closed._
   - Action: AA to create JMMP GitHub repository for GO coordination - done. _Closed._
2. NOC-Southampton new member: Amber Walsh
3. GOSI10:
   - Highlights of progress or blockers regarding JMMP GO work plan
   - Initial conditions: Move from EN4 to WOA? Average over which time period? Update runoff inputs?
   - Atmospheric forcing: given issues with ERA5 should we use JRA (discontinued after Jan 2024) for GOSI10 to avoid hassle of not working with a standardised product?
   - Length of simulations: Should we run two cycles, e.g. run from 1958 (ICs) until 2023, then take restart from the end of first cycle and to start another run from 1958 to have a fully spun-up ocean? Tobias Schulzki's poster at DRAKKAR suggests that long runs (2 cycles, 40-50 yrs each) are needed to have a stable AMOC
      - Alex to show plot with AMOC and T drift in ensemble with different forcing
      -  Catherine to show plot with GO6 and GOSI9 T drift using CORE
4. AOB
   - Move series of meetings to Thu 10:30-12:00am going forward if convenient?

----------

## JMMP GO work plan review:

1. _GOSI10p0_
Released at NEMOv4.2.2 [needs bug fixes after restartability issues identified when switching on key_qco (a more efficient vertical coordinate algorithm that replaces VVL and significantly reduces the number of 3D arrays); applicable to both coupled and forced runs]

2. _New bathymetries_:

3. _Optimisation_:
   - Baseline development: NEMOv5 official release is pencilled in for Jun 2024 (first beta release has been delayed to Mar 2024). Target a GOSI10 prototype release at NEMOv5 in Dec 2024?

4. _Vertical mixing_:

5. _Numerical mixing_: 

6. _High resolution and AGRIF_: 

7. _SE-NEMO_: 

8. _Assessment tools_: 

----------

## AOB:

