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
| Cobblestones / sett | Avoided |
| Dirt, ground or untagged tracks; dirt or ground paths marked for bikes | Allowed at moderate cost |
| Sand, mud and grass tracks and paths, grade4–5 tracks, unpaved paths without bicycle designation | Avoided |
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
| `CobblePenalty` | 3.0 | Extra cost on cobblestones / sett |
| `CycleRoutePavedShare` | 0.5 | Share of `PavedPenalty` paid on big cycle routes; 0 = routes preferred, 1 = no route bonus |
| `consider_elevation` | false | Penalise climbs and descents |
| `ferries_allowed` | false | Allow ferries |
| `allow_unpaved_paths` | false | Treat dirt/ground paths marked for bikes like bike paths: more forest, risk of sandy patches |

## Test results

BRouter 1.7.10, routing data `E10_N50.rd5` of 2026-10-04, default settings of both profiles. Totals over 9 routes:

| | Rennrad (sehr wenig Verkehr) | allroad-lowtraffic |
|---|---|---|
| Total distance | 212.9 km | 243.7 km |
| Unpaved share | 2 % | 30 % |
| On big signed cycle routes | 46.5 km | 125.4 km |
| On smooth gravel | 0.2 km | 63.6 km |
| Primary / secondary roads without bike track | 38.0 km | 3.8 km |
| Other roads with estimated traffic class ≥ 4 | 9.7 km | 1.9 km |
| Cobblestones / sett | 1.3 km | 2.0 km |
| Sand | 0 km | 0 km |

Routes: Alexanderplatz–Bernau, Kreuzberg–Potsdam, Köpenick–Erkner, Oranienburg–Pankow, Königs Wusterhausen–Neukölln, Bernau–Eberswalde, Potsdam–Werder, Erkner–Fürstenwalde, Oranienburg–Liebenwalde.

## Credits

Based on bikerouter.de's "Rennrad (sehr wenig Verkehr)" profile, itself derived from BRouter's fastbike profile.
