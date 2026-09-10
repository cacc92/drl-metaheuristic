# Dataset, results, models and source code (version 2, revised manuscript)

Companion data package, versioned in the public repository https://github.com/cacc92/drl-metaheuristic, for the article **"Towards Autonomous Bio-Inspired Optimization: Deep Reinforcement
Learning for Adaptive Metaheuristic Orchestration"** (MDPI *Biomimetics*, manuscript
biomimetics-4483602).

The article studies deep reinforcement learning agents (PPO) that orchestrate a portfolio of seven
bio-inspired, population-based metaheuristics over a single shared population. This package
contains everything behind the tables and figures of the revised manuscript and the software that
produced them: five-fold cross-validation on the thirty cb9 instances of the multidimensional knapsack
problem (fifteen strategies, 600 runs per strategy), the same protocol on 65 Set Covering instances
(eleven strategies, 1,300 runs per strategy) and on the cb7 instances, the ablation of the portfolio, the
studies at half the evaluation budget and over the budget-quantum grid, the robustness re-evaluation with
a second set of seeds, the training logs and trained models of every fold, the scripts that produce
every table and figure, and the complete source code of the research framework.

## What changed with respect to the first version of the deposit

The first version (July 2026) reported a single split of cb9 (21 training / 6 validation / 3 test instances,
ten seeds) with the reference values of Lai et al. (2018) only. This version replaces that study with the
cross-validated protocol of the revised article and adds: the registry of twenty evaluation seeds drawn
once with `SystemRandom`; the three control strategies (untrained agent, repair-only baseline and
5-MH-portfolio agent); the Set Covering domain; the reference values improved by Xu, Li and Yin (2024)
for three cb9 instances; the study at B = 12,500 and the B x Q grid of Section 5.5; the re-evaluation
with seeds 1000-1019; the evaluations of the intermediate and extended training checkpoints; the cb7
campaign under the same protocol (Section 5.6); and the complete source code of the framework.

## Contents

| File | Files | Size | Content |
|---|---|---|---|
| `01_instances_mkp_cb9_and_cb7.zip` | 67 | 2.0 MB | 30 cb9 and 30 cb7 instances, reference tables (Appendix B for cb9), official OR-Library files |
| `02_instances_scp.zip` | 133 | 32.5 MB | 65 SCP instances (converted and original), reference table with sources |
| `03_protocol_folds_and_seeds.zip` | 12 | 0.0 MB | fold compositions and seed registries of every campaign (copies of the files in zips 04-06), protocol notes |
| `04_results_cb9_cross_validation.zip` | 658 | 2.4 MB | per-run results of the 14 strategies of the main cb9 campaign (headline and checkpoint evaluations), shared baselines, aggregates, logs |
| `05_results_cb9_additional_studies.zip` | 1747 | 4.3 MB | 5-MH ablation (the fifteenth cb9 strategy), B = 12,500 study, seeds 1000-1019 re-evaluations, B x Q grid, cb7 campaign |
| `06_results_scp_cross_validation.zip` | 342 | 1.1 MB | per-run results of the 11 strategies on the 65 SCP instances, shared baselines, logs |
| `07_training_logs_and_models.zip` | 720 | 207.1 MB | training logs and 140 trained models (Stable-Baselines3 2.8), one per fold and phase |
| `08_analysis_scripts_and_tables.zip` | 27 | 0.7 MB | table and figure scripts of the article, final tables (CSV/LaTeX), figures and the CC0 licence of the scripts |
| `09_framework_source.zip` | 307 | 0.9 MB | complete source code of the framework: packages, campaign scripts and notebooks (outputs cleared), tests, requirements, CC0 licence |

Total: 250.9 MB in nine zip files (decimal megabytes). `MANIFEST.sha256`
lists the SHA-256 of each zip.

Inside the zips, `data/...` and `output/...` mirror the layout of the research framework; the source
code of zip 09 is the framework itself (its `README.md` documents the packages, in Spanish, the language
of the code base); the scripts, tables and figures of zip 08 are gathered under `paper/` (`_make_cv_figs.py`),
`paper/tables/` and `paper/figures/`. Unzipping all the zips into one folder gives a self-contained tree in which the
framework packages, the data and the outputs sit where the scripts expect them.

## Campaigns and how they map to the article

