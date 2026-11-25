<h1 align="center">visual white matter</h1>

<p align="center">
  Builds polar-angle-by-eccentricity visual cortex parcels from pRF maps and segments the tractogram streamlines that terminate in each parcel and visual area, using Connectome Workbench, FSL and MRtrix3.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.785"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.785-blue.svg" alt="Run on brainlife.io"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Builds polar-angle-by-eccentricity visual cortex parcels from pRF maps and segments the tractogram streamlines that terminate in each parcel and visual area, using Connectome Workbench, FSL and MRtrix3.**

Projects the pRF polar-angle and eccentricity maps from the FreeSurfer surface into DWI space, bins them into wedges around the chosen polar-angle meridians and eccentricity steps, and combines them into a volumetric parcellation and cortexmap. In this single-endpoints variant it then assigns each streamline to the visual area and retinotopic bin of its endpoint with MRtrix3 and writes the per-bin tracts as a white matter classification. Use it to study visual white matter organised by visual-field position; needs a FREESURFER_LICENSE and inputs that share the subject's DWI space.

| | |
|---|---|
| **Inputs** | `neuro/dwi` · `neuro/freesurfer` · `neuro/prf` · `neuro/track/tck` · `neuro/parcellation/volume` |
| **Outputs** | `neuro/parcellation/volume` · `neuro/cortexmap` · `neuro/track/tck` · `neuro/tcks` · `neuro/wmc` · `neuro/tractmeasures` · `raw` |
| **Resources** | 8 CPUs · 3 h walltime |
| **Requires** | `FREESURFER_LICENSE` |
| **Run it** | [doi.org/10.25663/brainlife.app.785](https://doi.org/10.25663/brainlife.app.785) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` | yes | Preprocessed DWI, used as the reference space for the parcellation. |
| `freesurfer` | `neuro/freesurfer` | yes | FreeSurfer subject directory used to map surface pRF data to the volume. |
| `prf` | `neuro/prf` | yes | pRF (population receptive field) maps on the surface, with polar angle and eccentricity. |
| `track` | `neuro/track/tck` | yes | Whole-brain .tck tractogram in DWI space. |
| `varea` | `neuro/parcellation/volume` | yes | Volumetric visual-area parcellation with its label.json. |

#### Outputs

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
<summary><b>Parameters</b> (12)</summary>

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

## Versions on brainlife.io

This repository hosts several Apps, each built from its own branch.

| App | Branch | Run it | Status |
|---|---|---|---|
| Structural network connectomics of visual white matter by polar angle and eccentricity - Single Endpoints | [`v1.1`](https://github.com/bacaron/app-visual-white-matter/tree/v1.1) | [doi:10.25663/brainlife.app.785](https://doi.org/10.25663/brainlife.app.785) | current |
| Structural network connectomics of visual white matter by polar angle and eccentricity - first version | [`v1.0`](https://github.com/bacaron/app-visual-white-matter/tree/v1.0) | [doi:10.25663/brainlife.app.780](https://doi.org/10.25663/brainlife.app.780) | deprecated |

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.785](https://doi.org/10.25663/brainlife.app.785), choose `neuro/dwi`, `neuro/freesurfer`, `neuro/prf`, `neuro/track/tck`, `neuro/parcellation/volume` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/parcellation/volume`, `neuro/cortexmap`, `neuro/track/tck`, `neuro/tcks`, `neuro/wmc`, `neuro/tractmeasures`, `raw`.

### Locally

You need [`jq`](https://jqlang.github.io/jq/), `FREESURFER_LICENSE`.

1. Clone this repository and check out the App's branch (`git checkout v1.1`).
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "dwi": "/path/to/dwi/dwi",
       "bvecs": "/path/to/dwi/bvecs",
       "bvals": "/path/to/dwi/bvals",
       "sbref": "/path/to/dwi/sbref",
       "sbref_json": "/path/to/dwi/sbref_json",
       "freesurfer": "/path/to/freesurfer/freesurfer",
       "prfVerticesDir": "/path/to/prf/prfVerticesDir",
       "prfSurfacesDir": "/path/to/prf/prfSurfacesDir",
       "track": "/path/to/track/track",
       "varea_parc": "/path/to/varea/varea_parc",
       "varea_label": "/path/to/varea/varea_label",
       "paMeridians": "0 45 90 135 180",
       "paAngle": "15",
       "eccAngle": "2",
       "inputparc": "aparc.a2009s",
       "assignment_radial_search": 4,
       "assignment_reverse_search": "",
       "assignment_forward_search": "",
       "inv_length": true,
       "richClubPercentage": 90,
       "analysis": "polarAngleMeridiansConnectomics",
       "include_periph": true,
       "endpoints_assignment": "single_endpoints"
   }
   ```

3. Run `./main`.

## potential analyses

1. polarAngleMeridiansConnectomics: this analysis pipeline will use structural connectivity within binned polar angle x eccentricity maps

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Structural network connectomics of visual white matter by polar angle and eccentricity - Single Endpoints. brainlife.io App. [doi:10.25663/brainlife.app.785](https://doi.org/10.25663/brainlife.app.785)
- Fischl. FreeSurfer. *NeuroImage* 62:774-781 (2012). [doi:10.1016/j.neuroimage.2012.01.021](https://doi.org/10.1016/j.neuroimage.2012.01.021)
- Jenkinson et al. FSL. *NeuroImage* 62:782-790 (2012). [doi:10.1016/j.neuroimage.2011.09.015](https://doi.org/10.1016/j.neuroimage.2011.09.015)
- Tournier et al. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. *NeuroImage* 202:116137 (2019). [doi:10.1016/j.neuroimage.2019.116137](https://doi.org/10.1016/j.neuroimage.2019.116137)
- Garyfallidis et al. Dipy, a library for the analysis of diffusion MRI data. *Frontiers in Neuroinformatics* 8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Marcus et al. Informatics and Data Mining Tools and Strategies for the Human Connectome Project. *Frontiers in Neuroinformatics* 5 (2011). [doi:10.3389/fninf.2011.00004](https://doi.org/10.3389/fninf.2011.00004)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

Maintained by [@bacaron](https://github.com/bacaron).
