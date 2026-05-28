# cornerman-audio-debug-viewer

Debug viewer for the Cornerman Phase-2 audio impact classifier (mel-CNN v3).
For each bagwork round it shows the model's per-time impact probability
stream alongside the labeled punch spans, the librosa-detected impact
moments, and the discrete peaks the model would emit at a given threshold.

Live: https://tradermathe.github.io/cornerman-audio-debug-viewer/

## What you see

- **Top:** audio player for the round.
- **Threshold τ slider:** moves the horizontal line on the prob plot. Peaks
  above τ are marked as orange dots. Stats update live.
- **Timeline canvas:**
  - Blue area: mel-CNN prob stream sampled at 80 ms hop.
  - Dashed orange: current τ.
  - Red bars: lead-hand GT spans. Blue bars: rear-hand GT spans.
  - Green vertical lines: per-positive impact moments (librosa onset-strength
    peak inside the GT span — this is what the classifier was trained on).
  - Orange dots: local maxima of the prob stream above τ — what the model
    would emit as discrete predicted events.
  - Red vertical playhead syncs with the audio player.
- **Click anywhere on the timeline** to seek.
- **Hover** to read prob, the GT span you're on, and Δ to nearest impact.

## Stats

- `recall` = fraction of GT midpoints with max prob ≥ τ within ±150 ms.
- `hits` = matched / total GT.
- `peaks` = discrete predicted events.
- `lift` = recall − chance (chance = ±150 ms union coverage of predicted
  windows / round duration). Green ≥ +5 pp, red < 0.

## Notes

- Video playback is disabled in the public build (source videos are
  copyrighted YouTube workouts; not redistributed). Run the local viewer
  in the cornerman-backend repo for video.
- All 160 bagwork rounds (110 min of audio) are included.
- Per-round predictions use the held-out fold's mel-CNN (5-fold CV).

## Local version

Source: `cornerman-backend/audio_impact_phase2/viewer/`. The local version
shows video too. Run with `python3 -m http.server` from the phase-2 dir.
