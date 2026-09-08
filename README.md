# Federated Digital Twins for Poisoning-Resilient Intrusion Detection at the Industrial IoT Edge

Replication package for the paper of the same name. This repository contains
the experiment notebook, the real experimental output it produced, the
analysis script that turns that output into every table and figure in the
paper, and the paper's own LaTeX source -- so that every number in the paper
traces back to a file in this repository, and you can check that for yourself
rather than take it on faith.

## What's actually in here

```
notebooks/   The Colab notebook that runs every experiment in the paper
             against real, Kaggle-hosted CICIoT2023, Edge-IIoTset, and
             N-BaIoT traffic, and writes structured results to disk.
results/     The actual output of that notebook -- CSVs and JSON files,
             one directory per dataset, plus a run log. This is real
             measured data, not sample/placeholder output.
analysis/    generate_tables.py reads results/ and reprints every table
             in the paper. Run it yourself and diff the output against
             paper/main.tex.
paper/       The paper's LaTeX source, bibliography, figures, and a
             compiled PDF.
docs/        Step-by-step reproduction instructions, at two levels:
             re-checking our arithmetic (fast, no Kaggle needed) and
             re-running the experiments from scratch (slow, needs Kaggle).
```

## Quick start

```bash
git clone <this-repo>
cd <this-repo>
pip install -r requirements.txt
cd analysis && python generate_tables.py
```

That prints every table in the paper, computed live from the included
results data. See [`docs/REPRODUCING.md`](docs/REPRODUCING.md) for the full
guide, including how to re-run the experiments from scratch against fresh
Kaggle downloads.

## What this paper actually found

The short version, elaborated in the paper itself: a *digital-twin
validation gate* lets each federated client test a candidate model update
against its own held-out traffic before that update can influence the
shared model. We validated it against real traffic from three different
IoT/IIoT intrusion-detection datasets rather than one, at five seeds per
experiment, and reported the results that complicated the headline finding
alongside the ones that supported it -- including places where the gate
underperforms established baselines, an earlier claim that didn't survive
a larger seed count, and a negative result for our own proposed improvement
to the mechanism. The abstract in `paper/main.tex` has the full summary.

## Data provenance

Every file in `results/` is the unmodified output of a real notebook run
against real downloaded traffic -- nothing in this directory was generated,
estimated, or hand-edited to match the paper. Two genuine data-pipeline bugs
were found and fixed during this project (N-BaIoT's device identity
collapsing to a single value; a label-leakage column in Edge-IIoTset's
feature set); both are documented in the paper and in the notebook's own
comments at the relevant cells, not hidden.

`results/run_log.txt` records exactly which of the notebook's experiment
sections succeeded and which didn't for this specific run, with full
tracebacks for any failures -- included as-is, not cleaned up.

## Citing this work

See [`CITATION.cff`](CITATION.cff) for both the software citation and the
paper citation (GitHub will render a "Cite this repository" button from
this file automatically). Note that author names and final publication
details (volume, pages, DOI) are placeholders in that file until the paper
is accepted -- update them before relying on the file for a real citation.

## License

MIT -- see [`LICENSE`](LICENSE). The three underlying datasets (CICIoT2023,
Edge-IIoTset, N-BaIoT) are not redistributed here and remain under their
own original licenses on Kaggle; see `paper/main.tex`'s bibliography for
their citations.
