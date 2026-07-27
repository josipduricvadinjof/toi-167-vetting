Vetting Analysis of TOI-167.01: An Independent Photometric Assessment Using Public TESS Data

Author: Josip Đurić Vadinjof
Affiliation: Faculty of Electrical Engineering and Computing (FER), University of Zagreb Date: July 2026

ABSTRACT

We present an independent vetting analysis of TOI-167.01 (TIC 149990841), an unconfirmed TESS Object of Interest with a catalog period of 4.453 days. Using publicly available TESS light curve data spanning 25 sectors (2019–2025) and standard open-source tools (lightkurve), we recover a transit-like signal at a period of 4.453045 days, consistent with the catalog value to five significant figures. We apply two standard false-positive vetting checks — an odd-even transit depth comparison and a photocenter (centroid) shift analysis — and find no evidence of either an eclipsing binary signature or background contamination. We report these findings as a positive, though non-confirming, vetting result and discuss the transit depth (~1%) as an open question warranting further investigation.

1. INTRODUCTION

The Transiting Exoplanet Survey Satellite (TESS) has generated thousands of TESS Objects of Interest (TOIs) — candidate planetary transit signals awaiting confirmation. Due to the volume of candidates, a persistent backlog exists of TOIs that have not yet undergone detailed vetting. This work presents a reproducible, open-source vetting pipeline applied to one such candidate, TOI-167.01, as an exercise in — and contribution to — the broader community effort of candidate triage.

This analysis was conducted as an independent, self-directed project using only public data and free software tools, with the goal of producing a genuine, checkable scientific contribution.

2. TARGET
TIC ID: 149990841
TOI: 167.01
Catalog period: 4.453 days
TESS magnitude: 11.6349 ± 0.006
Confirmed planets: None (as of analysis date)
Data source: ExoFOP-TESS

3. DATA AND METHODS
3.1 Data acquisition

Light curve data were retrieved via the lightkurve Python package (v2.x) from the Mikulski Archive for Space Telescopes (MAST), using SPOC (Science Processing Operations Center) pipeline products. To avoid duplicate/conflicting exposure cadences, only 2-minute cadence (exptime=120) products were retained, yielding 25 sectors spanning 2019–2025.

3.2 Preprocessing

Individual sector light curves were stitched into a single continuous time series, with NaN values removed and outliers beyond 6σ clipped. A rolling-window flatten (window_length=901, corresponding to ~30 hours) was applied to remove long-term stellar variability (rotation, pulsation) while preserving short-duration transit-like features.

3.3 Transit search

A Box Least Squares (BLS) periodogram search was performed over a period range of 2–8 days (bracketing the catalog value of 4.453 days) using frequency_factor=500. The period, transit epoch (t0), and duration corresponding to maximum BLS power were extracted.

3.4 Vetting checks

Two standard false-positive discriminant tests were applied:

Odd-even transit depth test: Transits were split by cycle parity (odd/even count from t0) and independently phase-folded. A significant depth discrepancy between odd and even transits is a signature of an eclipsing binary at half the true period.
Centroid (photocenter) shift test: Using target pixel file (TPF) data, the flux-weighted centroid position was computed per cadence and phase-folded on the candidate period. A systematic centroid shift localized at transit phase would indicate the signal originates from a background/nearby contaminating source rather than the target star.

All code and analysis steps are available in the accompanying Jupyter notebook.

4. RESULTS
4.1 Transit search

The BLS search recovered a dominant peak at P = 4.453045 days, matching the ExoFOP catalog value (4.453 days) to five significant figures. The best-fit transit epoch was t0 = 1546.284 (BTJD), with a transit duration of ~0.1 days (~2.4 hours).

<img width="953" height="462" alt="image" src="https://github.com/user-attachments/assets/24237906-175a-40b1-a685-1886055a3585" />


Notably, this result was stable across two independent data subsets analyzed during this work — an initial 4-sector subset and the full 25-sector dataset — with the recovered period unchanged, supporting the robustness of the detection against sector-specific systematics.

4.2 Phase-folded light curve

The phase-folded light curve shows a consistent, well-defined V-shaped dip centered at phase 0, with a depth of approximately 1.0–1.5% relative to the normalized out-of-transit flux.

