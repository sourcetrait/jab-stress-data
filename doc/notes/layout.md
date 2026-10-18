# The data layout

This repository keeps the records of the test and measurement runs of Jab
and of its testing workbench, Jab Stress, and the data the workbench's
tests and recipes read. A run writes its files outside this repository
while it runs, and its record is copied in whole once it is done.

## Runs

A run's record lives at `runs/<subject>/<set>/<stamp>/`.

- The subject is the part of Jab the run tested: `sdk`, `kernel`, or
  `fps`.
- The set is what the run took, named for the recipe, probe, or bench
  that ran it, such as `qualification`, `export`, or the probe's `sdl`.
- The stamp is the run's start in UTC as `YYYYMMDDTHHMMSS`. A record
  that kept only its date has the date, `YYYYMMDD`, and the run's label
  after a dash, such as `20261017-baselines`, so two runs of one date
  stay apart. Two runs that together took a set whole carry what each
  took as a label, such as `20261017T181257-1ka1`.

The directory holds the run's own files exactly as its runner wrote
them: its logs, results, baselines, and receipts, byte for byte. Nothing
in them is edited after the copy. Beside them sits `run.nuon`, the
manifest this repository adds.

A reread passes a run's saved logs through a reader again, with nothing
run again, when the reader changes in a way that reaches what the logs
say. It is a run of its own beside the run it reads, which stays as
written. Its stamp is the reread's start with the label `reread`, its
items are those whose logs it read, each reader revision is one of its
parts, and its `why` names the run it read.

## The manifest

`run.nuon` is one record with six fields.

- `set` and `stamp` are the run's set and stamp, as its path gives them.
- `task` is the name of the piece of work the run was made for, such as
  `IncludeKeyedBuild`.
- `scope` says how much of its set the run took.
  - `kind` is `whole` when the run took every item its set held at the
    time, and `targeted` when it took only some.
  - `ran` counts the items the run took, and `of` the items its set held
    at the time. A probe's or a bench's items are its runs, an export's
    its images, and the level comparison's its views.
  - `names` lists the items a targeted run took.
  - `combined` appears on each of two runs that together took their set
    whole. It names both runs and gives their joint `kind`, `ran`, and
    `of`.
- `builds` has one row for each part of the run, such as a shard, a
  round, a scenario, or a probe run. A run of one part has one row, named
  `run`.
  - `checkout` is `jab` when the part ran in the Jab checkout itself, and
    `synthetic` when it ran in a tree its runner laid out, such as an
    export's tree, a candidate copy, or a scratch checkout.
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

A run's kind is read from its own record: the items it took against the
items its set held at the run's time.

## Assets

`assets/<subject>/<purpose>/` holds data that a workbench test or recipe
reads. The fps pose recipes read their pose tables from
`assets/fps/poses/`. The workbench finds this repository as a checkout
beside its own, `../jab-stress-data`, the way it finds Jab at `../jab`.

## Receipts

`migration/<stamp>-<name>.nuon` is the receipt of a move of data into or
out of this repository. It names the commits the data left, lists every
source file with its path, SHA-256, size in bytes, and what became of it,
and lists every file the move wrote with the same facts. A receipt starts
with `complete` false. It is marked complete only when every phase of the
move and every check of the data's consumers has finished. A receipt
never lists itself.

Every SHA-256 and size in a receipt is read from the file's raw bytes. A
committed file's size comes from `git cat-file -s <commit>:<path>` and its
SHA-256 from `git cat-file blob <commit>:<path> | into binary | hash
sha256`. A value that a command printed as text is never measured or
hashed, because binding a command's output as text drops its last
newline.

A receipt is never edited once it is written. A mistake in one is
corrected by a new receipt, `migration/<stamp>-receiptcorrection.nuon`,
which names the receipt it corrects by path and SHA-256 and gives each
value it changes, both as recorded and as corrected.

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
