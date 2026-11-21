[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on006914-blue)](https://doi.org/10.82901/nemar.on006914)

This dataset, used in the analysis reported by Kochi et al., (2025), contains intracranial EEG recordings from 110 individuals who performed an visual‑naming task. Electrode coordinates are provided in MNI‑305 space.

Each EDF file is tagged for the visual naming task with the following event codes:  
401 – stimulus onset  
501 – response onset  

Reference: 
Ryuzaburo Kochi, Aya Kanno, Hiroshi Uda, Keisuke Hatano,  Masaki Sonoda, Hidenori Endo, Michael Cools, Robert Rothermel, Aimee F. Luat, Eishi Asano. Whole-Brain Millisecond-Scale Effective Connectivity Atlases of Speech 

## Channel names follow the data headers (18 Oct 2026)

In 353 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count). In the 353 with a different channel count, `mne_bids.read_raw_bids` warned and kept the header names, so the channels.tsv types and bad-channel status reached only the channels whose names happen to be identical; a reader that applies the channels.tsv rows by position gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 0 rows took the header name.

17648 header channels had no channels.tsv row (353 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-007_ses-1_task-picture_ieeg.edf`: POL A1-A2, POL A11-A12, POL A17-A18, POL A23-A24; `sub-007_ses-2_task-picture_ieeg.edf`: POL A26-A31, POL A27-A32, POL DC01, POL DC02; `sub-007_ses-3_task-picture_ieeg.edf`: POL B1-B2, POL B17-B18, POL B25-B26, POL B33-B34.

45762 channels.tsv rows had no channel in the data file (353 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-007_ses-1_task-picture_ieeg.edf`: A1, A2, A3, A4, A5; `sub-007_ses-2_task-picture_ieeg.edf`: A1, A2, A3, A4, A5; `sub-007_ses-3_task-picture_ieeg.edf`: A1, A2, A3, A4, A5.

Applied by position, the former rows would have given wrong labels to 17646 channels in 353 files (rows missing, extra or out of order before that position).

Bipolar data: in 350 files the data header holds bipolar derivations named after two contacts (e.g. `POL A1-A2`), while channels.tsv listed the monopolar contacts (`A1`, `A2`, ...). A derivation is not matched to the row of its first contact: each of the 16930 bipolar channels got its own row (type = the type of both contacts, `reference` = the second contact, columns shared by both contact rows copied, `status` bad when either contact was bad (5844 rows), good when both were good (11086 rows)), and the contact rows not recorded as such moved to `sourcedata/channels-removed-2026-10-18/`. electrodes.tsv (contact positions) is unchanged; a derivation `X-Y` lies between contacts X and Y.

## Electrode locations (19 Oct 2026; revised 22 Oct 2026)

Coordsystem.json Other -> MNI305/mm in 353 files. Evidence: README: "Electrode coordinates are provided in MNI-305 space." Paper bioRxiv 10.1101/2025.11.18.689014 (Methods, Intracranial electrodes): "Electrode coordinates were transformed into the FreeSurfer standard brain space". The former files said Other / patient-native RAS / mm, so MNE labelled the frame unknown instead of mni_tal.

Primary source: this dataset's own README (line 3 of README.md on main, nemarDatasets/on006914@1956b035), quoted above.

Reproducible check (22 Oct 2026): for each of the 353 electrodes.tsv, every contact's x/y/z was compared (to 0.001 mm) with every electrodes.tsv of other deposits from the same group at pinned nemarDatasets commits (on004551@c4c035f6, on004859@db21ebb2, on005007@b59363aa, on005545@948f2825, on005931@cbd529e5, on006107@b37dcbfc, on006233@396692f0, on006910@9eb96ebd, on007118@8f2c41fa, on007119@c099745b, on007120@66296ebd). 353 files have all contacts identical to one file of another deposit: 198 to a deposit whose coordsystem.json declares fsaverage/mm (on007118, on007119, on007120) and 353 to a deposit whose README states MNI305/MNI-305 (on004551, on005545, on006233, on006910); a file can match both.

Limitation: these sidecars declare the frame and units only. electrodes.tsv is unchanged and no coordinate value changed. Positions do not attach to channels in mne_bids or EEGDash, because the channel names (data-header labels, e.g. prefixed labels or bipolar derivations) differ from the contact names in electrodes.tsv; a channel-to-contact mapping is not part of this change. This dataset mirrors OpenNeuro; the same change belongs upstream.
