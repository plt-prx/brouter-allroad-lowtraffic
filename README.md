# allroad-lowtraffic

A [BRouter](https://github.com/abrensch/brouter) routing profile for allroad and gravel bikes ridden at road-bike pace. It avoids main roads and busy traffic, allows smooth gravel and compacted forest roads, and favours big signed cycle routes. Tested around Berlin and Brandenburg.

## What it does

| Way type | Behaviour |
|---|---|
| Primary, secondary, tertiary roads | Avoided: higher base cost, traffic penalty from `estimated_traffic_class`, penalty for 70–100 km/h limits |
| Main roads with 30 km/h limit or lower | Avoided less, so short calm stretches are not bypassed with detours |
| Roads with a bike track mapped on the road (`cycleway*=track`) | Treated like a bike path |
| Big signed cycle routes (EuroVelo, national, regional) | Preferred when paved |
| Smooth gravel (`compacted`, `fine_gravel`, `gravel` on grade1–3 tracks) | Preferred |
| Dirt, ground or untagged tracks | Allowed at moderate cost |
| Sand, mud and grass tracks, grade4–5 tracks, unpaved paths without bicycle designation | Avoided |
| Climbs and descents | Free by default |

## Settings

Base cost of an asphalt cycleway = 1.0.

| Setting | Default | Effect |
|---|---|---|
| `GravelCostfactor` | 0.8 | Base cost of smooth gravel and compacted forest roads |
| `TrackUnpavedCostfactor` | 4.0 | Base cost of dirt, ground and untagged tracks |
| `UnknownTrackCostfactor` | 3.0 | Base cost of grade1/grade2 tracks without surface tag |
| `UnpavedCostfactor` | 25.0 | Base cost of all other unpaved ways |
| `CycleRouteCostfactor` | 1.0 | Base cost of paved ways on big signed cycle routes |
| `CycleTrackCostfactor` | 1.3 | Base cost of a road with a bike track mapped on it |
| `MainRoadFactor` | 2.0 | Multiplier for main road base costs above 30 km/h |
| `PavedPenalty` | 2.0 | Extra cost on paved ways; higher = more gravel |
| `CycleRoutePavedShare` | 0.5 | Share of `PavedPenalty` paid on big cycle routes; 0 = routes preferred, 1 = no route bonus |
| `consider_elevation` | false | Penalise climbs and descents |
| `ferries_allowed` | false | Allow ferries |

## Test results

BRouter 1.7.10, routing data `E10_N50.rd5` of 2026-10-04, default settings of both profiles. Totals over 9 routes:

| | Rennrad (sehr wenig Verkehr) | allroad-lowtraffic |
|---|---|---|
| Total distance | 212.9 km | 243.9 km |
| Unpaved share | 2 % | 32 % |
| On big signed cycle routes | 46.5 km | 124.1 km |
| On smooth gravel | 0.2 km | 63.2 km |
| Primary / secondary roads without bike track | 38.0 km | 4.6 km |
| Other roads with estimated traffic class ≥ 4 | 9.7 km | 1.8 km |
| Cobblestones / sett | 1.3 km | 2.3 km |
| Sand | 0 km | 0 km |

Routes: Alexanderplatz–Bernau, Kreuzberg–Potsdam, Köpenick–Erkner, Oranienburg–Pankow, Königs Wusterhausen–Neukölln, Bernau–Eberswalde, Potsdam–Werder, Erkner–Fürstenwalde, Oranienburg–Liebenwalde.

## Known limits

- BRouter compares cost per km, not stretch length. A short stretch of a 50 km/h main road can still cause a detour; a via point fixes it.
- `estimated_traffic_class` is BRouter's estimate from map data, not measured traffic.

## Credits

Based on bikerouter.de's "Rennrad (sehr wenig Verkehr)" profile, itself derived from BRouter's fastbike profile. Map data © OpenStreetMap contributors.

## Licence

To be confirmed with the bikerouter.de maintainer, since the base profile is theirs.
