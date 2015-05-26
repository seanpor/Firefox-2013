Firefox-2013
============

A restricted dataset of bugs in Firefox extracted from the academic release of the bugzilla dataset.  This dataset is intended for the use of software reliability researchers and has some interesting features including:
* 23 version labels (PreRapid, 5, 6,... 25, Unknown) for bugs from Firefox releases.
* 1,959 labeled bugs and 8,461 unlabeled bugs - without a version label.

The columns in the dataset are:
* Version: The version number of Firefox associated with this bug, e.g. 14, or `PreRapid' to describe any version prior to 5, or `Unknown' for any bug which did not have a version number associated with it.
* bug_id: The original bug number in the Bugzilla database. Further details on this bug can be queried at https://bugzilla.mozilla.org/
* bug_severity: One of: blocker, critical, major, normal, minor, trivial. Note that these labels have an order.
* bug_status: One of: ASSIGNED, NEW, REOPENED, RESOLVED, VERIFIED.
* priority: One of: `--', P1, P2, P3, P4, P5. Where P1 has the highest priority and `--' means that no priority has been assigned.
* creation_ts: A character string POSIX timestamp of when the bug was first inserted into the bugzilla database, e.g. "2011-04-13 17:19:05".
* reporter: The ID number of the person reporting the bug from the underlying bugzilla database - for privacy reasons instead of needlessly using email addresses.
* component_id: The ID number of the component from the Bugzilla database. There are 45 components used in Firefox, some of which are shared with other Mozilla Foundation products like Thunderbird.

In R, I might read this in using:
```
m2 <- read.csv("Firefox-2013-FINAL.csv", colClasses='character')
versionLevels <- c("PreRapid", "5", "6", "7", "8", "9", "10",
                   "11", "12", "13", "14", "15",
                   "16", "17", "18", "19", "20",
                   "21", "22", "23", "24", "25", "Unknown")
m2$Version <- ordered(m2$Version, levels=versionLevels)
m2$bug_severity <- ordered(m2$bug_severity, levels=c('blocker', 'critical', 'major', 'normal', 'minor', 'trivial'))
m2$bug_status <- factor(m2$bug_status)
m2$priority <- factor(m2$bug_status , levels=c('--', 'P1', 'P2', 'P3', 'P4', 'P5'))
m2$creation_ts <- as.POSIXct(m2$creation_ts)
m2$component_id <- factor(m2$component_id)
str(m2)
summary(m2)
```

The file `FirefoxVersionInfo.csv` gives information on the versions of Firefox in this dataset - namely the Rapid-Releases from 5 to 25.
The columns in this file are:

* Version: The release version number, e.g. 5.
* Release: The release version as text, e.g. `release-15.0'.
* FChanged: The number of files changed since the previous version.
* LInserts: The number of lines added since the previous version.
* LDeletions: The number of lines deleted since the previous version.
* releasedate: The release date for this version.
