# CACS 2026 Paper Draft

**Working title**: Keep3R: Memory-Bounded Streaming Visual Geometry for Long-Horizon Robotic Manipulation

## Build

```bash
cd latex && latexmk -pdf main.tex
```

Requires TeX Live with `newtxtext/newtxmath` (Times), TikZ, and pgfplots. Output: `main.pdf`; the content including references currently ends a few lines into page 7 (about 6.05 pages). Style notes applied on 2026-09-04: no em-dash connectives, few colons, "pseudo-ground-truth" wording removed (a single "model-generated labels" caveat remains in IV-A).

## Layout

- `main.tex` — preamble, title, abstract wrapper, author placeholders, references (manual IEEE-style `thebibliography`, ordered by first citation).
- `cacs2026.sty` — official template style (do not modify).
- `sections/` — abstract, intro, related, method, experiments, conclusion.
- Method name is the macro `\method` in `main.tex` — change once to rename globally.

## Figures

- **Fig. 1** (pipeline, TikZ) and **Fig. 2** (VRAM/FPS vs. T, pgfplots) live as single-source bodies in `figures/fig1_body.tex` and `figures/fig2_body.tex`, `\input` both by the paper and by the standalone export wrappers `figures/fig1_pipeline.tex` / `figures/fig2_efficiency.tex`. Edit the body file and both stay in sync.
- Standalone exports (`.pdf` + 300 dpi `.png`) are committed in `figures/`; rebuild with `latexmk -pdf` inside `figures/` and `pdftoppm -png -r 300 -singlefile <name>.pdf <name>`.
- Series colors are a colorblind-safe palette (blue/orange/aqua/violet) with distinct marker shapes.
- The former Fig. 3 (qualitative point clouds) was removed per editorial decision (2026-09-04), together with the Qualitative Results subsection.

## Placeholders / TODO before submission

Search for `TODO(` in the sources:

1. **`TODO(authors)`** (`main.tex`) — real author list, affiliations, funding footnote.
2. **`TODO(rerun)`** (`sections/experiments.tex`) — all "Ours" numbers are thesis-run placeholders. The paper's method drops the thesis's HSAP workspace anchors + DFS (ablations showed no measurable effect), so re-run "Ours" rows with the final configuration (expected within noise). Also: confirm episode list, `a_init` value, and the exact config difference between the "diversity-only ranking" retention strategy (Table 4) and the β=0 sweep point (Table 6) — they are different runs in the thesis with different numbers.

## Venue facts (verified 2026-08-04 from cacs2026.fcu.edu.tw)

- **Submission deadline: 2026-08-15** (final manuscript 2026-09-25). English, PDF only, via the Academia Sinica system (link on the paper-submission page).
- Regular track: 6–8 pages; **6 pages is the free target**, NT$3,000 per page beyond 6, hard cap 8. Accepted regular papers are submitted to IEEE Xplore (EI-indexed); camera-ready must pass IEEE PDF eXpress (Conference ID 68747X).
- Review is most likely **not double-blind** (no anonymization requirement stated anywhere) — but unconfirmed; email cacs2026@fcu.edu.tw to be sure before deciding whether to keep real names on the submission.

## Name & citation verification (web-checked 2026-08-04)

- **"Keep3R" is collision-free** in CV/3D/robotics (only unrelated crypto project "Keep3r Network"). Backup name "Curat3R" also clean; "Anch3R" is burned (Anchor3R, arXiv:2606.05035, same subfield).
- StreamVGGT and TTT3R upgraded to **ICLR 2026** in the bibliography; StreamVGGT's v2 title dropped "4D" (verify camera-ready title on OpenReview). CUT3R = CVPR 2025 oral, real title "Continuous 3D Perception Model with Persistent State" (already correct here).
- **GHOST** (arXiv:2605.15852, May 2026): training-free geometry-driven token eviction for streaming 3D reconstruction — nearest competitor, now cited and differentiated in Related Work; someone should read it fully before finalizing the method framing.
- PointWorld's abstract does not mention DROID — confirm the "PointWorld-DROID" label-set name against the paper body (see `TODO(verify)` in experiments.tex).

## Key editorial decisions vs. the thesis

- **Cut**: HSAP adaptive workspace anchors + dynamic foreground suppression (no measurable ablation gain); pose/ATE/RPE evaluation (commented out in the thesis; unfavorable). Poses remain a model output but are not an evaluated claim.
- **Kept**: all five baselines (CUT3R, TTT3R, StreamVGGT, Evict3R, InfiniteVGGT) — we are on par in quality and ~2× faster, so the direct memory-bounded baselines strengthen the paper.
- **Reframed**: first-frame retention kept as "reference-frame anchoring" (one paragraph inside BCC, attention-sink analogy).
- **New emphasis**: the budget sweep shows quality is flat over a 16× budget range while cost scales — the token budget is presented as a pure deployment cost knob (the "cost-controllable" story).
- Three thesis reconstruction tables merged into one (T=500) with T=100/300 trends in text.
