# Jab FPS qualification

This report says whether Jab FPS plays every frame of actual play under its 15 ms ceiling on each target. A run passes when its median frame is under 15 ms. Every run is judged on its own.

## The candidate

- Tag: none, so this is no qualification.
- jab at e21c5aeee99925966e543be9e411c3c24819c5a9, jab-stress at e5a05442406e97e6ddd32d019fdca15b0b55885c.
- The image's SHA-256 is 97cc681a036fe9c452b0fc74de53398d0a4519deefc0e0a0cc4abb53e271cf51. The kernel's is cb203ae1fe4a3ec0ccdd1f987d1b57ca5c5ea95a09ff433a1c1def0f3bb11fd2.
- The asset tree, render_0, has the digest 10b33d35c19a6501ea13c68a46d69dbf80a1631c9568e6386e9e4484b2ec668d.
- The toolchain is GNU assembler (GNU Binutils) 2.47.20260726.

## 1KB1

The qualification on 1KB1 does not pass. 7 of its 7 acceptance runs pass, 0 expected runs are missing, and the check names 8 problems below.
The bench game/fps/qual_1kb1 ran at 20261017-192135. 10 of its 11 steps ran to their end.

### The acceptance runs

Each run passes when it stands valid, runs the candidate on 1KB1, and its critical path's median frame is under 15 ms. Each run is judged on its own.

| Run | Seed | Median frame | Frames at or over 15 ms | Verdict |
| --- | --- | --- | --- | --- |
| auto_1 run 1 | 1 | 10.8 ms | 44 of 3311 | pass |
| auto_1 run 2 | 2 | 10.8 ms | 44 of 3312 | pass |
| auto_1 run 3 | 3 | 10.4 ms | 31 of 3309 | pass |
| auto_2 run 1 | 3 | 11.4 ms | 70 of 3310 | pass |
| auto_2 run 2 | 2 | 10.6 ms | 38 of 3304 | pass |
| auto_2 run 3 | 1 | 10.7 ms | 26 of 3306 | pass |
| free run 1 | 1 | 10.4 ms | 12 of 3548 | pass |

### The route by leg

Each cell is the median over the normal play runs of each run's median frame, in milliseconds, with the least and the greatest run in brackets.

| Leg | Critical path | Drawing | Preparation | Raster | Flip | Presented interval | Submission age |
| --- | --- | --- | --- | --- | --- | --- | --- |
| start | 12.2 (10.4 to 12.3) | 10.0 (8.5 to 10.3) | 2.2 (2.0 to 2.4) | 7.7 (6.5 to 8.1) | 1.7 (1.6 to 1.9) | 16.7 (16.6 to 16.7) | 16.6 (16.5 to 16.6) |
| fight | 10.5 (10.2 to 11.3) | 8.5 (8.3 to 9.2) | 1.7 (1.7 to 1.9) | 6.7 (6.5 to 7.2) | 1.8 (1.8 to 1.9) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| spawn | 10.5 (10.3 to 12.0) | 8.6 (8.5 to 9.8) | 1.8 (1.8 to 2.1) | 6.7 (6.7 to 7.7) | 1.7 (1.6 to 1.9) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| yard | 11.1 (11.1 to 12.1) | 9.4 (9.2 to 10.2) | 1.7 (1.7 to 1.9) | 7.6 (7.5 to 8.3) | 1.6 (1.6 to 1.8) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| up | 11.4 (10.6 to 12.2) | 9.8 (8.9 to 10.3) | 2.1 (2.1 to 2.3) | 7.6 (6.8 to 7.9) | 1.6 (1.5 to 1.6) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| down | 10.1 (10.0 to 12.0) | 8.0 (7.8 to 9.8) | 1.6 (1.6 to 1.9) | 6.4 (6.3 to 7.8) | 1.9 (1.8 to 2.0) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| ramp | 10.2 (10.0 to 11.4) | 8.3 (8.2 to 9.4) | 1.1 (1.0 to 1.2) | 7.2 (7.1 to 8.2) | 1.6 (1.6 to 1.8) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| revisit | 10.2 (10.2 to 10.6) | 8.4 (8.4 to 8.7) | 1.8 (1.8 to 1.9) | 6.6 (6.5 to 6.9) | 1.7 (1.6 to 1.8) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |
| pressure | 11.2 (11.0 to 12.7) | 9.3 (9.2 to 10.6) | 2.3 (2.2 to 2.5) | 7.1 (6.9 to 8.2) | 1.6 (1.6 to 1.7) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.5) |

Over the normal play runs the androids were roused 24 times and destroyed 6 times, the player was struck 5 times, and 6 magazines were picked up.

### The tails

The frames at or over 15 ms are summed over the normal play runs. The percentiles are each run's own, as above.

| Leg | Frames at or over 15 ms | 95th | 99th | Greatest |
| --- | --- | --- | --- | --- |
| start | 13 of 251 | 14.6 (13.0 to 17.2) | - | 17.8 (16.6 to 20.5) |
| fight | 24 of 3223 | 13.3 (13.0 to 13.9) | 14.3 (14.2 to 15.1) | 16.7 (15.1 to 18.2) |
| spawn | 28 of 3375 | 13.0 (12.5 to 14.1) | 14.2 (13.5 to 16.2) | 15.9 (14.5 to 19.0) |
| yard | 15 of 2370 | 12.8 (12.4 to 13.9) | 13.8 (13.2 to 15.3) | 16.1 (14.1 to 18.1) |
| up | 47 of 1986 | 14.2 (13.4 to 15.8) | 14.9 (14.1 to 18.4) | 19.3 (16.4 to 23.1) |
| down | 11 of 1449 | 12.1 (11.5 to 14.4) | 13.5 (12.4 to 15.7) | 15.2 (12.8 to 16.4) |
| ramp | 9 of 1973 | 12.0 (11.8 to 13.2) | 13.8 (12.8 to 15.2) | 16.0 (13.3 to 19.2) |
| revisit | 14 of 3599 | 12.3 (11.6 to 13.5) | 13.8 (13.1 to 15.2) | 16.2 (14.2 to 17.8) |
| pressure | 92 of 1626 | 15.1 (14.3 to 15.7) | 16.6 (15.9 to 18.1) | 19.2 (18.4 to 23.5) |

