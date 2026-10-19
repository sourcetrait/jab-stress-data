# The data layout

This repository keeps the records of the test and measurement runs of Jab
and of its testing workbench, Jab Stress, and the data the workbench's
tests and recipes read. A run writes its files outside this repository
while it runs, and its record lands here once it is done.

## Runs

A run's record lives at `runs/<subject>/<set>/<stamp>/`.

- The subject is the part of Jab the run tested: `sdk`, `kernel`, or
  `fierce`, or `jab` for the whole repository, such as its fresh builds.
  The game's runs from before it moved to `kit/fierce` stay under `fps`,
  as they were recorded.
- The set is what the run took, named for the recipe, probe, or bench
  that ran it, such as `qualification`, `export`, or the probe's `sdl`.
- The stamp is the run's start in UTC as `YYYYMMDDTHHMMSS`. A record
  that kept only its date has the date, `YYYYMMDD`, and the run's label
  after a dash, such as `20261017-baselines`, so two runs of one date
  stay apart. Two runs that together took a set whole carry what each
  took as a label, such as `20261017T181257-1ka1`.

Every file a runner wrote is an original. A run that lands now lists each
original in its inventory with what became of it, its disposition (below):
a kept original is stored in the run, a text log in a container, its name
with `.nuonl` added (`serial.log.nuonl`), and any other file exactly as
its runner wrote it, byte for byte; an archived original is a member of
the run's archive; a derived or hash-only original is not stored, its
SHA-256 kept. Beside them sit the three files this repository adds:
`run.nuon`, the manifest; `inventory.nuon`, every original; and
`storage.nuon`, every file stored here. A run that landed before the
containers holds its runner's files byte for byte with `run.nuon` beside
them, until a migration (below) moves it.

An original is never edited. A run's manifest and its storage are revised
only by a migration, which a receipt records whole, so the run as it
stood is named by the commit the receipt gives.

A reread passes a run's saved logs through a reader again, with nothing
run again, when the reader changes in a way that reaches what the logs
say. It reads the run through a restoration (Reading a run, below) and
is a run of its own beside the run it reads, which stays as written. Its
stamp is the reread's start with the label `reread`, its items are those
whose logs it read, each reader revision is one of its parts, and its
`why` names the run it read.

## The manifest

`run.nuon` is one record. A run that lands now carries `schema: 2` and
the fields below. A run that landed before names no schema and is schema
1: it has the same `set`, `stamp`, `task`, `scope`, and `kept`, each of
its builds names `checkout` in place of `source`, `jab` for the Jab
checkout and `synthetic` for a tree its runner laid out, an export's
among them, and it has no inventory or storage.

- `schema` is 2.
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
  - `source` is `checkout` when the part ran in the Jab checkout itself,
    `export` when it ran an exported build, and `synthetic` when it ran
    in another tree its runner laid out, such as a candidate copy, a
    scratch checkout, or a fixture's own data. A real guest build that was
    exported is an export, never synthetic.
  - A part from an export names it in `export`: its `name`; its source
    `commit`; its `provenance`, the path of the export's provenance record,
    inside the run, or from this repository's root where the export's own
    run holds it; its build `symbols`; its `image_sha256`, the guest image
    it ran, or a kernel export's ELF; and `kernel_sha256`, the kernel ELF
    the part ran with, or null where it ran none or none was recorded. The
    provenance record holds the rest, the export's tools among it, and
    its commit, symbols, and image must agree with these.
  - `build` is the Jab commit the part ran on. It is given only for a
    part that ran in the Jab checkout, and only when the part's record
    names the commit; otherwise it is null.
  - `record` is the path, inside the run, of the original file that says
    where the part ran, usually its baseline. A part with no such file
    reads `unrecorded`, and nothing fills it in from a later revision.
- `kept` says why the run is kept. `as` is `current`, `last`,
  `checkpoint`, or `targeted`, by the retention rule below. `why` gives
  the reason in a line, and for a checkpoint it names the document that
  cites the run.
