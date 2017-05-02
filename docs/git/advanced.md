# GIT-Advanced

## Repository anlegen

```bash
$ git init git-workshop
Initialisierte leeres Git-Repository in /home/jen/git-workshop/.git/

$ cd git-workshop
$ git status
Auf Branch master

Initialer Commit

nichts zu committen (Erstellen/Kopieren Sie Dateien und benutzen Sie "git add" zum Versionieren)
```

* initialisiert leeres GIT-Repository mit Arbeitsbereich
* wird kein Repository-Name angegeben, wird das Repository im aktuellen Verzeichnis initialisiert

```bash
$ git init --bare git-workshop
Initialisierte leeres Git-Repository in /srv/jen/git-workshop/

$ cd git-workshop
$ git status
fatal: This operation must be run in a work tree
```

* initialisiert leeres GIT-Repository ohne Arbeitsbereich

## Commit erstellen

### `git commit --amend` fügt letztem Commit neue Änderungen hinzu

```bash
$ git add docs

$ git status
Auf Branch master
Ihr Branch ist vor 'origin/master' um 1 Commit.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        neue Datei:     docs/mkdocs.md

$ git commit --amend
[master bfa931f] * moved brief mkdocs docu to separate page * modified index page
 Date: Wed Apr 5 13:13:20 2017 +0200
 3 files changed, 6 insertions(+), 18 deletions(-)
 rewrite docs/index.md (99%)
 copy docs/{index.md => mkdocs.md} (100%)

$ git status
Auf Branch master
Ihr Branch ist vor 'origin/master' um 1 Commit.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
nothing to commit, working tree clean

```

## Branches

### `git fetch [origin]` holt veröffentlichte Änderungen vom Remote-Server

```bash
$ git fetch
remote: Zähle Objekte: 5, Fertig.
remote: Komprimiere Objekte: 100% (4/4), Fertig.
remote: Total 5 (delta 0), reused 0 (delta 0)
Entpacke Objekte: 100% (5/5), Fertig.
Von git@git.ub.intern.uni-leipzig.de:git-workshop
   5e58653..bfa931f  master     -> origin/master
```

* Änderungen werden nicht auf lokale Branches angewendet, sondern zwischengespeichert

### `git merge master` vereint die Änderungen von *master* mit dem aktuellen Branch

```bash
$ git merge master
Already up-to-date.
```

* keine Änderungen im lokalen Master

```bash
]$ git checkout master
Zu Branch 'master' gewechselt
Ihr Branch ist zu 'origin/master' um 1 Commit hinterher, und kann vorgespult werden.
  (benutzen Sie "git pull", um Ihren lokalen Branch zu aktualisieren)
```

* Tracking-Konfiguration sorgt für die Information, dass der lokale *master*-Branch
 dem Branch auf dem Remote-Servers hinterher ist.

### `git merge` vereint die Änderungen

```bash
$ git merge
Aktualisiere 5e58653..bfa931f
Fast-forward
 docs/index.md  | 18 +++---------------
 docs/mkdocs.md | 17 +++++++++++++++++
 mkdocs.yml     |  2 +-
 3 files changed, 21 insertions(+), 16 deletions(-)
 create mode 100644 docs/mkdocs.md

$ git log
commit bfa931fdb94fd844820263e2bb5dec8e02433673
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed Apr 5 13:13:20 2017 +0200

    * moved brief mkdocs docu to separate page
    * modified index page

commit 5e586536cd4267fbf02a5c9e2ef8397b4e86294f
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* aktualisiert den lokalen *master*-Branch mit auf Remote-Server *origin*
 veröffentlichten Änderungen für *master*-Branch

### `git merge master` (again) aktualisiert den aktuellen Branch

```bash
$ git merge master
Merge made by the 'recursive' strategy.
 docs/index.md  | 18 +++---------------
 docs/mkdocs.md | 17 +++++++++++++++++
 mkdocs.yml     |  2 +-
 3 files changed, 21 insertions(+), 16 deletions(-)
 create mode 100644 docs/mkdocs.md

$ git log
commit 75a7b05d54c67c4aebf7f7129b0c1e67325ff808
Merge: edcb49e 117e2c4
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 18:15:27 2017 +0200

    Merge branch 'master' into create-git-workshop-docs

commit 117e2c4265322d8fcca898de9bea9749666ebcb4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Mon Apr 10 17:56:00 2017 +0200

    * moved brief mkdocs docu to separate page
    * modified index page

