TOI-167.01 Vetting Analysis

Independent photometric vetting of TOI-167.01 (TIC 149990841), an unconfirmed TESS Object of Interest, using publicly available TESS light curve data and open-source tools.

Summary
Target: TIC 149990841 (TOI-167.01)
Recovered period: 4.453045 days (matches ExoFOP catalog value of 4.453 days)
Data used: 25 TESS sectors (2019–2025), 2-minute cadence, SPOC pipeline
Vetting tests applied: Odd-even transit depth comparison, centroid (photocenter) shift analysis
Result: Candidate passes both vetting checks; recovered period is stable across independent data subsets. Transit depth (~1%) remains an open question — see full report for discussion.

<img width="972" height="465" alt="image" src="https://github.com/user-attachments/assets/1755e39d-b91d-4a6e-8048-228b6c83f933" />


Repository Structure

notebooks/   → Full analysis notebook (Jupyter)

reports/     → Written vetting report (Markdown)

figures/     → Saved plots referenced in the report and notebook

Methodology
Search and download TESS light curves via lightkurve (SPOC pipeline, 2-min cadence)
Stitch sectors, remove NaNs and outliers, flatten to remove stellar variability
Run a Box Least Squares (BLS) periodogram search
Phase-fold on the best-fit period
Odd-even transit depth test (checks for eclipsing binary signatures)
Centroid shift test using target pixel files (checks for background contamination)

Full methodology and results are in reports/TOI-167_vetting_report.md.

TESS light curves retrieved from the Mikulski Archive for Space Telescopes (MAST) via the lightkurve Python package. Candidate parameters from ExoFOP-TESS.

Disclaimer:
This is an independent, community-level vetting analysis and does not constitute official TFOP (TESS Follow-up Observing Program) confirmation. See the limitations section of the full report for details.

Author:
Josip Đurić Vadinjof, Faculty of Electrical Engineering and Computing (FER), University of Zagreb