<img width="975" height="463" alt="image" src="https://github.com/user-attachments/assets/63d8fe49-18a3-48a3-8b33-b3907d0d0c6c" />


4.3 Odd-even test

Odd- and even-numbered transits show consistent depth and shape upon independent phase-folding, with no statistically apparent offset between the two subsets.

<img width="1257" height="738" alt="image" src="https://github.com/user-attachments/assets/a159908f-37a6-4209-9311-81970eba5aec" />


Interpretation: This result is inconsistent with a simple eclipsing binary scenario in which BLS has locked onto half the true orbital period (which would manifest as alternating primary/secondary eclipse depths).

4.4 Centroid analysis

Centroid column and row positions, phase-folded on the candidate period, show no localized systematic shift coincident with the transit phase. Observed structure in the raw centroid time series was attributable to inter-sector instrumental offsets rather than transit-phase-correlated motion.

<img width="1243" height="725" alt="image" src="https://github.com/user-attachments/assets/dd136c38-3694-4ba4-befe-63af25472636" />
<img width="1231" height="711" alt="image" src="https://github.com/user-attachments/assets/f4e2ebee-4c7c-4d8a-8ef9-0fe3a0e53906" />

4.5 Planet radius estimate

Using the transit depth and the stellar radius reported on ExoFOP (user-uploaded stellar parameters: 1.09 R☉ and 1.14 R☉, averaged to 1.115 R☉), the planet radius was estimated from the standard transit relation:

R_planet = R_star × √(transit depth)

With a transit depth of approximately 1%, this yields:

R_planet ≈ 0.112 R☉
R_planet ≈ 12.2 R⊕ (Earth radii)
R_planet ≈ 1.09 × R_Jupiter

This places the candidate in the gas-giant regime, comparable in size to Jupiter — a physically reasonable and common category of object for TESS to detect, given that larger planets produce deeper, more easily recovered transit signals. This estimate is an independent calculation by the authors and has not been cross-checked against a dedicated transit model fit; it should be treated as an order-of-magnitude approximation rather than a precise measurement.

5. Discussion

The candidate passes both vetting tests applied in this work. Combined with the high-precision period match to the catalog value and the stability of the detection across independent data subsets, this constitutes a positive — though not confirmatory — vetting outcome.

Transit depth and radius: The observed transit depth (~1%), combined with the radius estimate in Section 4.5, is consistent with a Jupiter-sized gas giant rather than an unusually large small planet. This resolves the ambiguity noted in earlier iterations of this analysis, though the estimate remains approximate and photometry alone cannot rule out a low-mass stellar/substellar companion of similar radius (e.g., a brown dwarf).
Interpretation: This result is inconsistent with the signal originating from a background eclipsing binary or nearby contaminating source within the photometric aperture.

6. LIMITATIONS
This analysis is subject to important limitations that preclude a confirmation claim:

No radial velocity or independent photometric follow-up was performed or is accessible to this study.
The centroid analysis used pipeline aperture masks and did not include a full pixel-level difference-imaging analysis.
Stellar parameters (radius, mass) were not independently derived or verified against the TIC catalog.
A rigorous statistical false-alarm probability for the BLS detection was not computed.

7. CONCLUSION

TOI-167.01 passes standard odd-even and centroid vetting checks using an independent, reproducible open-source pipeline applied to the full available TESS photometric dataset. The transit period is recovered with high precision and is stable across data subsets. The candidate's large transit depth remains an open question best resolved through spectroscopic follow-up. This work is offered as a genuine, checkable contribution to the TOI vetting backlog and as a reproducible template for similar candidate assessments.

Data and Code Availability

Analysis code (Jupyter notebook) is available at: [insert GitHub link] Data source: MAST/TESS SPOC light curves, publicly available via lightkurve.

References
Ricker, G. R., et al. 2015, JATIS, 1, 014003 (TESS mission)
Lightkurve Collaboration, 2018, "Lightkurve: Kepler and TESS time series analysis in Python," Astrophysics Source Code Library
ExoFOP-TESS, https://exofop.ipac.caltech.edu/tess/