commit edcb49e0bcb60c406fed61c1002fdafd16e559e8
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 16:53:41 2017 +0200

    added git-workshop-documentation

commit 5e586536cd4267fbf02a5c9e2ef8397b4e86294f
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* erstellt einen Merge-Commit, der die Änderungen des lokalen *master*-Branch
 mit den Änderungen auf dem aktuellen Branch zusammenführt

```bash
2017-04-10 18:15 Maurice Moss  M─┐ [create-git-workshop-docs] Merge branch 'master' into create-git-workshop-docs
2017-04-10 17:56 Roy Trenneman │ o [master] {origin/master} * moved brief mkdocs docu to separate page * modified index
2017-04-10 16:53 Maurice Moss  o │ {origin/create-git-workshop-docs} added git-workshop-documentation
2017-04-05 10:49 Jen Barber    I─┘ created mkdocs structure
```

#### `git merge <branch>` bringt Änderungen auf den Master

```bash
$ git merge create-git-workshop-docs
Aktualisiere 117e2c4..26171f5
Fast-forward
 docs/git.md | 464 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 464 insertions(+)
 create mode 100644 docs/git.md


$ git log
commit 26171f5ee64aa72b16ac643e8a2b64654fefa7fa
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 18:28:19 2017 +0200

    more into git documentation

commit 45e97f7aa2244066b2d018397d4ea29da6d5d6c3
Merge: 26796c0 117e2c4
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 18:56:24 2017 +0200

    Merge remote-tracking branch 'origin/master' into create-git-workshop-docs

commit 26796c04f33a4ecad877c6977afac3179dbe6a41
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 16:53:41 2017 +0200

    added git-workshop-documentation

commit 117e2c4265322d8fcca898de9bea9749666ebcb4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Mon Apr 10 17:56:00 2017 +0200

    * moved brief mkdocs docu to separate page
    * modified index page

commit 5e586536cd4267fbf02a5c9e2ef8397b4e86294f
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* holt alle Commits eines Branches in den aktuellen Branch
* verunreinigt GIT-log

```
2017-04-10 18:28 Maurice Moss  o [master] [create-git-workshop-docs] more into git documentation
2017-04-10 18:56 Maurice Moss  M─┐ Merge remote-tracking branch 'origin/master' into create-git-workshop-docs
2017-04-10 17:56 Roy Trenneman │ o {origin/master} * moved brief mkdocs docu to separate page * modified index page
2017-04-10 16:53 Maurice Moss  o │ added git-workshop-documentation
2017-04-05 10:49 Jen Barber    I─┘ created mkdocs structure
```

#### `git branch -d <local-branch-name>` löscht den Branch



### Branches (Alternative Strategie)

#### `git rebase master` setzt die Commits des aktuellen Branchs auf den vorgespulten *master*

```bash
$ git rebase master
Zunächst wird der Branch zurückgespult, um Ihre Änderungen
darauf neu anzuwenden ...
Wende an: added git-workshop-documentation

$ git log
commit 58cf8023e4e1d41980bca3aefea8eb4148a1298e
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 16:53:41 2017 +0200

    added git-workshop-documentation

commit 117e2c4265322d8fcca898de9bea9749666ebcb4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Mon Apr 10 17:56:00 2017 +0200

    * moved brief mkdocs docu to separate page
    * modified index page

commit 5e586536cd4267fbf02a5c9e2ef8397b4e86294f
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* ignoriert chronologische Reihenfolge der Commits
* hält Verzweigung sauber
* sollte Standardverhalten sein, eigenen Feature-Branch aktuell zu halten

```bash
2017-04-10 16:53 Maurice Moss  o [create-git-workshop-docs] added git-workshop-documentation
2017-04-10 17:56 Roy Trenneman o [master] {origin/master} * moved brief mkdocs docu to separate page * modified index p
2017-04-05 10:49 Jen Barber    I created mkdocs structure
```

#### `git merge --squash <branch>` holt Änderungen eines Branches in den aktuellen Branch

