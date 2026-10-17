# Jab FPS qualification

This report says whether Jab FPS plays every frame of actual play under its 15 ms ceiling on each target. A run passes when its median frame is under 15 ms. Every run is judged on its own.

## The candidate

- Tag: none, so this is no qualification.
- jab at e21c5aeee99925966e543be9e411c3c24819c5a9, jab-stress at e5a05442406e97e6ddd32d019fdca15b0b55885c.
- The image's SHA-256 is 97cc681a036fe9c452b0fc74de53398d0a4519deefc0e0a0cc4abb53e271cf51. The kernel's is cb203ae1fe4a3ec0ccdd1f987d1b57ca5c5ea95a09ff433a1c1def0f3bb11fd2.
- The asset tree, render_0, has the digest 10b33d35c19a6501ea13c68a46d69dbf80a1631c9568e6386e9e4484b2ec668d.
- The toolchain is GNU assembler (GNU Binutils) 2.47.20260726.

## 1KA1

The qualification on 1KA1 does not pass. 7 of its 7 acceptance runs pass, 0 expected runs are missing, and the check names 1 problems below.
The bench game/fps/qual_1ka1 ran at 20261017-181257. 11 of its 11 steps ran to their end.

### The acceptance runs

Each run passes when it stands valid, runs the candidate on 1KA1, and its critical path's median frame is under 15 ms. Each run is judged on its own.

| Run | Seed | Median frame | Frames at or over 15 ms | Verdict |
| --- | --- | --- | --- | --- |
| auto_1 run 1 | 1 | 12.9 ms | 488 of 3300 | pass |
| auto_1 run 2 | 2 | 12.7 ms | 483 of 3302 | pass |
| auto_1 run 3 | 3 | 12.7 ms | 399 of 3301 | pass |
| auto_2 run 1 | 3 | 12.6 ms | 384 of 3313 | pass |
| auto_2 run 2 | 2 | 12.6 ms | 400 of 3299 | pass |
| auto_2 run 3 | 1 | 12.7 ms | 390 of 3304 | pass |
| free run 1 | 1 | 12.8 ms | 45 of 3546 | pass |

### The route by leg

Each cell is the median over the normal play runs of each run's median frame, in milliseconds, with the least and the greatest run in brackets.

| Leg | Critical path | Drawing | Preparation | Raster | Flip | Presented interval | Submission age |
| --- | --- | --- | --- | --- | --- | --- | --- |
| start | 12.9 (12.9 to 14.1) | 11.0 (11.0 to 12.1) | 1.7 (1.7 to 2.5) | 9.3 (9.3 to 9.4) | 1.6 (1.6 to 1.7) | 16.6 (16.6 to 16.7) | 16.5 (16.5 to 16.5) |
| fight | 12.2 (12.1 to 12.4) | 10.5 (10.4 to 10.6) | 1.4 (1.4 to 1.4) | 9.0 (8.9 to 9.1) | 1.6 (1.5 to 1.6) | 16.7 (16.6 to 16.7) | 16.5 (16.5 to 16.6) |
| spawn | 12.1 (11.7 to 12.7) | 10.2 (9.8 to 10.9) | 1.4 (1.3 to 1.7) | 8.9 (8.6 to 9.2) | 1.6 (1.5 to 1.6) | 16.7 (16.6 to 16.7) | 16.5 (16.5 to 16.5) |
| yard | 14.7 (14.7 to 14.9) | 12.8 (12.6 to 12.9) | 1.7 (1.6 to 1.7) | 11.1 (11.0 to 11.1) | 1.7 (1.7 to 1.7) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.5) |
| up | 13.9 (13.9 to 15.3) | 12.0 (12.0 to 13.3) | 2.1 (2.0 to 2.2) | 10.0 (10.0 to 11.1) | 1.6 (1.6 to 1.7) | 16.7 (16.6 to 16.7) | 16.5 (16.5 to 16.6) |
| down | 11.2 (10.9 to 11.6) | 9.4 (9.2 to 9.9) | 1.3 (1.2 to 1.3) | 8.2 (8.0 to 8.6) | 1.6 (1.4 to 1.6) | 16.7 (16.6 to 16.7) | 16.5 (16.5 to 16.6) |
| ramp | 12.8 (12.7 to 13.0) | 10.9 (10.9 to 11.2) | 0.9 (0.9 to 0.9) | 9.9 (9.9 to 10.1) | 1.6 (1.6 to 1.6) | 16.7 (16.6 to 16.7) | 16.5 (16.5 to 16.6) |
| revisit | 12.4 (12.2 to 12.6) | 10.6 (10.4 to 10.9) | 1.6 (1.5 to 1.6) | 9.1 (8.9 to 9.3) | 1.6 (1.6 to 1.7) | 16.7 (16.6 to 16.7) | 16.6 (16.5 to 16.6) |
| pressure | 14.3 (14.0 to 14.6) | 12.4 (12.0 to 12.6) | 2.1 (2.0 to 2.2) | 10.1 (9.9 to 10.3) | 1.7 (1.6 to 1.7) | 16.7 (16.7 to 16.7) | 16.5 (16.5 to 16.6) |

Over the normal play runs the androids were roused 24 times and destroyed 6 times, the player was struck 5 times, and 6 magazines were picked up.

### The tails

The frames at or over 15 ms are summed over the normal play runs. The percentiles are each run's own, as above.

