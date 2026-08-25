# FlyRank Capstone — Structured Content Archetype Clustering

Groups pages into six performance archetypes (protect / improve / rewrite /
merge / prune / monitor) from safe search signals, and ships a ranked,
reason-coded action table. Deployed paper: `paper/index.html`.

## Layout

```
scripts/archetype_pipeline.py   pipeline: load -> features -> split -> cluster -> archetypes -> ranked table
scripts/baseline_compare.py     staleness/CTR rule baseline vs. the clustering model
scripts/make_charts.py          regenerates the 4 charts used in the paper
work/notebooks/                 capstone notebook (+ drop your weekly assignment notebooks here too)
data/                           run outputs (clustered_pages.csv, run_report.json, baseline_comparison.json)
paper/                          the deployed research paper (index.html + charts) — this is what GitHub Pages serves
submission/paper_url.txt        put your live paper URL here — this is the one file graders read
```

## Finish it with your real data (I don't have HF or GitHub access from my sandbox)

1. **Get real numbers.** Locally, with `HUGGING_FACE_HUB_TOKEN` exported:
   ```
   cd scripts
   pip install duckdb huggingface_hub pandas scikit-learn matplotlib
   python archetype_pipeline.py --source real --out ../data/clustered_pages.csv
   python make_charts.py   # edit make_charts.py's load call to load_data_real() first
   ```
   Check the column names in `load_data_real()` against the actual warehouse
   schema on the dataset card before running — I wrote it from the described
   workflow (DuckDB over `hf://`, notebook 03 pattern), not against the live
   table, since huggingface.co isn't reachable from where I built this.

2. **Swap the numbers into `paper/index.html`.** The abstract, stat row,
   Results section, and sample table all currently report the synthetic
   run's real output. Same structure, just re-run and paste in your numbers.

3. **Push to your own repo:**
   ```
   git init -b main
   git add .
   git commit -m "Capstone: structured content archetype clustering"
   git remote add origin https://github.com/Mycodelab-lab/<your-repo-name>.git
   git push -u origin main
   ```
   (This repo is already `git init`'d for you below — just add your remote and push.)

4. **Turn on GitHub Pages:** repo → Settings → Pages → Source: `main` branch,
   folder `/paper`. Your paper goes live at
   `https://mycodelab-lab.github.io/<your-repo-name>/`.

5. **Put that exact URL in `submission/paper_url.txt`**, commit, push again.
   That file is the only thing anyone reads to find your paper.

## Data safety

No client names, domains, URLs, private queries, credentials, or raw exports
anywhere in this repo. Every feature is a derived, aggregated number.
`trend_direction` / `trend_pct` are pulled only for an internal sanity check
against synthetic ground truth — never used as a model feature (see the
docstring at the top of `archetype_pipeline.py`).