```bash
$ git merge --squash create-git-workshop-docs
Aktualisiere 117e2c4..550cc4a
Fast-forward
Quetsche Commit -- HEAD wird nicht aktualisiert
 docs/git.md | 464 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 464 insertions(+)
 create mode 100644 docs/git.md

$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        neue Datei:     docs/git.md

$ git commit
[master 242d2af] merged git-workflow documentation
 1 file changed, 464 insertions(+)
 create mode 100644 docs/git.md

$ git log
commit beafe7411a4082f5297d073f4cec983d5899a52f
Author: Maurice Moss <maurice@reynholm-industries.co.uk>
Date:   Mon Apr 10 18:49:58 2017 +0200

    merged git-workflow documentation

commit 117e2c4265322d8fcca898de9bea9749666ebcb4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Mon Apr 10 17:56:00 2017 +0200

    * moved brief mkdocs docu to separate page
    * modified index page

commit 5e586536cd4267fbf02a5c9e2ef8397b4e86294f
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* holt Änderungen, die sich aus allen Commits des angegebenen Branches ergeben
 in die Staging-Area
* `commit` erstellt einen neuen Commit auf dem Master, ohne jegliche Referenz zu den ursprünglichen Commits

```bash
2017-04-10 18:49 Maurice Moss  o [master] merged git-workflow documentation
2017-04-10 17:56 Roy Trenneman o {origin/master} * moved brief mkdocs docu to separate page * modified index page
2017-04-05 10:49 Jen Barber    I created mkdocs structure
```

### Ignorierte Dateien

#### `git add -f [file|folder]` fügt ignorierte Dateien zur Versionskontrolle hinzu

```bash
$ git add .vscode/tasks.json
Die folgenden Pfade werden durch eine Ihrer ".gitignore" Dateien ignoriert:
.vscode/tasks.json
Verwenden Sie -f wenn Sie diese wirklich hinzufügen möchten.

$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        neue Datei:     .vscode/tasks.json
```


### `git reset [commit]` macht alle Commits bis zum angegebenen rückgängig

```bash
$ git reset HEAD^
Nicht zum Commit vorgemerkte Änderungen nach Zurücksetzung:
M       docs/git.md

$ git status
Auf Branch master
Ihr Branch ist zu 'origin/master' um 1 Commit hinterher, und kann vorgespult werden.
  (benutzen Sie "git pull", um Ihren lokalen Branch zu aktualisieren)
Änderungen, die nicht zum Commit vorgemerkt sind:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)
  (benutzen Sie "git checkout -- <Datei>...", um die Änderungen im Arbeitsverzeichnis zu verwerfen)

        geändert:       docs/git.md

keine Änderungen zum Commit vorgemerkt (benutzen Sie "git add" und/oder "git commit -a")
```

* relative Angaben sind möglich *HEAD^* bedeutet *letzter Commit*
* ermöglicht das schrittweise Zurückgehen der Commits, ohne die Änderungen zu verlieren

### `git reset --hard [commit]` macht alle Commits bis zum angegebenen rückgängig und verwirft alle Änderungen

```bash
$ git reset --hard HEAD^
HEAD ist jetzt bei 9288761 added vufind-docs, modified mkdocs, modified settings

$ git status
Auf Branch master
Ihr Branch ist zu 'origin/master' um 1 Commit hinterher, und kann vorgespult werden.
  (benutzen Sie "git pull", um Ihren lokalen Branch zu aktualisieren)
nothing to commit, working tree clean

$ git pull
Aktualisiere 9288761..b822b5e
Fast-forward
 docs/git.md | 23 ++++++++++++-----------
 1 file changed, 12 insertions(+), 11 deletions(-)
```

* hilft bei Fehlersuche, z.B. ab wann ein bestimmter Fehler auftritt
* ermöglicht irreversiblen Eingriff in die Projekt-Historie
* Änderung bereits veröffentlichter Commits sollte vermieden werden
* Veröffentlichung einer geänderten Historie ist standardmäßig nicht erlaubt

## Erweiterte Kenntnisse

```ascii
+       master      <--+
|                      |
+-+     .git           |
  +-+                  |                                  +----------------------------------+
    +-+ origin/master -+                                  |                                  |
    |                                                     | Remote (origin)                  |
    +-+ origin/create->ufind-workshop-docs                |  +                               |
    |                                                     |  |                               |
    +-+ remotes/origin/master           <---------------> |  +-+ master                      |
    |                                                     |  |                               |
    +-+ remotes/origin/create->ufind-workshop-docs <----> |  +-+ create->ufind-workshop-docs |
    |                                                     |  |                               |
    +-+ remotes/origin/create-git-workshop-docs <-------> |  +-+ create-git-workshop-docs    |
                                                          |                                  |
                                                          +----------------------------------+
```