| Leg | Frames at or over 15 ms | 95th | 99th | Greatest |
| --- | --- | --- | --- | --- |
| start | 20 of 268 | 15.0 (14.4 to 19.5) | - | 21.3 (21.0 to 23.3) |
| fight | 21 of 3212 | 13.6 (13.3 to 13.7) | 14.6 (14.4 to 15.4) | 19.6 (19.3 to 21.3) |
| spawn | 40 of 3442 | 13.8 (13.6 to 14.3) | 14.9 (14.5 to 15.5) | 16.6 (15.2 to 19.7) |
| yard | 921 of 2293 | 16.2 (16.1 to 16.7) | 17.4 (17.0 to 18.1) | 18.7 (17.7 to 23.6) |
| up | 714 of 1977 | 16.5 (16.3 to 18.1) | 18.8 (18.5 to 19.4) | 21.8 (18.6 to 24.1) |
| down | 55 of 1445 | 14.9 (14.8 to 15.0) | 16.1 (15.7 to 18.0) | 17.8 (16.9 to 19.3) |
| ramp | 58 of 1974 | 14.3 (14.1 to 14.7) | 15.8 (15.1 to 17.3) | 17.2 (16.2 to 21.1) |
| revisit | 46 of 3603 | 14.1 (13.7 to 14.3) | 15.3 (14.3 to 15.8) | 16.6 (14.9 to 19.7) |
| pressure | 669 of 1605 | 18.4 (18.3 to 19.1) | 20.6 (20.3 to 21.0) | 20.9 (20.9 to 22.5) |

### Presentation

The probed runs carry 6 of 6 bundles. Their shown, superseded, and repeated frames come from the probe's own reader.
The shim control does not find every effect within its noise, so the probed readings describe the instrumented runs only.

| Measure | Probed (us) | Plain (us) | Effect (us) | Noise (us) | Within |
| --- | --- | --- | --- | --- | --- |
| critical path median | 12663 | 12624 | 39 | 75 | yes |
| drawing median | 10858 | 10858 | 0 | 48 | yes |
| flip median | 1651 | 1601 | 50 | 22 | no |
| presented interval median | 16659 | 16659 | 0 | 2 | yes |
| presented interval 95th | 17165 | 17140 | 25 | 39 | yes |

### CPU and the guest's waits

Each thread's utilization is its CPU seconds over the readings' interval, from five seconds in to the route's end, as the median over the normal play runs.

| Thread | Utilization over the whole run |
| --- | --- |
| CPU 0/TCG | 0.11 (0.11 to 0.12) |
| CPU 1/TCG | 0.56 (0.56 to 0.57) |
| CPU 2/TCG | 0.56 (0.56 to 0.57) |

The guest waited in its await 0.0 (0.0 to 0.0) ms and before its flips 4.0 (3.8 to 4.1) ms at the median frame.

### The tile pool

The pool built 93721 tiles and evicted 81433 over the normal play runs. It held at most 2048 slots.

### The host

- Fedora Linux 44, Intel(R) Core(TM) i9-14900K, 32 cores
- QEMU emulator version 11.1.2 at /home/box/proj/sourcetrait/jab/extern/qemu/bin/qemu-system-riscv64, SHA-256 ccaecb8393dce3169e189f0437aed672290cfc661c8c27c1d8e7b292cfb9d6b8
- The program's RAM window is 4086 MiB of 4 GiB installed. The RAM past the window is held apart.
- QEMU's resident memory, read by the watch over 31 runs, was 426 MiB at the median and 427 MiB at the greatest peak.

### Worker scaling

The forced runs are judged by this comparison alone. Each cell is the median over the arm's runs.

| Leg | Forced critical path | Automatic critical path | Forced raster | Automatic raster |
| --- | --- | --- | --- | --- |
| start | 22.0 (21.5 to 22.3) | 12.8 (12.6 to 12.9) | 18.3 (18.2 to 18.4) | 9.3 (9.3 to 9.4) |
| fight | 21.0 (20.8 to 21.1) | 12.1 (12.1 to 12.3) | 17.7 (17.6 to 17.8) | 9.0 (8.9 to 9.1) |
| spawn | 20.9 (19.6 to 21.4) | 12.3 (12.1 to 12.5) | 17.5 (16.6 to 17.9) | 9.0 (8.9 to 9.2) |
| yard | 25.2 (25.0 to 25.4) | 14.5 (14.5 to 14.8) | 21.6 (21.5 to 21.8) | 10.9 (10.9 to 11.2) |
| up | 23.2 (22.8 to 24.5) | 15.0 (13.9 to 15.2) | 19.4 (19.1 to 20.4) | 11.0 (10.0 to 11.1) |
| down | 19.0 (18.8 to 19.1) | 11.1 (11.0 to 11.8) | 16.0 (15.8 to 16.0) | 8.1 (8.0 to 8.6) |
| ramp | 22.7 (22.6 to 23.0) | 12.7 (12.7 to 13.0) | 19.6 (19.5 to 19.8) | 9.9 (9.8 to 10.0) |
| revisit | 21.6 (20.2 to 21.9) | 12.5 (12.1 to 12.7) | 18.0 (17.1 to 18.3) | 9.1 (8.8 to 9.2) |
| pressure | 23.7 (23.5 to 23.9) | 14.2 (13.9 to 14.4) | 19.2 (19.0 to 19.4) | 10.0 (9.9 to 10.1) |

### Problems

- The candidate is untagged, so no qualification stands on it.

### Limits

- The physical display is not qualified. TheUser's eye is a qualitative reading and never a measured 60 frames a second.
- A thread's CPU comes from its host's counters, hundredths of a second read every 100 ms, so only an interval of seconds resolves. One less a thread's utilization is everything it did not run, descheduling included, and is never the guest's idle time; the guest's own waits come from its records.
- Upload and render selection come from the probed runs alone. They stand for the unprobed program's presentation only where the shim control finds every effect within its noise.
- A release run carries no sound counters. The audio's evidence comes from separate debug runs, labelled diagnostic.