| Directory (inside `output/`) | Article | Content |
|---|---|---|
| `mkp/cv_cb9/cv5x3` | Sections 4-5, Tables 4-6, Figures 3-6 | Main cb9 campaign: `base` (selection agent; its `step_logs.csv` are the decision trace of Table 6), `coevolution`, `selgen` (selection + configuration), `lineas_base_compartidas` (the shared pool: random, round_robin, the seven single metaheuristics, `ppo_untrained`, `repair_only`), `aggregate` |
| `mkp/cv_cb9/ablacion5mh` | Section 5.3; row "PPO agent (5-MH portfolio)" of Table 4 and of Figures 5-6 | Same protocol with PSO and Dolphin removed from the portfolio; its `lineas_base_compartidas` also hold the blind selectors and the untrained agent restricted to the five-method portfolio (not reported in the article) |
| `mkp/cv_cb9/registro_B12500` | Section 5.5, Table 9 | The thirteen strategies re-evaluated at B = 12,500 with the registry seeds |
| `mkp/cv_cb9/semillas_paper_B25000`, `semillas_paper_B12500` | Section 4.2 (robustness sentence) | Re-evaluation with the independent seeds 1000-1019 |
| `scp/cv_scp/cv5x3` | Section 5.4, Tables 7-8, Figure 4(b) | Set Covering campaign (`base` and the shared pool) |
| `mkp/bq_sensitivity` | Section 5.5 (second measurement: sensitivity to the quantum; no table) | Grid B x Q on the 12 instances of folds 0 and 2, seeds 1000-1002, six strategies (ppo, random, round_robin, single_ga, single_island_ga, single_mga), 1944 runs |
| `mkp/cv_cb7/cv5x3` | Section 5.6, Table 10 | Same protocol (B = 25,000, Q = 25) on the cb7 set (n = 100) with the ten-strategy pool of the SCP campaign (no repair_only); its instances are in `01_instances_mkp_cb9_and_cb7.zip` |

Strategy names in the files and in the article: `ppo` = PPO agent (selection); `coevolution_ppo` =
PPO agent (co-evolution); `selgen_ppo` = PPO agent (selection + configuration); `ppo` inside
`ablacion5mh` = PPO agent (5-MH portfolio); `ppo_untrained` = untrained PPO agent; `repair_only` =
repair-only baseline; `random` = uniform random selection; `round_robin` = round-robin selection;
`single_<mh>` = the metaheuristic alone (`ga`, `island_ga`, `mga`, `eda`, `shade`, `pso`, `dolphin`).

## Layout of a campaign

```
output/<domain>/<campaign>/
  folds.json                     fold composition, split seed, training seeds (campaigns that trained agents)
  semillas_evaluacion.json       registry of the twenty evaluation seeds
  <formulation>/fold<k>/
    phase<j>_seed<s>/training/   one chained training phase (5,376 agent decisions each)
      ppo_*.zip                  trained model at the end of the phase (Stable-Baselines3 2.8)
      step_logs.csv              one row per logged agent decision (about 5,300 per phase: the decisions of
                                 episodes still open when the phase ends are not written), with the state
                                 features, the action, the reward and the best value
      episode_rewards.csv        one row per training episode
      rollout_metrics.csv        PPO update diagnostics, one row per update (12 per phase, 448 timesteps each)
      training_summary.json      configuration, seeds and timing of the phase
      kappa_usage.csv            (selgen only) configuration vectors used during training
    evaluation/                  evaluation of the model of the last phase (phase 3, 21,504 decisions)
      comparison.csv             one row per run: method, instance, seed, best_fitness, gap_percent,
                                 total_reward, steps; coevolution adds env_type and mean_cardinality (mean
                                 number of active metaheuristics), selgen adds env_type and actions_taken
                                 (the action index of every decision, separated by ";"). For the SCP,
                                 best_fitness is the negated cost, because the environment maximizes.
      summary.json               per-method and per-instance summary of the fold
      (base and shared pool)  runs.json: the metaheuristic selected at every decision of every run;
              statistical_summary.csv/.json/.xlsx: per-instance descriptive statistics of the 20 runs
              (best, mean, SD, worst, median, and the same for the GAP; the fields reference_value and
              base_seed are unused). The paired Wilcoxon tests and A12 of the article are computed from
              comparison.csv by the scripts of zip 08, not read from these files.
      (coevolution)  mh_usage.csv: share of each metaheuristic in the simplex weights
      (selgen)  kappa_usage.csv (aggregate use of each metaheuristic and its configuration),
              trajectories.csv and runs_partial.jsonl (the configuration vector kappa_e, kappa_x, kappa_p
              chosen at every decision); wilcoxon.json is empty (no baseline was evaluated in that directory)
    evaluation_phase<j>/         (cb9 `base` only) the same evaluation for the model of phase j
  lineas_base_compartidas/fold<k>/<strategy>/   the non-agent strategies, one run per (instance, seed),
                                 with comparison.csv, runs.json, statistical_summary.* and summary.json
                                 (repair_only reports steps = 25,000, one per evaluation)
  aggregate/                     (cb9 cv5x3 only) cv_all_runs.csv: the runs of the three agents plus a copy
                                 of the ten-strategy pool per formulation (repair_only not included);
                                 cv_ranking.csv: mean GAP and SD per formulation; cv_learning_curve.csv:
                                 phases 0-3 of the selection agent (phases 4-7 are in
                                 paper/tables/tabla5_curva_entrenamiento.csv of zip 08); cv_agent_breakdown.csv
  logs/                          console logs (see the glossary below)
  resumen_entrenamiento_cb9.xlsx (cb9 cv5x3 only) summary of the training phases
```

