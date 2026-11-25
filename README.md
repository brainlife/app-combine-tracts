<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Merges selected tracts of a white matter classification into new combined tracts with MRtrix3 and outputs the updated tractogram and classification.**

Extracts each classified tract from the tractogram with connectome2tck, concatenates the tracts listed on each line of combineIndices with tckedit, and builds a new classification (named by mergeNames) and merged tractogram. Use it to combine, for example, left and right or sub-segments of a tract into one bundle.

Run it on brainlife.io: [https://doi.org/10.25663/brainlife.app.711](https://doi.org/10.25663/brainlife.app.711)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `wmc` | `neuro/wmc` | yes | White matter classification (classification.mat) of the tractogram. |
| `track` | `neuro/track/tck` | yes | Tractogram (.tck) indexed by the classification. |

| Output | Datatype | Description |
|---|---|---|
| `track` | `neuro/track/tck` | Tractogram containing the merged tracts. |
| `wmc` | `neuro/wmc` | New classification with one tract per line of combineIndices. |
| `raw` | `raw` | Per-tract intermediate .tck files and assignment lists. |

| Parameter | Type | Default | Description |
|---|---|---|---|
| `combineIndices` | string |  | These are the index numbers found within the classification.mat file for the tracts you want to combine. For example, if you want to combine the first two tracts from a classification structure, this field would be set as '1 2'. You can also have multiple sets of tracts combined into individual tracts by separating values by a new line |
| `mergeNames` | string |  | Names of the newly merged tracts you would like to set. Need to have the same number of names as the number of sets of tracts in combineIndices. |

Runtime: container `brainlife/pyafq:1.0`; 1 CPUs; 20 GB memory; 0.5 h walltime.

<!-- brainlife-app:end -->

# app-combine-tracts