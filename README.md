# Stenosed channel — P2–P1 IPCS

Silas Houessou · September 2026

A straight two-dimensional channel with a cosine stenosis, solved with Taylor–Hood finite elements (P2 velocity, P1 pressure) and the Incremental Pressure Correction Scheme. The fluid has nowhere else to go, so severity is paid as a pressure drop and a wall-shear peak.

The written report is [`Rapport_Hemodynamique_Stenose_HOUESSOU.docx`](Rapport_Hemodynamique_Stenose_HOUESSOU.docx). This repository holds that report and the plates. The solver is not published here.

## Question

At fixed inlet flux, how does a cosine constriction of amplitude 0 %, 50 % or 75 % change the pressure drop and the wall shear, and does a shear-thinning Carreau law change the answer?

Lubrication theory already sketches the Newtonian part: on a two-dimensional slot, wall shear at fixed flux grows like the inverse square of the residual height. The computation checks whether convection, a cosine that is not slowly varying, and a non-linear viscosity destroy that sketch. They do not.

## Model

- Domain: unit-height channel, cosine stenosis built into the contour.
- Inlet: steady Poiseuille profile, peak velocity 1, exact flux \(2/3\).
- Wall: no-slip. Outlet: do-nothing / pressure reference.
- Elements: Taylor–Hood P2–P1 on triangles.
- Time: IPCS (Chorin–Goda), \(\Delta t = 0.02\), marched to a viscous steady state.
- Rheology: Newtonian campaign, then the same geometries with a Carreau law.

## Campaign, working mesh

Newtonian reference, grid \(120 \times 36\). Peak velocity, pressure drop and maximum wall shear:

| Stenosis | \(U_\max\) | \(\Delta p\) | WSS max |
|---|---:|---:|---:|
| 0 % | 1.000 | 64.0 | 3.92 |
| 50 % | 1.95 | 106 | 17.0 |
| 75 % | 3.91 | 306 | 63.4 |

The healthy row recovers the Poiseuille certificate (\(U_\max = 1\), WSS \(\approx 4\)). A 75 % throat multiplies the wall shear by about sixteen and the pressure drop by about five.

Carreau, same geometries: the jet still forms, but the peaks fall (WSS max 5.5 / 14.8 / 41.7 at 0 / 50 / 75 %). Shear-thinning does not remove the throat; it lowers the bill.

## Mesh study

Three structured grids, \(n_y = 18,\ 36,\ 54\). On the 75 % case the pressure drop moves from 298 to 306 to 307, and the wall-shear peak from 65.5 to 63.4 to 62.4. The mechanism is stable. The single digit 63 is a campaign measurement, not a mesh-independent constant.

## What is in `figures/`

| File | Content |
|---|---|
| `geo_s00.png`, `geo_s50.png`, `geo_s75.png` | Geometry at 0 / 50 / 75 % |
| `mesh_s00.png`, `mesh_s50.png`, `mesh_s75.png` | Meshes |
| `campaign_geometries.png` | The three contours on one plate |
| `poiseuille_glyphs.png`, `s50_glyphs.png`, `s75_glyphs.png` | Velocity glyphs |
| `glyphs_s75_newt.png` | Newtonian 75 % jet |
| `s00_wss.png`, `s50_wss.png`, `s75_wss.png` | Wall shear along the wall |
| `campaign_wss.png`, `campaign_wss_newtonian.png` | Campaign plates |
| `wss_s00_newt.png`, `wss_s50_newt.png`, `wss_s75_newt.png` | Newtonian WSS |
| `loi_carreau.png` | Carreau viscosity against shear rate |
| `convergence.png` | \(U_\max\), \(\Delta p\) and WSS against \(h\) |

Preview plates `00_*` to `03_*` are the early jury sketches, not the campaign results.

## How to read it

Start with the report. Then `campaign_geometries.png`, the 75 % glyphs, `campaign_wss_newtonian.png`, and `convergence.png`. Flux is imposed. The interesting quantities are \(\Delta p\) and the throat shear.
