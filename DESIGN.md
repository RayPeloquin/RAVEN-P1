# RAVEN-P1 computational design scope

User request: create a theoretical synthetic protein without new laboratory testing; AI-assisted review is authorized. Design choices below are the assistant's.

The output is a new amino-acid sequence proposed for the experimentally observed Top7 backbone. No binding, therapeutic, catalytic, solubility, or safety function is claimed. The 92-residue target is chain A positions 3-94 of PDB 1QYS; MSE37 is normalized to methionine while retaining N/CA/C/O. The original 93-residue paper construct and 106-residue deposited construct are not conflated with this crop.

Approaches considered: manual mutation (weak sequence-structure evidence); fixed-backbone ProteinMPNN redesign (chosen, reproducible on CPU); unconstrained new-fold generation (larger scope, no novelty requirement). Forward folding is a separate follow-up check, attempted only on a small reference/candidate set using free available computation. Failed services must remain visible.

Generation: official ProteinMPNN commit 8907e6671bfbfc92303b5f79c4b5e6ce47cdef57, vanilla v_48_020, 32 sequences at each temperature 0.1/0.2, seed 20261104; cysteine excluded to avoid introducing a disulfide requirement, X excluded as unknown. No backbone noise at inference. All 64 outputs retained.

Selection is declared before rescoring: choose the sequence with the lowest mean ProteinMPNN negative log likelihood across eight common decoding-order seeds 1001-1008. Report standard deviation across these orders descriptively, not as a confidence interval. Scores are internal model compatibility, not folding energy or probability of physical success. Compare the template's normalized sequence and 16 composition-preserving shuffles of that sequence; also compare 16 shuffles of the selected design after selection. Shuffles are not experimentally verified nonfolders.

Deliverables: candidate FASTA; original and normalized reference inputs plus explicit numbering map; generation outputs; all scores and controls; exact scripts, model provenance/checksums and limitations; a compact PDF and research ZIP. Include a candidate 3D file only if actual forward prediction succeeds. An unchanged target backbone remains labeled template. Top7 may be in model training data; no generalization or worldwide novelty claim.

Checks: preserve 92 consecutive residues and all 368 backbone atoms; verify MSE position and sequence registration; validate sequence lengths/alphabet and model masks; use common decoding orders for fair comparisons; reproduce selected score; verify structural sequence and confidence scale if forward prediction succeeds; inspect every PDF page and verify ZIP content.
