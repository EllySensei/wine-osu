# wine-osu

# OUTDATED: use gonX's patched version instead! https://drive.google.com/drive/folders/17MVlyXixv7uS3JW4B-H8oS4qgLn7eBw5

Original PKGBUILD from ThePooN and Geno: https://gitlab.com/telastrus/wine-osu & https://blog.thepoon.fr/osuLinuxAudioLatency
Original Patches from gonX, openglfreak and tdeo
**Join ThePooN's Discord Server for any questions and problems regarding running osu! using Wine https://discord.com/invite/thepoon**

The `master` branch uses stable wine and seems to work the best, but the `staging` branch is available if you want to play around. 

**NOTE: CURRENTLY AUDIO COMPATIBILITY MUST BE TURNED ON IN osu! FOR THIS TO WORK** (probably a bug, still investigating)

**NOTE: MUST BE BUILT WITH GCC UNDER 10 AS IT BREAKS ONE OF THE DLL FILES, WILL FIND THE FIX LATER AS THIS IS A WORKAROUND**

Environment variables:

- `STAGING_AUDIO_DURATION` - set the staging audio duration; I've had success with this being set to 20000, but this varies from PC to PC, so I recommend tweaking this to your liking
- `STAGING_AUDIO_PERIOD` - set the staging audio period; I use 15000, but same disclaimer from above applies
- `STAGING_TLENGTH_USEC` - can also help with latency but will update on where it is used.

**SYSTEM OPTIONS**
- `PULSE_LATENCY_MSEC` - an option to reduce latency on the system, I use 10 but it varies PC to PC
