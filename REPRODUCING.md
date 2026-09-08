# Reproducing this paper's results

There are two levels of reproduction here, and they need different things from you.

## Level 1: Verify the numbers in the paper against the data we measured

This does **not** require Kaggle, a GPU, or any long-running compute. The
`results/` directory already contains the actual output of our experiment
runs -- the same CSVs and JSON files that every table and figure in
`paper/main.tex` was built from.

```bash
pip install -r requirements.txt
cd analysis
python generate_tables.py
```

This prints every table in the paper, recomputed live from `results/`.
Compare the output to the tables in `paper/main.tex` (or `paper/paper.pdf`)
line by line. If something doesn't match, that is a bug worth reporting --
open an issue.

This is the right level if you want to check our arithmetic, re-plot the
data your own way, or build on our results without re-running the experiments.

## Level 2: Re-run the experiments from scratch against real data

This is the right level if you want to verify the *experiments themselves*,
not just our arithmetic on their output -- for example, running more seeds,
a different dataset, or checking whether our findings hold on a fresh
Kaggle download.

**Requirements:**
- Google Colab (or a local Jupyter environment with a GPU; the notebook was
  built and tested on Colab specifically)
- A Kaggle account and API token (`kaggle.json`) -- get one from
  kaggle.com → your profile icon → Settings → API → Create New Token
- Time: the full run across all three datasets at 5 seeds took on the order
  of several hours in our runs. `QUICK_MODE` and `MAX_ROWS_QUICK` in the
  notebook's config cell control this tradeoff; read the comments there
  before changing them.

**Steps:**

1. Open `notebooks/federated_digital_twins_AUTO_all_datasets.ipynb` in Colab.
2. Run the cells top to bottom. The kaggle.json upload is an interactive
   widget -- select your token file when prompted.
3. Watch for the sanity-check cell (Section 16b) before the long run starts.
   It should print "All checks passed." If it doesn't, fix whatever it
   flags before continuing -- this exists specifically so you don't
   discover a configuration problem three hours into the run.
4. The final cell zips everything into `results_bundle.zip` and downloads
   it automatically.
5. Unzip it, and either replace this repo's `results/` directory with the
   new output or point `analysis/generate_tables.py` at it via the
   `RESULTS_DIR` variable at the top of the script.

**What can differ from our numbers, and why:** small seed-to-seed and
run-to-run floating-point variation is expected and does not indicate a
problem -- we observed this ourselves between runs (see the paper's
discussion of the "centralized" condition in Section VI-A, and Section
VI-B's note on why the pipeline caches each seed's data sample once rather
than resampling it). Large, qualitative differences (a table showing an
opposite sign, or a catch rate off by an order of magnitude) are not
expected and are worth investigating.

## A note on N-BaIoT and Edge-IIoTset specifically

Both loaders required real correctness fixes during this project -- not
just tuning -- documented in `paper/main.tex` (Section V, "Real-data
loading") and in the notebook's own comments at the relevant loader cells.
If you fork this notebook to point at a differently-structured download of
either dataset, re-read those comments first; the loaders make specific
assumptions about file naming (N-BaIoT) and column presence (Edge-IIoTset)
that a differently-packaged copy of either dataset might not satisfy.