The B x Q grid (`mkp/bq_sensitivity/cb9`) is organized as `fold<k>/B<budget>_Q<quantum>/` cells, each
with comparison.csv, runs.json, summary.json and wilcoxon_tests.json, plus `aggregate/` with
bq_all_runs.csv (every run), bq_agent_rank.csv and bq_gap_by_regime.csv.

Phases 0-3 are the four chained training phases of the article (5,376 decisions each: twelve PPO updates
of 32 steps x 14 parallel environments = 448 transitions; 21,504 decisions in total; the model reported
is the one at the end of phase 3). In the cb9 `base` campaign, phases 4-7 extend the training to 43,008
decisions and `evaluation_phase<j>` evaluates each checkpoint with the same twenty seeds; these
evaluations support the training-budget analysis and are not the headline result.

Glossary of the console logs. cb9 cv5x3: `fold<k>.log` = training of the selection + configuration agent
(phases 0-3); `train_ext_base_fold<k>.log`, `train_ext2_base_fold<k>.log` = phases 4-5 and 6-7 of the
selection agent; `eval_fold<k>.log` = evaluation of the three formulations; `evaluate_ext_base_fold<k>.log`,
`evaluate_ext2_base_fold<k>.log` = evaluation of the checkpoints; `repaironly_fold<k>.log` = repair-only
baseline; `fold3_fase0_error1455.log` = console of phase 0 of the selection agent on fold 3, which stopped
when phase 1 failed to start its worker processes (Windows error 1455, page file too small); phase 1 was
relaunched from the saved phase-0 model, so no transition was lost. The training consoles of phases 0-3
of the selection and co-evolution agents on cb9 were overwritten by the launcher and are not preserved;
their numeric training records are complete in zip 07. SCP, ablation and cb7: `fold<k>.log` or
`train_fold<k>.log` = training, `eval_fold<k>.log` or `evaluate_fold<k>.log` = evaluation; the SCP
evaluation logs list six phases and end with two "[bloqueado]" lines because the launcher was configured
for the exploratory phases 4-5, which were trained only for the cb9 selection agent; the SCP models have
four phases. Re-evaluations: `eval_B12500_fold<k>.log`, `eval_B25000_fold<k>.log`; grid:
`bq_sensitivity.log`. Local paths in the logs were replaced by `<FRAMEWORK_ROOT>`, `<PYTHON>` and `<HOME>`.

## Quality measure

GAP = (f_ref - f) / f_ref x 100 for the multidimensional knapsack problem (maximisation) and
GAP = (f - f_ref) / f_ref x 100 for the Set Covering problem (minimisation), where f_ref is the reference
value of the instance (`references_cb9.csv`, `references_cb7.csv`, `references_scp.csv`). Every evaluation
uses B = 25,000 objective evaluations (12,500 in the half-budget study), quantum Q = 25 iterations per
decision and a population of N = 100, except the B x Q grid, which varies B over {12,500; 25,000; 50,000}
and Q over {10, 25, 50}; all evaluations use the complete repair operator. Comparisons between strategies
are paired by (instance, seed) scenario and reported with the Wilcoxon signed-rank test and the
Vargha-Delaney A12 effect size.

## Source code (zip 09)

