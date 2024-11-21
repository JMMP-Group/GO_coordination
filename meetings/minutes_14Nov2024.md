# JMMP global ocean model development meeting minutes

14th Nov 2024.

Participants: Ana Aguiar (AA - chair, minutes), Catherine Guiavarc'h (CG), Dave Storkey (DS), Diego Bruciaferri (DB), 
Isabella Ascione (IA), David Schroeder (DSc), Emma Fiedler (EF), Daley Calvert (DC), Mike Bell (MB)

Apologies: Alex Megann, Amber Walsh, Adam Blaker, Chris Wilson, Richard Renshaw, Kristian Mogensen, Sarah Keeley, Ed Blockley.

----------

## Agenda:
1. Review actions and approve the minutes from [26 Sep 2024](https://github.com/JMMP-Group/GO_coordination/blob/main/meetings/minutes_26Sep2024.md)
2. (Isabella) First results of GOSI10 with NEMO5 (MLF)
3. (Diego) Results from the run with TRIADS and the new htau
4. Rota for minuting our meetings
5. AOB

----------


## Review actions from previous meetings

1. **Action 4.1: CG to contact Rachel Killick (Cc AB) regarding EN4 issues at the poles.** _Ongoing_

----------

## Progress with GOSI10

</br>

2. _GOSI10 with NEMO5RC_: IA presented results from validation notes and slides available from [issue#22](https://github.com/JMMP-Group/GO_coordination/issues/22#issuecomment-2483238276) 
  1. AMOC weakens and there's freshening and cooling of the North Atlantic
  2. Sea ice extent doesn't change much but sea ice seems thicker. EF and DSc suggested changing the albedo and snow conductivity which previously have been tuned to increase sea ice thickness and may no longer need to be as high. DSc suggested that SIT changes in Mar and Sep seem to be thermodynamically driven, originating in the sea ice model, not the ocean. There's less heat loss in te SPG and Indian Ocean (net surface heat flux maps). NEMO5RC should still be using momentum advection vector invariant form but the stirring/strength of the currents appears to have changed. IA will arrange a meeting with sea ice people to decide what parameters and values to test in (new) sensitivity experiments. 

3. _Results from the run with TRIADS and new nn\_htau_: see e-mail that DB sent ahead of the meeting with slides. DB will run a new experiment with nn_htau profile that's closer to estimates based on observations. There was some discussion about replacing TKE with GLS (Generic Length Scale) mixing as for SE-NEMO.

4. _Rota for minuting_, to follow participants' surname in alphabetical order: IA, AB, EB, DB, DC, EF, CG, AM, DSc, DS, AW. Ana will name next person on the rota along with agenda for each meeting and rearrange in case that person cannot be present. 

----------

## Actions

* 4.1: CG to contact Rachel Killick (Cc AB) regarding EN4 issues at the poles.
* 5.1: AA to send out agenda ~1 week ahead of the next meeting (13th Feb) and ask IA to minute the meeting.
