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

## NEMAR curation changes (2026-05-21)

BIDS validator: 1 error + 743 warnings → 0 errors + 379 warnings. Raw `.set`/`.fdt` binary payloads unchanged.

### `participants.tsv`
- `participant_id` values `sub-01`/`sub-02` → `sub-G01`/`sub-G02` to match the on-disk subject directory names. Closes the `PARTICIPANT_ID_MISMATCH` error. Other columns (`age, sex, handedness, lesion_side, stroke_years_ago`) unchanged.

### `dataset_description.json`
- Appended a second `GeneratedBy` entry recording the NEMAR rehost (`nemar-cli 0.8.8`); the existing EEGLAB 2023 entry is preserved.

### `task-PictureNaming_events.json`
- Added `Units: "s"` to `onset`, `duration`, and `response_time` (BIDS-canonical SI symbol). Closes 28 `TSV_COLUMN_TYPE_REDEFINED` warnings via inheritance.
- Added `sample` and `value` column definitions. Closes 28 `TSV_ADDITIONAL_COLUMNS_UNDEFINED` warnings via inheritance.

### `task-PictureNaming_eeg.json` (new, inheriting root sidecar)
- Channel counts: `EEGChannelCount: 32`, all other modality counts `0` (mechanical — every `channels.tsv` has 32 EEG rows, no rows of other types). Closes 168 channel-count warnings via inheritance.
- `EEGPlacementScheme: "10-20"` (mechanical — all 32 channel names in every `channels.tsv` are standard 10-20/10-10 electrode labels: Fp1/Fp2/F3/Fz/F4/F7/F8/FC1/FC2/FC5/FC6/C3/Cz/C4/T7/T8/CP1/CP2/CP5/CP6/P3/Pz/P4/P7/P8/PO3/PO4/O1/Oz/O2/AF3/AF4).
- `Manufacturer: "Neuroelectrics"`, `ManufacturersModelName: "Starstim-32"`, `SoftwareVersions: "EEGLAB 2023"` (mechanical — all stated explicitly in this README and the existing `dataset_description.json` `GeneratedBy` array).
- `TaskDescription` paraphrased from this README (picture naming task across 7 timepoints during the 8-week tACS intervention).

### `sub-G*/ses-*/eeg/*_channels.tsv` (14 files)
- `type` column: `n/a` → `EEG` for all 32 rows in each file. Channel `name` column unchanged.
- `units` column: `n/a` → `uV` for all 32 rows. Standard for scalp EEG processed through EEGLAB.
