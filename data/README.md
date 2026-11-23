# Coexistence from a lion’s perspective: movements and habitat selection by African lions (Panthera leo) across a multi-use landscape

<https://doi.org/10.5061/dryad.j6q573nnb>

The dataset represents movements of 22 collared lions in the Ngorongoro Conservation Area, Tanzania. The data have been processed and prepped for step-selection analysis by using the the amt package (Signer et al., 2019) in Program R to estimate each animal’s movement parameters for step length and turn angle, then sampled from these parameter distributions to generate random available locations (n=50 locations) around each used location. Used and available locations are denoted in the "case_" column. The available locations are conditioned on the last step taken by the animal, meaning they are relative to the current location, resulting in a stratified dataset with stratification noted in the "step_id_" column.

## Description of the data and file structure

Column Descriptions:

"case_": True/False whether the row represents a used step

"id": The unique ID of the lion

"step_id_": The ID_step stratification

"Dry_season": 0/1 whether sample occurred in the Wet (Dec-May) or Dry (Jun-Nov) season

"Night": 0/1 which sample occurred at night or during the day, with sunset determined using the suncalc package in Program R

"Dispersal_movement": 0/1 whether the sample was part of a long-distance dispersal movement

"Male": 0/1 whether the lion is male (or female)

"Land_cover": The landcover class of where the step ended (1=Open, 2=Forest, 3=Shrub)

"ForShrub": The percent of a 50m radius around the step that is covered by either Forest or Shrub habitat

"Dist_river": The distance to the nearest river (m)

"TRI": Terrain roughness index produced from USGS EROS digital elevation model using the terrain ruggedness index of Riley et al., 1999

"Dist_human": The distance to the nearest bomas/buildings (m)

"Dist_water": The distance to the nearest source of water (m)

"Dens_human_1k": The density of bomas/buildings in a 1km radius around the point

"Dens_human_3k": The density of bomas/buildings in a 3km radius around the point

"Dens_human_5k": The density of bomas/buildings in a 5km radius around the point

"EVI": MODIS monthly vegetation Indices (MOD13A3) Version 6. We summarized EVI for wet (Dec-May) and dry (Jun-Nov) seasons, respectively, by averaging over each year from 2012-2022