- `inventory` and `storage` are the SHA-256 of `inventory.nuon` and
  `storage.nuon`.

A run's kind is read from its own record: the items it took against the
items its set held at the run's time.

## The inventory and the storage

`inventory.nuon` is a table with a row for each original: its `path`
inside the run, its `role` by the landing's classes (below), its `length`
in bytes and its `sha256`, its `disposition`, and a `reason`.

`storage.nuon` is a table with a row for each file stored in the run: its
`object` path, its `kind`, which is `raw` for a file as its runner wrote
it, `container` for a wrapped log, or `archive`, its own `length` and
`sha256`, and `holds`, the paths of the originals it carries.

A container is a NUON lines file. Its first line is a header naming the
log's producer (`source`: `guest`, `qemu`, `shim`, or the run's set), its
`stream` (`stdout`, `stderr`, or `file`), and when it was `captured`. The
log's bytes follow in chunks of 64 KiB at most, each with its `seq` from
0, and its last line is a trailer with the chunk count, the log's length
and SHA-256, and whether its last byte is a newline. The bytes are never
split into lines or decoded, so the log comes back exactly as it was
written.

The archive, `archive.tar.xz`, one a run at most, is a tar compressed by
xz whose members are the archived originals, each once by its content and
named by its SHA-256, so byte-identical copies in a run are stored once.
The inventory maps each archived original's path to its SHA-256 and so to
its member, and the archive lists exactly those members.

The landing classes each file by its name:

| class | files | lands as |
|---|---|---|
| log | `.log`, `.stdout`, `.stderr`, `.txt`, `.nm`, `.disas` | a container |
| stream | `.out`, `.in`, `.wrpl`, `.wav` | written |
| record | `.nuon`, `.nuonl`, `.toml`, `flags`, a build's `home` | written |
| picture | `.ppm`, `.png` | written |
| build | `.jab`, `.elf`, `.romfs`, a flat `.bin`, an object `.o`, or any ELF | written |
| source | a copied `.nu`; an export's `.S`, `.inc`, `.ld`, and `.tar` | written |
| asset | a map tree's `.map`, `.pcm`, `.mid`, `.svg`, `map/name`, `map/title` | written |
| fixture | a staged `.jabstage.<nonce>` | written |

A `.json` file is a launch's control file, which its launch removes, so
one found refuses the landing. So does a file of a kind the table does
not list, so a new kind is classified on purpose. A pipe or a socket, a
plain launch's live wire, holds nothing at rest and is left out. A link
refuses the landing.

## Retention

What a run keeps of its originals is decided by what reads them. A
measurement nothing can regenerate is kept, since a later launch makes new
observations and does not regenerate a run's records; an input a reread
consumes is kept, compactly and losslessly, since a hash cannot give a
corrected reader its values. Nothing bulky enters this repository: a
large kept input goes into the run's archive. Each original takes one
disposition:

- `kept`: stored in the run as a raw object or a container.
- `archived`: a member of the run's archive.
- `derived`: made again from the kept originals by the reader its reason
  names, at that reader's revision; a reread writes a new output under its
  own revision, and the old hash identifies the old result.
- `hash-only`: no consumer reads it, or it is rebuilt from inputs whose
  digest is checked; its SHA-256 alone is kept, the reason saying which.

A derived or hash-only original gives its reason. An input left out with
no proved recovery leaves its reread unavailable, and the record of that
work says so.

An original outside this repository, a measurement's bulk in the box's
target or its scratch, is removed only once its removal gate passes: its
archive is committed here; a restoration into a fresh location gives back
every member at its SHA-256; and every consumer that reads it has run once
from that restoration, with the original and its absolute path
unavailable. Until then the original stays where it is.

## A launch's files

Every file a supervised launch leaves in its run's directory has its
disposition before a run of it lands:

- `launch.nuon`, the launch's record, the supervisor's exit receipt whole,
  every action's state and times, every reading, and the clock: an
  original timing observation, kept as a record.
