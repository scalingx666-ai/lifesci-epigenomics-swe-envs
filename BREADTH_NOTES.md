# Build notes - Epigenomics & Gene Regulation

Repo2RLEnv pr_diff (v0.9.6), Harbor-format output. Domain: epigenomics.

## Funnel

| Stage | Count |
|---|---|
| Domain candidates (deduped) | 258 |
| After active+star+noise filter | 128 |
| Final target list (cap 200) | 128 |
| Repos with >=1 task | 93 |
| Repos with 0 tasks | 35 |
| Total tasks (<=3/repo) | 193 |

## Generation

- Shared task bank: each repo generated once (pr_diff, limit=3,
  max_files_per_pr=15, min_loc_changed=2), 4-way concurrency,
  rate-limit-aware with .done checkpoint resume.
- Per-domain consolidation: rank by reward_calibration.loc_changed desc,
  keep at most 3 tasks per repo. Zero-yield repos are NOT fabricated.
- Overlap with the 3 published cancer editions is allowed (user decision);
  domains are independent selections, not a partition of one repo set.

## Zero-yield repos (informational, 35)

- crazyhottommy/ChIP-seq-analysis
- crazyhottommy/scRNAseq-analysis-notes
- SiatBioInf/panCTC
- oncologylab/craftgrn
- stjude/learngenomics.dev
- agitter/single-cell-pseudotime
- Beliavsky/Fortran-code-on-GitHub
- anthropics/uplifting-biomolecular-modeling
- r3fang/SnapATAC
- genecell/single-cell-papers-with-code
- K-Dense-AI/scientific-agents
- nf-core/chipseq
- jsh58/Genrich
- jokergoo/EnrichedHeatmap
- PoisonAlien/trackplot
- nanoporetech/remora
- FunctionLab/ExPecto
- crazyhottommy/scATACseq-analysis-notes
- bioinfomaticsCSU/deepsignal
- epigen/atacseq_pipeline
- LiuLabUB/HMMRATAC
- reskejak/ATAC-seq
- MeHelmy/princess
- lucascamillomd/CpGPT
- tobiasrausch/ATACseq
- igordot/sns
- xy-chen16/EpiAgent
- RWilton/Arioc
- WGLab/DeepMod2
- treangenlab/methphaser
- PacificBiosciences/MethBat
- epigen/genome_tracks
- XiaoTaoWang/HiCPeaks
- BioinformaticsFMRP/TCGAWorkflow
- sajuukLyu/Protocols-4pub

## Caveats

- Static validation only; real grading needs an LLM judge key + Docker.
- inline_dockerfile mode skips image build (no Docker daemon needed to publish).
- Provider secrets in upstream diffs were redacted before public release.
