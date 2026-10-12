# allroad-lowtraffic

A [BRouter](https://github.com/abrensch/brouter) routing profile for allroad and gravel bikes ridden at road-bike pace. It avoids main roads and busy traffic, allows smooth gravel and compacted forest roads, and favours big signed cycle routes. Tested around Berlin and Brandenburg.

## What it does

| Way type | Behaviour |
|---|---|
| Busy primary, secondary, tertiary roads (`estimated_traffic_class` 4–7 or no estimate) | Avoided: higher base cost, traffic penalty, penalty for 70–100 km/h limits |
| Quiet primary, secondary, tertiary roads (`estimated_traffic_class` 1–3) | Allowed at about the cost of unclassified roads, whatever the speed limit |
| Bike paths and sidewalks of paving stones, sett or cobbles (`cycleway:surface`, else `surface`) | Used only where no other way exists |
| Roads with a bike track mapped on the road (`cycleway*=track`) | Treated like a bike path only with `cycleway:surface=asphalt\|concrete`; otherwise costed as the road |
| Roads with a painted bike lane (`cycleway*=lane`, solid or dashed line) | Treated like a bike path |
| Big signed cycle routes (EuroVelo, national, regional) | Preferred when paved and not paving stones |
| Smooth gravel (`compacted`, `fine_gravel`, `gravel` on grade1–3 tracks) | Preferred |
| Cobblestones / sett on roads | Avoided |
| Turns | Cost 150 per 90° turn, against detours around single blocks |
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
| `CycleTrackCostfactor` | 1.3 | Base cost of a road with an asphalt bike track or a painted bike lane |
| `MainRoadFactor` | 2.0 | Multiplier for base costs of busy main roads |
| `PavedPenalty` | 2.0 | Extra cost on paved ways; higher = more gravel |
| `CobblePenalty` | 5.0 | Extra cost on cobblestones / sett |
| `PavingStonesPenalty` | 20.0 | Extra cost on bike paths and sidewalks of paving stones, sett or cobbles |
| `TurnCost` | 150 | Cost of a 90° turn; higher values also keep routes longer on busy roads |
| `CycleRoutePavedShare` | 0.5 | Share of `PavedPenalty` paid on big cycle routes; 0 = routes preferred, 1 = no route bonus |
| `consider_elevation` | false | Penalise climbs and descents |
| `ferries_allowed` | false | Allow ferries |
| `allow_unpaved_paths` | false | Treat dirt/ground paths marked for bikes like bike paths: more forest, risk of sandy patches |

## Test results

BRouter 1.7.11-beta, routing data `E10_N50.rd5` downloaded 2026-09-21, default settings of both profiles. Totals over 9 routes:

| | Rennrad (sehr wenig Verkehr) | allroad-lowtraffic |
|---|---|---|
| Total distance | 221.7 km | 239.9 km |
| Unpaved share | 0 % | 25 % |
| On big signed cycle routes | 78.1 km | 114.6 km |
| On smooth gravel | 0.2 km | 57.3 km |
| Bike paths and sidewalks of paving stones, sett or cobbles | 14.0 km | 1.1 km |
| Busy main roads (`estimated_traffic_class` ≥ 4) without bike lane or asphalt track | 34.7 km | 9.4 km |
| Busy main roads with bike lane or asphalt track | 16.6 km | 9.2 km |
| Quiet main roads (`estimated_traffic_class` ≤ 3) | 12.8 km | 30.8 km |
| Cobblestones / sett on roads | 2.2 km | 1.6 km |
| Sand | 0 km | 0 km |
| Turns over 60° per 100 km | 70 | 75 |

Routes: Alexanderplatz–Bernau, Kreuzberg–Potsdam, Köpenick–Erkner, Oranienburg–Pankow, Königs Wusterhausen–Neukölln, Bernau–Eberswalde, Potsdam–Werder, Erkner–Fürstenwalde, Oranienburg–Liebenwalde.

## Search speed

The profile sets `pass1coefficient 8` and `pass2coefficient 2`. Most ways here cost well above 1 per metre (about 4.3 on average over 6 test routes), so BRouter's default estimate of 1.5 makes the search explore far too much.

Measured with BRouter 1.7.11-beta on an idle server:

| Setting | 6 routes (Berlin, Brandenburg) | Routes |
|---|---|---|
| defaults | 11.3 s | reference |
| `pass1coefficient 8` | 7.4 s | identical |
| `pass1coefficient 8`, `pass2coefficient 2` | 5.9 s | identical |
| `pass1coefficient 8`, `pass2coefficient 4` | 3.1 s | total cost +0.05 % |

The chosen setting returned identical routes (same cost, length and points) on 40 routes: the 9 test routes above and 31 random pairs across Berlin and Brandenburg.

## Credits

Based on bikerouter.de's "Rennrad (sehr wenig Verkehr)" profile, itself derived from BRouter's fastbike profile.