- The schedule: its SHA-256 in `launch.nuon` stands for it, since the
  receipt carries every action; it does not land.
- The JSON wire files, the schedule, the readiness, the authorization, the
  cancel, the outcome, and the exit receipt: control files the launch
  removes on every path once its supervisor has ended; one a landing finds
  refuses it by name.
- `qemu.stdout` and `qemu.stderr`, the child's streams: logs, landed as
  containers where a consumer reads them.

## Assets

`assets/<subject>/<purpose>/` holds data that a workbench test or recipe
reads. The fierce pose recipes read their pose tables from
`assets/fierce/poses/`. The import-map checker reads the migration contract
of Jab's nushell trees from `assets/jab/importmap/contract.nuon`. The
workbench finds this repository as a checkout beside its own,
`../jab-stress-data`, the way it finds Jab at `../jab`.

## Receipts

`migration/<stamp>-<name>.nuon` is the receipt of a move of data into or
out of this repository, or of a run from one representation to another.
It names the commits the data left, lists every source file with its
path, SHA-256, size in bytes, and what became of it, and lists every file
the move wrote with the same facts. A receipt starts with `complete`
false. It is marked complete only when every phase of the move and every
check of the data's consumers has finished; a receipt written once at a
move's end, as `runner migrate` writes it, is written complete. A receipt
never lists itself.

Every SHA-256 and size in a receipt is read from the file's raw bytes. A
committed file's size comes from `git cat-file -s <commit>:<path>` and its
SHA-256 from `git cat-file blob <commit>:<path> | into binary | hash
sha256`, or from the file in a checkout whose path git holds unchanged. A
value that a command printed as text is never measured or hashed, because
binding a command's output as text drops its last newline.

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

1. From the workbench's checkout, run `nu nu/bin/nuance.nu runner land
   <dir> <subject> <set> <stamp> <manifest>`. `<dir>` is the run's
   directory as its runner left it, and `<manifest>` is a NUON file of
   the record's own fields, `task`, `scope`, `builds`, and `kept`, with
   `kept.as` set by the rule, and `retention`, the rules that dispose of
   the originals: each a `match`, a regular expression over the
   run-relative path, a `disposition`, and a `reason`, the first that
   matches deciding and an original no rule matches kept. The landing
   writes the run to `runs/<subject>/<set>/<stamp>/`, wrapping each kept
   log, copying each other kept file, archiving the archived, checks every
   file against its source's SHA-256, writes the inventory, the storage,
   and `run.nuon`, and holds the whole run to its record. A landing that
   fails leaves nothing behind.
2. Apply the rule to the run's set again. A new whole run becomes
   current, the current run becomes last, and the former last run is
   pruned unless it is cited or is still the latest run of a slice that
   no whole run covers.
3. Commit the run and any prune together.

The rule also runs again when a piece of work that cites a checkpoint is
finished, since its citation is then no longer in use.

## When a run migrates

A run that landed before the containers moves to schema 2 by `nu
nu/bin/nuance.nu runner migrate <run> <manifest> <scratch>`. Its manifest
holds the new record's fields, `retention`, `migration`, the receipt's
name, and `with`, the trees outside this repository holding more of the
run's originals, each `{from, to}` laid at `to` in the run. The run's
committed files stay as committed unless a rule disposes of them; a
further original at a path the run holds must equal it. The new run is
held to its record and restored whole before it replaces the old, and the
receipt is written; the commit carries the run and its receipt together.

## Reading a run

A run is read through a restoration, never in place. From the
workbench's checkout, `nu nu/bin/nuance.nu runner restore <run>
<scratch>` lays the run's files out under `<scratch>/tree/` exactly as
its runner wrote them, its archive's members among them, each checked
against the inventory, with the receipt `restoration.nuon` beside them. A
derived or hash-only original is listed absent. A run that landed before
the containers restores as a copy of its files. `nu nu/bin/nuance.nu
runner log text <container>` gives one log back as text.
