# The data layout

This repository keeps the records of the test and measurement runs of Jab
and of its testing workbench, Jab Stress, and the data the workbench's
tests and recipes read. A run writes its files outside this repository
while it runs, and its record is copied in whole once it is done.

## Runs

A run's record lives at `runs/<subject>/<set>/<stamp>/`.

- The subject is the part of Jab the run tested: `sdk`, `kernel`, or
  `fps`, or `workbench` for the workbench's tests of its own runners.
- The set is what the run took. For a mutant set it is the set's file
  name less `_mutants.nuon`, so `demand_mutants.nuon` gives `demand`. A
  run of several sets as one file, or a runner's proofs of itself, is
  named for its runner: `program` is the fps packet, worker, and pool
  sets run together, and `deadline` is the checkout runner's deadline
  proofs. A probe's runs are named for the probe, such as `sdl`.
- The stamp is the run's start in UTC as `YYYYMMDDTHHMMSS`. A record
  that kept only its date has the date, `YYYYMMDD`, and the run's label
  after a dash, such as `20261014-gfxscaling`, so two runs of one date
  stay apart. Two runs that together took a set whole carry the program
  each ran as a label, such as `20261013T165603-workers`.

The directory holds the run's own files exactly as its runner wrote
them: its logs, results, baselines, and receipts, byte for byte. Nothing
in them is edited after the copy. Beside them sits `run.nuon`, the
manifest this repository adds.

## The manifest

`run.nuon` is one record with six fields.

- `set` and `stamp` are the run's set and stamp, as its path gives them.
- `task` is the name of the piece of work the run was made for, such as
  `IncludeKeyedBuild`.
- `scope` says how much of its set the run took.
  - `kind` is `whole` when the run took every item its set held at the
    time, and `targeted` when it took only some.
  - `ran` counts the items the run took, and `of` the items its set held
    at the time. A mutant set's items are its mutants. The deadline
    proofs' items are their scenarios, and a probe's are its runs.
  - `names` lists the items a targeted run took.
  - `combined` appears on each of two runs that together took their set
    whole. It names both runs and gives their joint `kind`, `ran`, and
    `of`.
- `builds` has one row for each part of the run, such as a shard, a
  round, a scenario, or a probe run. A run of one part has one row, named
  `run`.
  - `checkout` is `jab` when the part ran in the Jab checkout itself, and
    `synthetic` when it ran in a tree its runner laid out, such as a
    candidate copy, a mirror, or a scratch checkout.
  - `build` is the Jab commit the part ran on. It is given only for a
    part that ran in the Jab checkout, and only when the part's record
    names the commit; otherwise it is null.
  - `record` is the path, inside the run, of the original file that says
    where the part ran, usually its baseline. A part with no such file
    reads `unrecorded`.
- `kept` says why the run is kept. `as` is `current`, `last`,
  `checkpoint`, or `targeted`, by the retention rule below. `why` gives
  the reason in a line, and for a checkpoint it names the document that
  cites the run.

A run's kind is read from its own selection record where it has one: the
names it was told to run, against the version of the set that its
recorded hash names. A run with no selection record is read from its
logs, against the set file's version at the run's time in that file's
history.

## Assets

`assets/<subject>/<purpose>/` holds data that a workbench test or recipe
reads. The runners' own test cases read their canned logs from
`assets/workbench/cases/logs/`. The fps pose recipes read their pose
tables from `assets/fps/poses/`. The checkout runner reads the SDK's
mutant sets from `assets/sdk/mutants/`, one file a set. The workbench
finds this repository as a checkout beside its own, `../jab-stress-data`,
the way it finds Jab at `../jab`.

## Receipts

`migration/<stamp>-<name>.nuon` is the receipt of a move of data into or
out of this repository. It names the commits the data left, lists every
source file with its path, SHA-256, size in bytes, and what became of it,
and lists every file the move wrote with the same facts. A receipt starts
with `complete` false. It is marked complete only when every phase of the
move and every check of the data's consumers has finished. A receipt
never lists itself.

## The retention rule

Each set keeps:

- its current run, the latest run that took the whole set;
- its last run, the whole run before the current one;
- every run that a document still in use cites as evidence, as a
  checkpoint;
- the latest targeted run of any slice that no whole run covers, as
  targeted.

A targeted run never becomes a set's current or last run, so a later spot
check cannot displace whole-set evidence. A run kept for more than one
reason takes the first that applies, in the order current, last,
checkpoint, targeted. Everything else is pruned: a superseded run that no
document in use cites, and an attempt that a later run replaced. A run is
kept or pruned whole, never in part. A prune is a commit, so the
repository's history still holds the run.

## When a run lands

1. Copy the run's directory whole to `runs/<subject>/<set>/<stamp>/`, and
   check every copied file's SHA-256 against its source.
2. Write its `run.nuon`, with `kept.as` set by the rule.
3. Apply the rule to the run's set again. A new whole run becomes
   current, the current run becomes last, and the former last run is
   pruned unless it is cited or is still the latest run of a slice that
   no whole run covers.
4. Commit the run and any prune together.

The rule also runs again when a piece of work that cites a checkpoint is
finished, since its citation is then no longer in use.
