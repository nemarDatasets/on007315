[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on007315-blue)](https://doi.org/10.82901/nemar.on007315)

**Title:** Transcranial Alternating Current Stimulation (tACS) for Patients with Post-Stroke Anomia: Preliminary Data on Picture Naming Performance

**Dataset Description:**\
This dataset includes EEG recordings from two post-stroke patients with chronic anomia who participated in an 8-week individualized neuromodulation intervention using transcranial alternating current stimulation (tACS). The intervention alternated between stimulation and non-stimulation phases every two weeks and was designed to enhance naming abilities via cortical entrainment, guided by individual EEG profiles.

**Data Overview:**

- **Participants:** 2 individuals with post-stroke anomia (1 left-hemisphere lesion, 1 right-hemisphere lesion)
- **Sessions:** EEG recorded every two weeks during the intervention (W1, W2, W4, W6, W8), and at follow-ups (W12, W20)
- **Stimulation:** tACS applied during alternating weeks; frequency and montage were personalized based on initial EEG
- **Tasks:** Picture naming task using a standardized set of stimuli; EEG recorded during task execution
- **Modality:** EEG (recorded using Starstim-32), processed in EEGLAB and prepared for BIDS

**Experimental Design:**\
A single-case experimental design (SCED, ABAB type) was employed. Behavioral and EEG data were collected across 24 naming sessions and 6 EEG recording sessions per participant. The data includes tACS and no-tACS conditions.

**Purpose:**\
To investigate whether tACS improves naming accuracy and latency in chronic aphasia and whether those effects are sustained after intervention.

**Data Notes:**

- EEG recordings are organized in BIDS format, with sessions labeled by week (e.g., `week-01`, `week-12`)
- Session and run numbers reflect weeks of intervention

**Ethics:**\
All participants provided written informed consent. The study was approved by the Ethics Committee of the Medical School of Ioannina (approval nr. 49625) and conducted in accordance with the Declaration of Helsinki.

**Contact:**\
For questions about the dataset, contact Maria Martzoukou (<m.martzoukou@uoi.gr>)

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 1 error + 743 warnings to 0 errors + 379 warnings. None of the raw `.set`/`.fdt` files were modified — every change is to a text sidecar.

**Participants table (`participants.tsv`)**
- The participant identifiers were `sub-01` and `sub-02`, but the on-disk subject directories are `sub-G01` and `sub-G02`. The values were updated to `sub-G01`/`sub-G02` so the table agrees with the directory names. Other columns (`age`, `sex`, `handedness`, `lesion_side`, `stroke_years_ago`) are unchanged. This change was carried over from EEGDash's on-load repair and baked into the rehost.

**Channel tables (`sub-G*/ses-*/eeg/*_channels.tsv`, 14 files)**
- The `type` column was `n/a` for every row; it now reads `EEG` for all 32 rows in each file, matching what the recordings actually contain (32 scalp electrodes named with standard 10-20/10-10 labels). Channel names are unchanged.
- The `units` column was `n/a`; it now reads `uV`, which is the standard unit for scalp EEG processed through EEGLAB.

**Events sidecar (`task-PictureNaming_events.json`, root)**
- Added `Units: "s"` to the `onset`, `duration`, and `response_time` columns. BIDS expects an SI symbol on time columns, and seconds is what the per-recording event tables already contain.
- Added column descriptions for `sample` and `value`, which appear in every events table but were previously undocumented.

**Recording sidecar (`task-PictureNaming_eeg.json`, root, new)**
- Records the channel inventory (`EEGChannelCount: 32`, all other modality counts `0`), which matches every `channels.tsv` in the dataset (32 EEG rows, no rows of other types).
- Records the electrode layout (`EEGPlacementScheme: "10-20"`), justified by the channel names in every `channels.tsv`: Fp1, Fp2, F3, Fz, F4, F7, F8, FC1, FC2, FC5, FC6, C3, Cz, C4, T7, T8, CP1, CP2, CP5, CP6, P3, Pz, P4, P7, P8, PO3, PO4, O1, Oz, O2, AF3, AF4.
- Records the hardware (`Manufacturer: "Neuroelectrics"`, `ManufacturersModelName: "Starstim-32"`, `SoftwareVersions: "EEGLAB 2023"`), all of which are stated in this README and the source's `dataset_description.json`.
- Adds a short `TaskDescription` paraphrased from this README (picture naming task across the 7 timepoints of the 8-week tACS intervention).

**Dataset description (`dataset_description.json`)**
- Updated `BIDSVersion` to `1.11.1` (the version the current validator checks against).
- `GeneratedBy` was kept exactly as the source published it — a single EEGLAB 2023 entry recording the original BIDS-conversion tooling. Nothing else was added there.

**Remaining warnings (379) — left on purpose**
- All remaining warnings are "recommended but missing" sidecar fields that need information from the study, lab, or equipment that isn't in the dataset (for example: device serial number, institution name and address, cap manufacturer and model, ground electrode location, head circumference, hardware filters, subject artefact description, stimulus-presentation details, cognitive-atlas IDs, HED schema version). They were left blank rather than filled with guesses. `GeneratedBy` is not among them, since the source already declares an EEGLAB entry.
