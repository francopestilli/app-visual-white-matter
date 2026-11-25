# app-visual-white-matter

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Builds polar-angle-by-eccentricity visual cortex parcels from pRF maps and segments the tractogram streamlines that terminate in each parcel and visual area, using Connectome Workbench, FSL and MRtrix3.**

Projects the pRF polar-angle and eccentricity maps from the FreeSurfer surface into DWI space, bins them into wedges around the chosen polar-angle meridians and eccentricity steps, and combines them into a volumetric parcellation and cortexmap. In this single-endpoints variant it then assigns each streamline to the visual area and retinotopic bin of its endpoint with MRtrix3 and writes the per-bin tracts as a white matter classification. Use it to study visual white matter organised by visual-field position; needs a FREESURFER_LICENSE and inputs that share the subject's DWI space.

| | |
|---|---|
| 📥 **Inputs** | `neuro/dwi` · `neuro/freesurfer` · `neuro/prf` · `neuro/track/tck` · `neuro/parcellation/volume` |
| 📤 **Outputs** | `neuro/parcellation/volume` · `neuro/cortexmap` · `neuro/track/tck` · `neuro/tcks` · `neuro/wmc` · `neuro/tractmeasures` · `raw` |
| ⚙️ **Resources** | 8 CPUs · 3 h walltime |
| 🔑 **Requires** | `FREESURFER_LICENSE` |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.785](https://doi.org/10.25663/brainlife.app.785) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` | yes | Preprocessed DWI, used as the reference space for the parcellation. |
| `freesurfer` | `neuro/freesurfer` | yes | FreeSurfer subject directory used to map surface pRF data to the volume. |
| `prf` | `neuro/prf` | yes | pRF (population receptive field) maps on the surface, with polar angle and eccentricity. |
| `track` | `neuro/track/tck` | yes | Whole-brain .tck tractogram in DWI space. |
| `varea` | `neuro/parcellation/volume` | yes | Volumetric visual-area parcellation with its label.json. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `parc` | `neuro/parcellation/volume` | Volumetric polar-angle x eccentricity parcellation. |
| `cortexmap` | `neuro/cortexmap` | Surface (cortexmap) versions of the polar-angle x eccentricity bins. |
| `track` | `neuro/track/tck` | Tractogram of the streamlines assigned to visual-area / retinotopic bins. |
| `tracts` | `neuro/tcks` | One .tck per segmented bin. |
| `wmc` | `neuro/wmc` | White matter classification of the segmented streamlines. |
| `tractmeasures` | `neuro/tractmeasures` | Macrostructural measures of the segmented tracts. |
| `raw` | `raw` | Intermediate files (masks, MRtrix images, transforms and text outputs). |

<details>
<summary><b>⚙️ Parameters</b> (12)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `paMeridians` | string | `0 45 90 135 180` | Space-separated polar-angle meridians (degrees) around which wedges are built. |
| `paAngle` | string | `15` | Half-width (degrees) of each polar-angle wedge around its meridian. |
| `eccAngle` | string | `2` | Width (degrees) of each eccentricity bin from 0 to 8 degrees. |
| `inputparc` | enum | `aparc.a2009s` | FreeSurfer parcellation (aparc, aparc.a2009s or aparc.DKTatlas) used when building the cortexmap. Options: `aparc`, `aparc.a2009s`, `aparc.DKTatlas`. |
| `assignment_radial_search` | number | `4` | tck2connectome -assignment_radial_search distance; used only in the both_endpoints workflow, not in this App. |
| `assignment_reverse_search` | string |  | tck2connectome -assignment_reverse_search distance; used only in the both_endpoints workflow, not in this App. |
| `assignment_forward_search` | string |  | tck2connectome -assignment_forward_search distance; used only in the both_endpoints workflow, not in this App. |
| `inv_length` | boolean | `true` | Not read by the code. |
| `richClubPercentage` | number | `90` | Percentile for the rich-club coefficient; used only in the both_endpoints workflow, not in this App. |
| `analysis` | enum | `polarAngleMeridiansConnectomics` | Analysis pipeline to run; only polarAngleMeridiansConnectomics is implemented. Options: `polarAngleMeridiansConnectomics`. |
| `include_periph` | boolean | `true` | Add a peripheral eccentricity bin from 8 to 90 degrees. |
| `endpoints_assignment` | enum | `single_endpoints` | Assign streamlines by a single endpoint (this App) or by both endpoints. Options: `both_endpoints`, `single_endpoints`. |

</details>

<!-- brainlife-app:end -->

## potential analyses
1. polarAngleMeridiansConnectomics: this analysis pipeline will use structural connectivity within binned polar angle x eccentricity maps