### Presentation

The probed runs carry 3 of 3 bundles. Their shown, superseded, and repeated frames come from the probe's own reader.
The shim control does not find every effect within its noise, so the probed readings describe the instrumented runs only.

| Measure | Probed (us) | Plain (us) | Effect (us) | Noise (us) | Within |
| --- | --- | --- | --- | --- | --- |
| critical path median | 10573 | 10915 | -342 | 315 | no |
| drawing median | 8660 | 8986 | -326 | 309 | no |
| flip median | 1746 | 1736 | 10 | 33 | yes |
| presented interval median | 16663 | 16664 | -1 | 4 | yes |
| presented interval 95th | 17137 | 17187 | -50 | 32 | no |

### CPU and the guest's waits

Each thread's utilization is its CPU seconds over the readings' interval, from five seconds in to the route's end, as the median over the normal play runs.

| Thread | Utilization over the whole run |
| --- | --- |
| CPU 0/TCG | 0.13 (0.13 to 0.14) |
| CPU 1/TCG | 0.42 (0.4 to 0.44) |
| CPU 2/TCG | 0.41 (0.4 to 0.43) |
| CPU 3/TCG | 0.41 (0.4 to 0.43) |
| CPU 4/TCG | 0.0 (0.0 to 0.0) |
| CPU 5/TCG | 0.0 (0.0 to 0.0) |
| CPU 6/TCG | 0.0 (0.0 to 0.0) |

The guest waited in its await 0.0 (0.0 to 0.0) ms and before its flips 5.9 (5.3 to 6.3) ms at the median frame.

### The tile pool

The pool built 90200 tiles and evicted 77912 over the normal play runs. It held at most 2048 slots.

### The host

- Fedora Linux 44, Intel(R) Core(TM) i9-14900K, 32 cores
- QEMU emulator version 11.1.2 at /home/box/proj/sourcetrait/jab/extern/qemu/bin/qemu-system-riscv64, SHA-256 ccaecb8393dce3169e189f0437aed672290cfc661c8c27c1d8e7b292cfb9d6b8
- The program's RAM window is 4086 MiB of 8 GiB installed. The RAM past the window is held apart.
- QEMU's resident memory, read by the watch over 31 runs, was 431 MiB at the median and 432 MiB at the greatest peak.

### Worker scaling

The forced runs are judged by this comparison alone. Each cell is the median over the arm's runs.

| Leg | Forced critical path | Automatic critical path | Forced raster | Automatic raster |
| --- | --- | --- | --- | --- |
| start | 12.8 (12.6 to 13.0) | 12.3 (11.9 to 12.7) | 9.3 (9.2 to 9.4) | 7.6 (7.5 to 8.0) |
| fight | 12.3 (12.1 to 12.3) | 10.7 (10.3 to 11.9) | 9.0 (8.9 to 9.1) | 6.7 (6.5 to 7.7) |
| spawn | 12.4 (12.1 to 12.7) | 10.5 (10.4 to 12.0) | 9.1 (8.8 to 9.2) | 6.8 (6.6 to 7.9) |
| yard | 14.6 (14.5 to 14.8) | 11.2 (11.1 to 12.2) | 11.0 (10.9 to 11.1) | 7.7 (7.6 to 8.2) |
| up | 14.2 (13.6 to 15.2) | 11.2 (11.0 to 12.4) | 10.3 (9.9 to 11.1) | 7.4 (7.2 to 8.1) |
| down | 11.1 (11.0 to 11.9) | 10.1 (9.9 to 11.8) | 8.1 (8.0 to 8.6) | 6.3 (6.2 to 7.6) |
| ramp | 12.7 (12.6 to 12.8) | 10.2 (10.1 to 11.4) | 9.8 (9.8 to 10.0) | 7.1 (7.0 to 8.2) |
| revisit | 12.6 (12.5 to 12.8) | 10.3 (10.2 to 10.6) | 9.2 (9.1 to 9.3) | 6.5 (6.5 to 6.8) |
| pressure | 14.2 (13.9 to 14.4) | 11.4 (11.3 to 11.9) | 9.9 (9.8 to 10.2) | 7.3 (7.3 to 7.6) |

### Problems

- The candidate is untagged, so no qualification stands on it.
- The bench: step probed_2 ended failed, exit 1.
- Presentation's shim control: probed_2 run 1 is missing.
- Presentation's shim control: probed_2 run 2 is missing.
- Presentation's shim control: probed_2 run 3 is missing.
- Presentation: probed_2 run 1 is missing.
- Presentation: probed_2 run 2 is missing.
- Presentation: probed_2 run 3 is missing.

### Limits

- The physical display is not qualified. TheUser's eye is a qualitative reading and never a measured 60 frames a second.
- A thread's CPU comes from its host's counters, hundredths of a second read every 100 ms, so only an interval of seconds resolves. One less a thread's utilization is everything it did not run, descheduling included, and is never the guest's idle time; the guest's own waits come from its records.
- Upload and render selection come from the probed runs alone. They stand for the unprobed program's presentation only where the shim control finds every effect within its noise.
- A release run carries no sound counters. The audio's evidence comes from separate debug runs, labelled diagnostic.
