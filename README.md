# McLocalizer Experiment

This repository contains the files used during the McLocalizer experiment, which have been made public for the sake of transparency.

The goal of the experiment is to evaluate quality of **fixing commits** and **bug scopes** identified by the McLocalizer in context of open-source java projects. <br>
**Fixing commit** is a commit that removes the bug. <br>
**Bug scope** is a set of classes that must be changed to remove the bug. <br>

## Experiment objects

* [McLocalizer v1.0.0-alpha](https://github.com/jkbkasprzak/mclocalizer/releases/tag/v1.0.0-alpha) <br>
Evaluated software, used to identify fixing commits with bug scopes.

* [Defects4J v2.0.1](https://github.com/rjust/defects4j/releases/tag/v2.0.1) <br>
Collection of reproducible bugs, contains manually identified fixing commits with bug scopes for some open source java projects.
Used to generate control set.

* [Closure Compiler v20240317](https://github.com/google/closure-compiler/releases/tag/v20240317) <br>
Open source java project used as a subject for McLocalizer analysis.

## Indicators

* Fixing Commit Precision (FCP) - How many retrieved fixing commits are indeed fixing commits?
$$\mathrm{FCP} = \frac{\mathrm{tp}}{\mathrm{tp}+\mathrm{fp}}$$
* Fixing Commit Recall (FCR) - How many fixing commits are retrieved?
$$\mathrm{FCR} = \frac{\mathrm{tp}}{\mathrm{tp}+\mathrm{fn}}$$
* Average Bug Scope Precision (ABSP) - How many classes in bug scope are relevant?
$$\mathrm{ABSP} = \frac{\mathrm{tp}}{\mathrm{tp}+\mathrm{fp}}$$
* Average Bug Scope Recall (ABSR) - How many classes that should be in bug scope are retrieved?
$$\mathrm{ABSR} = \frac{\mathrm{tp}}{\mathrm{tp}+\mathrm{fn}}$$


## Experiment procedure

1. Collect McLocalizer results for Closure Compiler repository `closure-mclocalizer.csv` - list of fixing commits with bug scopes.
```sh
mclocalizer ./closure-compiler -t java_class -o closure-mclocalizer.csv
```
2. Collect Defects4J results for Closure Compiler repository `closure-defects4j.csv` - list of fixing commits with bug scopes.
```sh
defects4j query -A -p Closure -q "bug.id,revision.id.fixed,classes.modified" > closure-defects4j.csv
```
3. Choose random sample of fixing commits from `closure-mclocalizer.csv` and manually verify if the commit is indeed a **fixing commit** and what top level java classes are modified, excluding test classes. Save results to `closure-manual.csv`.
4. Compare `closure-mclocalizer.csv` with `closure-defects4j.csv` as a control set.
    1. Calculate FCR, ABSP, ABSR
    2. Save results to `cmp-mclocalizer-defects4j.csv`
5. Compare `closure-mclocalizer.csv` with `closure-manual.csv` as a control set.
    1. Calculate FCP, ABSP, ABSR
    2. Save results to `cmp-mclocalizer-manual.csv`

## File structure
TODO

## Results
### Compare McLocalizer with Defects4J
|McLocalizer fixing commits|Defects4J fixing commits|Common commits|FCR|ABSP|ABSR|
|---:|---:|---:|---:|---:|---:|
|2540|176|176|1.000|0.866|1.000|

### Compare McLocalizer with manually verified commits
|McLocalizer fixing commits|Manually verified commits|Confirmed fixing commits|Confirmed pure fixing commits|Confirmed fixing commits with tracker|FCP|ABSP|ABSR|
|---:|---:|---:|---:|---:|---:|---:|---:|
|2540|100|94|73|25|0.940|1.000|1.000|
