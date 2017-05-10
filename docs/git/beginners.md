# GIT-Beginners

## Vorbereitung

### GIT installieren

Ein aktuelles Git sollte auf jeder größeren Distribution vorliegen, bzw. kann
für Windows/Mac unter [https://git-scm.com/downloads](https://git-scm.com/downloads) heruntergeladen werden.

```bash
# Debian/Ubuntu
$ apt-get install git
# Fedora
$ yum install git # (up to Fedora 21)
$ dnf install git # (Fedora 22 and later)
# Gentoo
$ emerge --ask --verbose dev-vcs/git
# Arch Linux
$ pacman -S git
# openSUSE
$ zypper install git
# Mageia
$ urpmi git
# FreeBSD
$ pkg install git
# Solaris 9/10/11 (OpenCSW)
$ pkgutil -i git
# Solaris 11 Express
$ pkg install developer/versioning/git
# OpenBSD
$ pkg_add git
# Alpine
$ apk add git
```

### Anpassen der Benutzerinformationen

```bash
$ git config --global user.name "Roy Trenneman"
$ git config --global user.email "roy@reynholm-industries.co.uk"
$ git config --global --list
user.name=Roy Trenneman
user.email=roy@reynholm-industries.co.uk
```

* `--global` ist Benutzer-Kontext, Einstellungen werden in `$HOME/.gitconfig` gespeichert
* `--local` ist Repository-Kontext, Einstellungen werden in `<repository-folder>/.git/config` gespeichert
* `--system` ist Host-Kontext, Einstellungen werden in `/etc/gitconfig` gespeichert
* Benutzer-Kontext überschreibt Host-Kontext, Repository-Kontext überschreibt Benutzer-Kontext

### Anpassen des Push-Verhaltens

```bash
$ git config --global push.default simple
```

* legt explizit das neue Standard-Verhalten von GIT bei Commit-Veröffentlichung fest.
 Andernfalls wird beim ersten Push-Versuch eine Warnung ausgegeben.

## Repository klonen

```bash
$ git clone workshop@git.ub.intern.uni-leipzig.de:git-workshop
Klone nach 'git-workshop' ...
Fertig.
```

* GIT-Repositorys werden geklont (kopiert)
* sämtliche Repository-Aktionen werden lokal durchgeführt
* das Ergebnis ist ein konsistentes Repository, welches primitiv auf einem oder mehreren Remote-Servern abgelegt werden kann

```bash
$ cd git-workshop/
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
nothing to commit, working tree clean
```

* die lokale Kopie des Repositorys ist identisch mit der Version des Servers
* der Standard-Branch ist *master*

```bash
$ git remote -v
origin  workshop@git.ub.intern.uni-leipzig.de:git-workshop (fetch)
origin  workshop@git.ub.intern.uni-leipzig.de:git-workshop (push)
```

* `git clone` fügt automatisch den Server, von dem das Repository geklont wurde, als *origin*-Remote hinzu

## Dateien umbenennen mit `git mv <src> <dst>`

```bash
$ git mv docs/index.md docs/mkdocs.md
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        umbenannt:      docs/index.md -> docs/mkdocs.md

```

* git erkennt, dass eine Datei umbenannt wurde, in dem der Inhalt der Dateien verglichen wird
* werden zwischen Umbenennen und Hinzufügen zur Versionskontrolle Änderungen am Inhalt der Datei vorgenommen wird die Datei nicht als *umbenannt* erkannt

## Code ändern / Dateien hinzufügen

***`mkdocs.yml` ändern, `docs/index.md` hinzufügen, siehe `patches/code_ändern_dateien_hinzufügen.patch`***

### `git status` zeigt geänderte Dateien

```bash
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        umbenannt:      docs/index.md -> docs/mkdocs.md

Änderungen, die nicht zum Commit vorgemerkt sind:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)
  (benutzen Sie "git checkout -- <Datei>...", um die Änderungen im Arbeitsverzeichnis zu verwerfen)

        geändert:       mkdocs.yml

Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        docs/index.md

```

### `git diff mkdocs.yml` zeigt Änderungen einer Datei

```bash
$ git diff mkdocs.yml
diff --git a/mkdocs.yml b/mkdocs.yml
index c97182f..3212117 100644
--- a/mkdocs.yml
+++ b/mkdocs.yml
@@ -1 +1 @@
-site_name: My Docs
+site_name: Workshop Dokumentationen
```

* wird kein Dateiname übergeben, werden alle Änderungen in Dateien angezeigt, die bereits der Versionskontrolle unterliegen und nicht zur Staging-Area hinzugefügt wurden

### `git add <filename|dirname> [...]` fügt geänderte Dateien zur *Staging-Area* hinzu

```bash
$ git add docs mkdocs.yml
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        geändert:       docs/index.md
        kopiert:        docs/index.md -> docs/mkdocs.md
        geändert:       mkdocs.yml

```

### *Staging-Area* beinhaltet alle zum *commit* vorgemerkten Änderungen

```bash
$ git diff --staged
diff --git a/docs/index.md b/docs/index.md
index da37213..80ccc76 100644
--- a/docs/index.md
+++ b/docs/index.md
@@ -1,17 +1,3 @@
-# Welcome to MkDocs
+# Workshop Dokumentationen

-For full documentation visit [mkdocs.org](http://mkdocs.org).
-
-## Commands
-
-* `mkdocs new [dir-name]` - Create a new project.
-* `mkdocs serve` - Start the live-reloading docs server.
-* `mkdocs build` - Build the documentation site.
...
```

* zeigt neue Dateien an, wenn zur Staging-Area hinzugefügt

## Commit erstellen

### `git commit` erstellt einen Commit mit allen vorgemerkten Änderungen aus der *Staging-Area*

```bash
$ git commit
[master 66f8437] * moved brief mkdocs docu to separate page * created new index page * modified title
 3 files changed, 4 insertions(+), 18 deletions(-)
 rewrite docs/index.md (99%)
 copy docs/{index.md => mkdocs.md} (100%)
```

* leere Commit-Beschreibung bricht Commit ab
* Änderungen wurden als ein Commit in das **lokale** Repository gestellt

lokales Repository weicht um einen Commit vom Remote-Server ab

```bash
$ git status
Auf Branch master
Ihr Branch ist vor 'origin/master' um 1 Commit.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
nothing to commit, working tree clean

```

## Die Historie

Die Entwicklung des Projektes veranschaulicht die Historie der Versionskontrolle.
Sie hilft, den Ursprung von Implementationen, Autoren und Bemerkungen anzuzeigen
und nachzuvollziehen.

### `git log` zeigt gesamte Historie eines Branches

```bash
$ git log
commit 66f8437bd5b8d26d5e65ee1af25a82b52dcf82a5
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed Apr 26 17:02:45 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 27c4a4334ac4c55bb5585729fcd64a4eeb889743
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* zeigt neben eineindeutigen Commit-Hash, Commit-Autor und Commit-Datum, zusätzlich
 die Commit-Bemerkung
* Sortierung erfolgt nach Anwendung des Commits und muss nicht dem Commit-Datum
 entsprechen

```bash
$ git log --follow mkdocs.yml
commit 66f8437bd5b8d26d5e65ee1af25a82b52dcf82a5
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed Apr 26 17:02:45 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 27c4a4334ac4c55bb5585729fcd64a4eeb889743
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* zeigt nur die Commit-Historie, welche die angegebene Datei betrifft, auch, wenn sie nicht mehr in der Versionkontrolle vorhanden ist
* die Datei muss im Arbeitsbereich vorhanden sein

### `git push` veröffentlicht den neuen Commit auf Remote-Server

```bash
$ git push
Zähle Objekte: 5, Fertig.
Delta compression using up to 8 threads.
Komprimiere Objekte: 100% (4/4), Fertig.
Schreibe Objekte: 100% (5/5), 564 bytes | 0 bytes/s, Fertig.
Total 5 (delta 0), reused 0 (delta 0)
To workshop@git.ub.intern.uni-leipzig.de:git-workshop
   f5c2a5c..8c298ce  master -> master

$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
nothing to commit, working tree clean
```

## Branches erstellen

### `git branch <branch-name>` erstellt neuen Branch, ausgehend von Branch *master*

```bash
$ git branch create-git-beginners-docs
$ git checkout create-git-beginners-docs
Zu Branch 'create-git-beginners-docs' gewechselt
```

* Entwicklung erfolgt in Branches, um sich nicht gegenseitig zu behindern
* shortcut wäre `git checkout -b create-git-beginners-docs`
* Änderungen werden beim Branch-Wechseln zum neuen Branch mitgenommen, sofern möglich

***`docs/git-beginners.md` erstellen, `docs/index.md` ändern, siehe `patches/branches_erstellen.patch`***

### `git add` und `git commit` zum Speichern der Änderungen

```bash
$ git add docs/
$ git commit
[create-git-beginners-docs f6906eb] * initially added git-beginners documentation * modified index page
 2 files changed, 2 insertions(+)
 create mode 100644 docs/git-beginners.md

$ git status
Auf Branch create-git-beginners-docs
nothing to commit, working tree clean

$ git log
commit f6906eb41fb9f874d60b34cb304e6435e649b271
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed Apr 26 17:13:26 2017 +0200

    * initially added git-beginners documentation
    * modified index page

commit 66f8437bd5b8d26d5e65ee1af25a82b52dcf82a5
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed Apr 26 17:02:45 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 27c4a4334ac4c55bb5585729fcd64a4eeb889743
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

### `git push --set-upstream origin <local-branch-name>` veröffentlicht den Branch

```bash
$ git push --set-upstream origin create-git-beginners-docs
Zähle Objekte: 5, Fertig.
Delta compression using up to 8 threads.
Komprimiere Objekte: 100% (4/4), Fertig.
Schreibe Objekte: 100% (5/5), 560 bytes | 0 bytes/s, Fertig.
Total 5 (delta 0), reused 0 (delta 0)
To git.ub.intern.uni-leipzig.de:git-workshop
 * [new branch]      create-git-beginners-docs -> create-git-beginners-docs
Branch create-git-beginners-docs konfiguriert zum Folgen von Remote-Branch create-git-beginners-docs von origin.
```

* neuer Branch wird veröffentlicht und zum Folgen des gleichen konfiguriert
* durch `--set-upstream` wird der Remote-Server und Branch als Referenz eingetragen, um `git pull`, `git push`, `git status` ohne Argumente aufzurufen

## Branches aktuell halten (auf Branch *create-vufind-docker-docs*)

```bash
$ git checkout create-vufind-docker-docs

$ git pull
Already up-to-date.
```

* ohne Angabe von Remote-Server und Branch wird der aktuelle Branch mit Änderungen des referenzierten Branchs aktualisiert

***`docs/vufind-docker.md` ändern, siehe `patches/branch_aktuell_halten.patch`, commit erstellen***

```bash
$ git pull origin master
Von git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 docs/index.md  | 18 ++----------------
 docs/mkdocs.md | 17 +++++++++++++++++
 mkdocs.yml     |  2 +-
 3 files changed, 20 insertions(+), 17 deletions(-)
 create mode 100644 docs/mkdocs.md
```

```bash
$ git log
commit 5969abcb120c31977453629f41df9a2268f2fe70
Merge: f1a9427 202ba1a
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed May 10 15:24:26 2017 +0200

    Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

commit f1a9427c2087674fab1b6648ba5a8c70f2f833ae
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed May 10 15:23:57 2017 +0200

    WIP made changes to docs

commit 202ba1a4e25e2af3d101849c0565da16a181b075
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed May 10 14:33:07 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config

commit ef542025b53c461576f37f43c0951df72a2b92aa
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:13 2017 +0200

    initially added vufind-docker documentation

commit 10aeed70270a918e137c7e693a14f79197023977
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* gibt man Remote-Server und Branch an, werden dessen Änderungen in den aktuellen Branch gemerged
* *merge* sortiert den fehlenden *master*-Commit chronologisch ein

## Ignorierte Dateien (auf Branch *master*)

Beim Testen der Entwicklung entstehen temporäre Dateien, die nicht in der Versionskontrolle erfasst werden sollen.
Diese Dateien und Verzeichnisse werden in einer Datei `.gitignore` aufgelistet.

```bash
$ git checkout master
Zu Branch 'master' gewechselt
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
```

* wechselt die Arbeitskopie auf den Branch *master*

```bash
$ mkdocs build
INFO    -  Cleaning site directory
INFO    -  Building documentation to directory: /home/roy/git-workshop/site
```

* *mkdocs*-Befehl zum Erzeugen der statischen Seiten aus dem Projekt

```bash
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        site/

nichts zum Commit vorgemerkt, aber es gibt unversionierte Dateien (benutzen Sie "git add" zum Versionieren)
```

* Unversionierte Dateien und Verzeichnisse können in `.gitignore` aufgenommen werden
* Dateien, welche bereits der Versionskontrolle unterliegen, werden in .gitignore ignoriert

```bash
$ cat >.gitignore <<EOF
/site
EOF
```

* erzeugt Datei `.gitignore` mit einem Eintrag

```bash

$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        .gitignore

nichts zum Commit vorgemerkt, aber es gibt unversionierte Dateien (benutzen Sie "git add" zum Versionieren)
```

* GIT behandelt `.gitignore` als neue unversionierte Datei innerhalb der Arbeitskopie

```bash
$ git add .gitignore
$ git commit
[master c92f8e6] created .gitignore by adding /site
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore

$ git push
Zähle Objekte: 3, Fertig.
Delta compression using up to 8 threads.
Komprimiere Objekte: 100% (2/2), Fertig.
Schreibe Objekte: 100% (3/3), 369 bytes | 0 bytes/s, Fertig.
Total 3 (delta 0), reused 0 (delta 0)
remote: no instance branch. no deploy needed
To git.ub.intern.uni-leipzig.de:git-workshop
   82c53ee..67daf34  master -> master
```

* `.gitignore` werden üblicherweise im Projekt erfasst

***Ordner `/site` entfernen***
## Der Stash (auf Branch *create-vufind-docker-docs*)

Wenn Dateien im Arbeitbereich durch GIT aktualisiert werden sollen (`git pull/merge/apply ...`), diese Dateien aber Änderungen aufweisen, bricht die GIT-Aktion ab, um die neuen Änderungen nicht zu überschreiben.

```bash
$ git checkout create-vufind-docker-docs
Zu Branch 'create-vufind-docker-docs' gewechselt
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 3 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
```

* zwei Commits sind noch nicht mit dem referenzierten Remote-Server Branch synchronisiert

```bash
$ cat >.gitignore <<EOF
/.vscode
EOF
```

* erzeugt Datei `.gitignore` mit einem Eintrag

```bash
$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 3 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        .gitignore

nichts zum Commit vorgemerkt, aber es gibt unversionierte Dateien (benutzen Sie "git add" zum Versionieren)
```

* `.gitignore` befindet sich als unversionierte Datei innerhalb der Arbeitskopie

```bash
$ git pull origin master
Von git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
error: The following untracked working tree files would be overwritten by merge:
        .gitignore
Please move or remove them before you merge.
Abbruch
```

* führt zum Abbruch des Merges, da neu zu erzeugende Datei bereits vorhanden ist
* es ist kein Merge möglich, da GIT keine Informationen über den Inhalt der Datei hat (Commit)

Getrackte Änderungen (geänderte Dateien, hinzugefügte Dateien) können in einen Branch-übergreifenden
Stapel zwischengespeichert werden. Gleichzeitig wird der aktuelle Branch von diesen Änderungen bereinigt.

```bash
$ git add .gitignore
```

* fügt `.gitignore` zur Versionskontrolle hinzu

```bash
$ git stash
Saved working directory and index state WIP on create-vufind-docker-docs: 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs
HEAD ist jetzt bei 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs
```

* alle Änderungen, die sich in der Staging-Area befinden, werden den *Stash* verschoben
* die Arbeitskopie wird von den Änderungen bereinigt

```bash
$ git stash list
stash@{0}: WIP on create-vufind-docker-docs: 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs
```

* im Stash befindet sich ein Puffer, der unsere Änderungen vorhält
* standardmäßig ist der Puffer-Name der Commit, auf welchen die Änderungen aufsetzen

```bash
$ git pull origin master
Von git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 .gitignore | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore
```

* merge konnte problemlos angewendet werden

```bash
$ cat .gitignore
/site
```

* neue Datei `.gitignore` ist auf dem Stand, wie *master*

```bash
$ git stash apply
automatischer Merge von .gitignore
KONFLIKT (hinzufügen/hinzufügen): Merge-Konflikt in .gitignore
```

* wendet den letzten Stash-Puffer auf den aktuellen Branch an
* Konflikt in `.gitignore` wird angemerkt und muss manuell aufgelöst werden

```bash
$ cat .gitignore
<<<<<<< Updated upstream
/site
=======
/.vscode
>>>>>>> Stashed changes
```

* *Updated upstream* bezeichnet die Änderungen, welche seit den eigenen, anzuwendenden Änderungen hinzugekommen sind
* *Stashed changes* bezeichnet die Änderungen, welche durch Anwendung des Stashes eingefügt werden sollen

***Konflikt in `.gitignore` auflösen***

## Dateien löschen

Um Dateien aus der Versionskontrolle zu entfernen, aber im Arbeitsverzeichnis vorzuhalten nutzt man die Option `--cached`.

```bash
$ git log .vscode
commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config
```

* zeigt alle Commits, in denen die spezifizierten Dateien oder Verzeichnisse vorkommen

```bash
$ git rm --cached .vscode/tasks.json
rm '.vscode/tasks.json'
```

* entfernt die spezifizierte Datei aus der Versionskontrolle, behält sie aber in der Arbeitskopie
* Eintrag in `.gitignore` verhindert, dass die Datei als unversionierte Datei aufgeführt wird

## Diffs

Diffs - Unterschiede - sind kumulative Änderungen zu einer Basis, welche im
maschinell verarbeitbaren Format angezeigt werden. Diffs können als Text-Datei
gespeichert und verteilt werden. Die Einarbeitung in die Arbeitskopie erfolgt
mittels `git apply` oder dem Unix-Kommandozeilen-Tool [`patch`][1].

```bash
$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 5 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

	gelöscht:       .vscode/tasks.json

Änderungen, die nicht zum Commit vorgemerkt sind:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)
  (benutzen Sie "git checkout -- <Datei>...", um die Änderungen im Arbeitsverzeichnis zu verwerfen)

	geändert:       .gitignore
```

### `git diff` zeigt die Unterschiede zur Arbeitskopie

```bash
$ git diff
diff --git a/.gitignore b/.gitignore
index c9490a5..4eeb754 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1 +1,2 @@
 /site
+/.vscode
```

* zeigt alle Änderungen in Dateien, die bereits der Versionskontrolle unterliegen und nicht in Staging-Area aufgenommen wurden

### `git diff --staged` zeigt die Unterschiede der Staging-Area zur Arbeitskopie

```bash
$ git diff --staged
diff --git a/.vscode/tasks.json b/.vscode/tasks.json
deleted file mode 100644
index 4625e49..0000000
--- a/.vscode/tasks.json
+++ /dev/null
@@ -1,28 +0,0 @@
-{
-       // See https://go.microsoft.com/fwlink/?LinkId=733558
-       // for the documentation about the tasks.json format
-       "version": "0.1.0",
-       "command": "mkdocs",
-       "isShellCommand": true,
-       "showOutput": "always",
-       "suppressTaskName": true,
-       "tasks": [
-               {
-                       "taskName": "build docs",
-                       "args": [
-                               "build"
-                       ],
-                       "isBuildCommand": true
-                       },
-               {
-
-                       "taskName": "serve the docs",
-                       "args": [
-                               "serve",
-                               "-a",
-                               "127.0.0.1:8001"
-                       ],
-                       "isBackground": true
-               }
-       ]
-}
\ No newline at end of file
```

* zeigt alle Änderungen, die in Staging-Area aufgenommen wurden

### `git diff master` zeigt Unterschiede zweier Branches an

```bash
$ git diff origin/master
diff --git a/.gitignore b/.gitignore
index c9490a5..4eeb754 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1 +1,2 @@
 /site
+/.vscode
diff --git a/docs/vufind-docker.md b/docs/vufind-docker.md
new file mode 100644
index 0000000..a6ea6d2
--- /dev/null
+++ b/docs/vufind-docker.md
@@ -0,0 +1,6 @@
+# VuFind-Entwicklung mit Docker
+
+## Vorbereitung
+
+* installiere GIT
+* installiere Docker
\ No newline at end of file
```

* Vorsicht bei der Reihenfolge: erst kommt die Basis, dann der Ziel-Branch. Die Änderungen, die sich ergeben würden den Basis-Branch an den Ziel-Branch angleichen.
* wird der Zielbranch weggelassen, wird als Ziel die Arbeitskopie angenommen

### `git diff --name-only master` zeigt nur die betroffenen Dateien

```bash
$ git diff --name-only origin/master
.gitignore
docs/vufind-docker.md
```

### `git show [commit]` zeigt Metadaten und Änderungen des angegebenen Commits

```
$ git show
commit 0e1dd75b706268078e47a8db2614abba96416605
Merge: e02bbf9 057be63
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Wed May 10 16:18:51 2017 +0200

    Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs
```

* wird kein Commit angegeben, wird der letzte Commit des aktuellen Branches angezeigt

## Weiterführende Links

* [https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
* [http://de.gitready.com/](http://de.gitready.com/)
* [https://try.github.io/](https://try.github.io/)