`09_framework_source.zip` is the research framework at the date of the revised submission: the packages
`core` (Problem, Optimizer, Individual, CountingProblemProxy), `problems` (MKP, SCP and others),
`metaheuristics` (the seven portfolio members and further algorithms), `hyperheuristics` (the environment,
the state, the reward and the three action-space formulations with their translation maps),
`ai_controller` (PPO training and evaluation per formulation, the repair-only baseline), `infrastructure`
and `analysis` (exporters and plots), `experiments` (the cross-validation engine `cv_campaign.py`, the
launchers `run_cv_campaign_cb9.py`, `run_cv_campaign_cb7.py`, `run_cv_campaign_scp.py`,
`run_cv_ablacion_5mh.py`, `run_repair_only_cv.py`, `run_eval_semillas_paper.py`, `run_bq_sensitivity.py`,
the data tools and the notebooks, with outputs cleared) and `tests` (257 tests), plus `requirements.txt`,
`LICENSE` and the three READMEs of the repository (`README.md`, `experiments/README.md`, `data/README.md`). Every campaign of the deposit can be re-run from it:
unzip it, install `requirements.txt` in a Python 3.12 environment, put `data/` and `output/` from the
other zips next to the packages, and follow `experiments/README.md`.

## Reproducing the tables and figures

Unzip the zips into one folder (the "root") and set the environment variable `FRAMEWORK_ROOT` to it;
the table scripts write to `TABLES_DIR` and `_make_cv_figs.py` to `FIGURES_DIR/en` and `FIGURES_DIR/es`
(English and Spanish versions of the figures). Without the variables, `consolidar_tablas.py`, `tablas_latex.py`
and `_make_cv_figs.py` fall back to the authors' folders, and `tablas_reevaluaciones.py`, `tabla_cb7_vs_cb9.py`
and `trace_table_cb9.py` write next to themselves. The zips each script needs are listed in brackets.

- `paper/tables/consolidar_tablas.py` [04, 05, 06] writes `tabla1_cb9_ranking.csv` (Table 4),
  `tabla2_cb9_por_fold.csv` (Table 5), `tabla3_scp_ranking.csv` (Table 7), `tabla4_scp_por_fold.csv`
  (Table 8) and `tabla5_curva_entrenamiento.csv` (training-budget checkpoints). It stops with a message
  if the 5-MH ablation of zip 05 is missing.
- `paper/tables/tablas_reevaluaciones.py` [04, 05] writes `tabla6_semillas1000_1019_B25000.csv` and
  `tabla6_semillas1000_1019_B12500.csv` (robustness re-evaluation, Section 4.2) and
  `tabla7_presupuesto_registro_B12500_vs_B25000.csv` (Table 9).
- `paper/tables/trace_table_cb9.py --root <root>` [04, 07] writes `trace_table_cb9.csv` (Table 6).
- `paper/tables/tabla_cb7_vs_cb9.py` [04, 05, 07] writes `tabla8_cb7_vs_cb9.csv` (Table 10: the eleven common
  strategies on cb7 and cb9) and `tabla8_entropia_final.csv` (the relative final entropy of the policy per fold
  quoted in Section 5.6).
- `paper/tables/tablas_latex.py` renders the five CSV files of `consolidar_tablas.py` as LaTeX
  (`tablas_finales.tex`, with the long captions of the first draft); Tables 6, 9 and 10 of the article
  were typeset from `trace_table_cb9.csv`, `tabla7_*.csv` and `tabla8_cb7_vs_cb9.csv`. Figures 1 and 2
  (conceptual overview and flowchart) are drawings and have no generating script.
- `paper/_make_cv_figs.py` [04, 05, 06, 07] generates Figures 3-6 from `rollout_metrics.csv`,
  `episode_rewards.csv`, `runs.json` and `comparison.csv`; the generated English figures are in
  `paper/figures/`.
- `experiments/tools/resumen_entrenamiento_cb9.py` [07, 09] builds `resumen_entrenamiento_cb9.xlsx`.
- The shipped CSV files in `paper/tables/` are the ones the article was typeset from; running the scripts
  on the deposited data reproduces them.

Environment used: Python 3.12, Stable-Baselines3 2.8.0, PyTorch 2.11, NumPy 2.4, pandas 3.0, SciPy 1.17.

## Licence and contact

The data, the results and the documentation are released under the Creative Commons Attribution 4.0
International licence (CC BY 4.0). The source code of the framework (zip 09) and
the analysis scripts (zip 08) are dedicated to the public domain under CC0 1.0 Universal (the `LICENSE`
file inside zip 09 and `paper/LICENSE` inside zip 08). The OR-Library files are redistributed verbatim for verification purposes and remain
attributed to their author (J. E. Beasley).

César Carrasco Carré — cesar.carrasco.c@mail.pucv.cl
Pontificia Universidad Católica de Valparaíso, Chile.
