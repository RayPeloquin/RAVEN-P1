# AI-assisted design review

A separate AI review inspected the preprocessing, scoring scripts, saved tables, model runners, and report. This is advisory computational review, not experimental or clinical validation.

Corrections adopted:

- Explicit 92-residue crop instead of conflating paper, deposited-construct, and modeled lengths.
- Preserve HETATM MSE A37 as normalized MET backbone; do not silently delete it.
- Use common decoding-order seeds, retain all candidates and controls, and distinguish order variation from biological uncertainty.
- Label the experimental backbone as the design template, not a predicted new-sequence structure.
- Compare forward outputs by sequence order because OmegaFold writes residue IDs 0-91.
- State three-cycle settings and highest-confidence-among-cycles output selection.
- Label the 0.562 A pair-distance RMSE as nonlocal (excluding pairs within three sequence steps).

Independent arithmetic checks confirmed 97 scored rows, minimum selection of design_064, both shuffle means, sequence length and 50 substitutions. A second geometry implementation using Bio.SVDSuperimposer confirmed candidate/template C-alpha RMSD 0.780979564 A, agreeing with the main implementation within 3e-8 A. It also confirmed exact 92-residue input/output sequence agreement, candidate mean CA confidence 96.90293478/100, and contact-map F1 0.94430992736. Differences in float32 score averaging at about 2e-7 nats/residue are ordinary numerical rounding.

Open limitations remain visible: same-family generation/selection bias, possible training overlap with Top7, no worldwide sequence-novelty search, no new experimental data, and no assigned biological function. Original scaffold, model, and software authors retain attribution.
