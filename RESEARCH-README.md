> **Repository availability:** This is the original v0.1 research-pack README. Paths and commands below refer to the extracted `RAVEN-P1-v0.1` pack. The scripts, dependencies, inputs, and model outputs are still pending upload to this repository, so these commands cannot yet run from this repository alone. The candidate FASTA is available at `results/RAVEN-P1-v0.1.fasta`.

# RAVEN-P1 v0.1

A 92-residue computational sequence redesign for the published Top7 structural scaffold.

**Status: computational candidate and forward prediction only. No new laboratory synthesis or testing. No binding, catalytic, therapeutic, solubility, stability, or safety property is established.**

The candidate sequence is in `results/RAVEN-P1-v0.1.fasta`. This is a sequence generated under an open user brief, not a claim of worldwide sequence novelty or a new protein fold.

## Design target and provenance

- Original experimental reference: PDB 1QYS, Kuhlman et al., Science (2003), DOI 10.1126/science.1089427.
- Use only modeled chain A residues 3-94, a consecutive 92-residue crop. Do not equate it with the original paper's 93-residue protein or the deposited 106-residue construct.
- MSE at original A37 is normalized to MET at design position 35 while retaining its N/CA/C/O atoms. No internal residue is deleted.
- `inputs/top7-backbone-92.pdb` is the experimental DESIGN TEMPLATE, not a predicted RAVEN-P1 structure. It retains reference residue identities.
- `inputs/residue-map.json` preserves the exact original-to-design numbering.

## Executed computation

ProteinMPNN official commit `8907e6671bfbfc92303b5f79c4b5e6ce47cdef57`, vanilla v_48_020. Generate 32 sequences at each sampling temperature 0.1 and 0.2, seed 20261104, batch size 4, inference backbone noise 0. C and X excluded. All 64 unique candidates retained.

Before rescoring, the selection rule was specified as minimum mean NLL across common decoding-order seeds 1001-1008. All 64 designs and the normalized reference were scored. Sixteen composition-preserving reference shuffles and sixteen selected-candidate shuffles serve as computational controls. They are not experimentally verified nonfolders. `results/scores.csv` contains all 97 records and all eight per-order scores. The selected candidate's scores reproduced exactly on this runtime.

NLL is model-conditional sequence compatibility in nats per residue, not physical folding energy, stability, solubility, function, or probability of success. Standard deviation across decoding orders is not biological uncertainty. Generating and ranking with the same model introduces selection bias. Top7 may overlap model training data; this is not a held-out generalization benchmark.

OmegaFold official commit `313c873ad190b64506a497c926649e15fcd88fcd`, model 1, CPU, three cycles (official default is ten), subbatch 16, fifteen masked pseudo-sequences, mask rate 0.12. Its deterministic mask seed is sequence length 92, Torch seed 6104. Input is sequence only; no structural template is supplied. The output is the highest-confidence result among three cycles, not necessarily cycle three. PDB B-factors contain model confidence times 100. OmegaFold's output numbering is 0-91; compare by sequence order.

The public ESMFold API was attempted once on the reference and returned HTTP 504. No ESMFold result is reported. `results/esmfold-reference-status.json` retains the failure.

## Reproduction

Use Python 3.12 or the Python version recorded in `results/software-versions.json`. Install the exact versions in `requirements.txt`. No GPU or paid service was used.

```
python -m pip install -r requirements.txt
python reproduce.py
```

The command writes under `inputs/` and `results/`, recreating the generation and score analysis. Existing saved forward predictions are used only for figure/check regeneration unless `--fold` is specified. Small numerical differences may occur on other hardware or software versions. Model hashes and source identities are recorded.

For actual new forward predictions, download the official OmegaFold checkpoint from:

https://helixon.s3.amazonaws.com/release1.pt

Save as `vendor/omegafold-release1.pt`. Expected size: 3,181,611,124 bytes. Expected SHA-256:

`3a91f6a35dd6ba6f7643f99a407038a0dc05abab15ce5c92d62ce1359922b201`

The approximately 3.18 GB checkpoint is not included in this portable pack. After verifying it:

```
python reproduce.py --fold
python build_report.py
```

The custom runner deletes the extra checkpoint dictionary after assigning weights to reduce memory use; it does not change the neural-network architecture. The original model source is included with its license. ProteinMPNN weights and source are bundled under the original MIT license. OmegaFold source is bundled under Apache-2.0. Third-party authors retain their attribution. Data/source links and SHA-256 manifest are in the pack.

## Interpretation and next gate

The study supplies a sequence, model scores, controls, a forward-predicted structure, and explicitly limited structural agreement metrics. Physical folding and any useful function remain open. Simulation cannot establish those experimental outcomes. A future folding/stability measurement would test the present computational hypothesis. No biological manufacture, animal work, patient use, or dosing instructions are part of this package.

Anubis commissioned the work and set the computer-only scope. Asteris Solenne / ChatGPT designed and executed the computational study and prepared its artifacts. AI-assisted design review audited preprocessing, scoring, and claim boundaries; it is not experimental validation.
