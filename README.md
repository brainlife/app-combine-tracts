<h1 align="center">Combine classified white matter tracts</h1>

<p align="center">
  New combined tracts, a merged tractogram and an updated classification from selected tracts of a white matter classification.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.711"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.711-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Merges selected tracts of a white matter classification into new combined tracts with MRtrix3 and outputs the updated tractogram and classification.**

Extracts each classified tract from the tractogram with connectome2tck, concatenates the tracts listed on each line of combineIndices with tckedit, and builds a new classification (named by mergeNames) and merged tractogram. Use it to combine, for example, left and right or sub-segments of a tract into one bundle.

| | |
|---|---|
| 📥 **Inputs** | `neuro/wmc` · `neuro/track/tck` |
| 📤 **Outputs** | `neuro/track/tck` · `neuro/wmc` · `raw` |
| 🐳 **Container** | `brainlife/pyafq:1.0` |
| ⚙️ **Resources** | 1 CPUs · 20 GB memory · 0.5 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.711](https://doi.org/10.25663/brainlife.app.711) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `wmc` | `neuro/wmc` | yes | White matter classification (classification.mat) of the tractogram. |
| `track` | `neuro/track/tck` | yes | Tractogram (.tck) indexed by the classification. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `track` | `neuro/track/tck` | Tractogram containing the merged tracts. |
| `wmc` | `neuro/wmc` | New classification with one tract per line of combineIndices. |
| `raw` | `raw` | Per-tract intermediate .tck files and assignment lists. |

<details>
<summary><b>⚙️ Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `combineIndices` | string |  | These are the index numbers found within the classification.mat file for the tracts you want to combine. For example, if you want to combine the first two tracts from a classification structure, this field would be set as '1 2'. You can also have multiple sets of tracts combined into individual tracts by separating values by a new line |
| `mergeNames` | string |  | Names of the newly merged tracts you would like to set. Need to have the same number of names as the number of sets of tracts in combineIndices. |

</details>

<!-- brainlife-app:end -->

## 🚀 Running the App

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.711](https://doi.org/10.25663/brainlife.app.711), choose a white matter classification (`neuro/wmc`) and the tractogram it indexes (`neuro/track/tck`), set `combineIndices` and `mergeNames`, and press **Execute**. The App writes a merged tractogram, a new classification and the intermediate files to your project, all tagged `merged`.

Use it to combine, for example, the left and right parts of a tract, or several sub-segments of a tract, into one bundle.

- **`combineIndices`**: the index numbers of the tracts in `classification.mat` that you want to combine, separated by spaces. For example, `1 2` combines the first two tracts. To create several combined tracts at once, put each set on its own line.
- **`mergeNames`**: the names of the new combined tracts, one per set in `combineIndices`, separated by spaces.

### 🔬 How it works

1. The tract names and streamline assignments are read from the classification (Python).
2. Each classified tract is extracted from the tractogram with MRtrix3 `connectome2tck`, and the tracts listed on each line of `combineIndices` are concatenated with `tckedit`.
3. A new classification structure, named by `mergeNames`, is built for the merged tractogram (compiled MATLAB).

### 💻 Locally

You need [Singularity/Apptainer](https://apptainer.org/) and [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository and check out the released version (`git checkout v1.0`).
2. Create `config.json` with the paths to your inputs and the tracts to combine:

   ```json
   {
       "classification": "/input/wmc/classification.mat",
       "track": "/input/track/track.tck",
       "combineIndices": "1 2\n3 4",
       "mergeNames": "tractA tractB"
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

The App uses the `brainlife/pyafq:1.0`, `brainlife/mrtrix3:3.0.0` and `brainlife/mcr:neurodebian1604-r2017a` containers.

## 📦 Outputs

```text
track/
├── track.tck          tractogram containing the merged tracts
└── track_info.txt     tckinfo summary
wmc/
├── classification.mat new classification, one tract per line of combineIndices
└── tracts/            per-tract JSON files for visualisation
raw/                   per-tract intermediate .tck files and assignment lists
```

## 🩺 Troubleshooting

- **`combination failed. see logs and derivatives`**: the new classification was not produced. Check that `combineIndices` refers to tract indices present in the classification, and that `mergeNames` has one name per line of `combineIndices`.

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Combine classified white matter tracts. brainlife.io App. [doi:10.25663/brainlife.app.711](https://doi.org/10.25663/brainlife.app.711)
- Tournier et al. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. *NeuroImage* 202:116137 (2019). [doi:10.1016/j.neuroimage.2019.116137](https://doi.org/10.1016/j.neuroimage.2019.116137)
- Kruper et al. Evaluating the Reliability of Human Brain White Matter Tractometry. *Aperture Neuro* 2021 (2021). [doi:10.52294/e6198273-b8e3-4b63-babb-6e6b0da10669](https://doi.org/10.52294/e6198273-b8e3-4b63-babb-6e6b0da10669)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## 👥 Authors

- Brad Caron
- Franco Pestilli (project director)

**Contributors:** Soichi Hayashi

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## ⚖️ License

[MIT](LICENSE). MRtrix3 is distributed under the [Mozilla Public License 2.0](https://www.mrtrix.org/).
