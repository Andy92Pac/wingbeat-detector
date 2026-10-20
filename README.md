# Wingbeat Detector

A single-page tool that listens for mosquitoes. It finds the steady 280–950 Hz wingbeat tone and its harmonics, either live through the phone's microphone or in a recording you load.

**Open it:** https://andy92pac.github.io/wingbeat-detector/

Use Safari or Chrome directly; in-app browsers often block the microphone. Everything runs in the browser and no audio leaves the device.

## How it works

1. Audio is low-pass filtered and resampled to about 8 kHz, then cut into 128 ms FFT frames every 32 ms.
2. Each frequency bin is compared with a low percentile of its own level over time, so steady fans and electrical hum drop out.
3. For every candidate pitch between 280 and 950 Hz, the detector scores how far the first four harmonics stand above the middle of the gaps between them. Voices and most noise put energy in those gaps; a wingbeat does not.
4. A pass is reported when that pitch holds within 10% for about a third of a second.

**Long range** (the default for live listening) averages over about 0.8 s and relaxes the harmonic threshold. On real recordings made fainter by 24–36 dB, it caught passes about 6 dB further down than Normal, which is roughly twice the distance. In a simulated 12-minute night with snoring it raised no false alarms.

**Alerts:** while listening live, a detection turns the screen amber, beeps three times at 2.9 kHz (at most every 15 s, switchable) and vibrates on Android. Night screen keeps the display black until then. Put the phone by your pillow; a phone microphone can't reliably cover a whole room.

Checked against real phone recordings from Stanford's Abuzz dataset ([data on Zenodo](https://zenodo.org/records/4964774)): house mosquitoes (*Culex pipiens*) and tiger mosquitoes (*Aedes albopictus*) were found in all 14 recordings tested, and nothing in the background-only recording. Music, singing and some high-pitched voices also trigger it.
