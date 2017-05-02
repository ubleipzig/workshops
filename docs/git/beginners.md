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
$ git clone git@git.ub.intern.uni-leipzig.de:git-workshop
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
origin  git@git.ub.intern.uni-leipzig.de:git-workshop (fetch)
origin  git@git.ub.intern.uni-leipzig.de:git-workshop (push)
```

* `git clone` fügt automatisch den Server, von dem das Repository geklont wurde, als *origin*-Remote hinzu

## Dateien umbenennen

Änderungen werden nicht automatisch zum Commit vorgemerkt, wenn man Dateien im Arbeitsverzeichnis umbenennt oder löscht. Man muss diese Änderungen - wie inhaltliche Änderungen - explizit zum Commit vormerken.

```bash
$ mv docs/index.md docs/mkdocs.md
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
Änderungen, die nicht zum Commit vorgemerkt sind:
  (benutzen Sie "git add/rm <Datei>...", um die Änderungen zum Commit vorzumerken)
  (benutzen Sie "git checkout -- <Datei>...", um die Änderungen im Arbeitsverzeichnis zu verwerfen)

        gelöscht:       docs/index.md

Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        docs/mkdocs.md

keine Änderungen zum Commit vorgemerkt (benutzen Sie "git add" und/oder "git commit -a")
$ git rm docs/index.md
$ git add docs/mkdocs.md
$ $ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        umbenannt:      docs/index.md -> docs/mkdocs.md

```

* git erkennt, dass eine Datei umbenannt wurde, in dem der Inhalt der Dateien verglichen wird
* werden zwischen Umbenennen und Hinzufügen zur Versionskontrolle Änderungen am Inhalt der Datei vorgenommen wird die Datei nicht als *umbenannt* erkannt

### besser Dateien umbenennen mit `git mv <src> <dst>`


```bash
$ git mv docs/index.md docs/mkdocs.md
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        umbenannt:      docs/index.md -> docs/mkdocs.md

```

## Code ändern / Dateien hinzufügen

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

lokales Repository weicht um einen Commit vom Remote-Server ab

```bash
$ git status
Auf Branch master
Ihr Branch ist vor 'origin/master' um 1 Commit.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
nothing to commit, working tree clean

```

### `git push` veröffentlicht den neuen Commit auf Remote-Server

```bash
$ git push
Zähle Objekte: 5, Fertig.
Delta compression using up to 8 threads.
Komprimiere Objekte: 100% (4/4), Fertig.
Schreibe Objekte: 100% (5/5), 564 bytes | 0 bytes/s, Fertig.
Total 5 (delta 0), reused 0 (delta 0)
To git@git.ub.intern.uni-leipzig.de:git-workshop
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

### `git push --set-upstream origin <local-branch-name>:` veröffentlicht den Branch

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

## Branches aktuell halten

```bash
$ git checkout create-vufind-docker-docs

$ git pull
Already up-to-date.
```

* ohne Angabe von Remote-Server und Branch wird der aktuelle Branch mit Änderungen des referenzierten Branchs aktualisiert

```bash
$ git pull origin master
Von git@git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 docs/index.md  | 18 ++----------------
 docs/mkdocs.md | 17 +++++++++++++++++
 mkdocs.yml     |  2 +-
 3 files changed, 20 insertions(+), 17 deletions(-)
 create mode 100644 docs/mkdocs.md

$ git log
commit 1f6537c5c7045a6baad78beb6537d1cb7aa564dd
Merge: 1f01e40 82c53ee
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 15:26:14 2017 +0200

    Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config

commit ef542025b53c461576f37f43c0951df72a2b92aa
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:13 2017 +0200

    initially added vufind-docker documentation

commit 82c53ee44523e928adc7a58a91586c5469748da4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:58:23 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 10aeed70270a918e137c7e693a14f79197023977
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* gibt man Remote-Server und Branch an, werden dessen Änderungen in den aktuellen Branch gemerged

## Ignorierte Dateien

Beim Testen der Entwicklung entstehen temporäre Dateien, die nicht in der Versionskontrolle erfasst werden sollen.
Diese Dateien und Verzeichnisse werden in einer Datei `.gitignore` aufgelistet.

```bash
$ git checkout master

$ mkdocs build
INFO    -  Cleaning site directory
INFO    -  Building documentation to directory: /home/roy/git-workshop/site

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

$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        .gitignore

nichts zum Commit vorgemerkt, aber es gibt unversionierte Dateien (benutzen Sie "git add" zum Versionieren)
```

### `git ls-files --other --ignored --exclude-standard` zeigt von Versionskontrolle ausgenommene Dateien

```bash
$ git ls-files  --other  --ignored --exclude-standard
site/css/highlight.css
site/css/theme.css
site/css/theme_extra.css
...
site/search.html
site/sitemap.xml
site/vufind/index.html
```

### `git clean -fdX` löscht alle Dateien, die von der Versionkontrolle ausgenommen sind

```bash
$ git clean -fdXn
Würde site/ löschen

$ git clean -fdX
Lösche site/
```
* Vorsicht! `-x` löscht auch Dateien, die nicht zur Versionkontrolle hinzugefügt wurden


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

## Der Stash

Wenn Dateien im Arbeitbereich durch GIT aktualisiert werden sollen (`git pull/merge/apply ...`), diese Dateien aber Änderungen aufweisen, bricht die GIT-Aktion ab, um die neuen Änderungen nicht zu überschreiben.

```bash
$ git checkout create-vufind-docker-docs
Zu Branch 'create-vufind-docker-docs' gewechselt
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 2 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)

$ cat >.gitignore <<EOF
/.vscode
EOF

$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 2 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
Unversionierte Dateien:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)

        .gitignore

nichts zum Commit vorgemerkt, aber es gibt unversionierte Dateien (benutzen Sie "git add" zum Versionieren)

$ git pull origin master
Von git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
error: The following untracked working tree files would be overwritten by merge:
        .gitignore
Please move or remove them before you merge.
Abbruch
```

Getrackte Änderungen (geänderte Dateien, hinzugefügte Dateien) können in einen Branch-übergreifenden
Stapel zwischengespeichert werden. Gleichzeitig wird der aktuelle Branch von diesen Änderungen bereinigt.

```bash
$ git add .gitignore
$ git stash
Saved working directory and index state WIP on create-vufind-docker-docs: 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs
HEAD ist jetzt bei 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 2 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
nothing to commit, working tree clean

$ git stash list
stash@{0}: WIP on create-vufind-docker-docs: 1f6537c Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

$ git stash show
 .gitignore | 1 +
 1 file changed, 1 insertion(+)
```

* unversionierte Dateien werden nicht gestashed
* der Stash wird nach dem Commit benannt, auf den die gestashten Änderungen aufsetzen

```bash
$ git pull origin master
Von git.ub.intern.uni-leipzig.de:git-workshop
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 .gitignore | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore

$ cat .gitignore
/site

$ git stash pop
automatischer Merge von .gitignore
KONFLIKT (hinzufügen/hinzufügen): Merge-Konflikt in .gitignore

$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 4 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
Nicht zusammengeführte Pfade:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)
  (benutzen Sie "git add/rm <Datei>...", um die Auflösung zu markieren)

        von beiden hinzugefügt: .gitignore

keine Änderungen zum Commit vorgemerkt (benutzen Sie "git add" und/oder "git commit -a")

$ cat .gitignore
<<<<<<< Updated upstream
/site
=======
/.vscode
>>>>>>> Stashed changes
```

* auftretende Konflikte müssen aufgelöst werden

```bash
$ git add .gitignore

$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 4 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        geändert:       .gitignore

$ git commit
[create-vufind-docker-docs e705442] added /.vscode/ to .gitignore
 1 file changed, 1 insertion(+)
```

## Dateien löschen

Um Dateien aus der Versionskontrolle zu entfernen, aber im Arbeitsverzeichnis vorzuhalten nutzt man die Option `--cached`.

```bash
$ git log .vscode
commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config

$ git rm --cached .vscode/tasks.json
rm '.vscode/tasks.json'

$ git status
Auf Branch create-vufind-docker-docs
Ihr Branch ist vor 'origin/create-vufind-docker-docs' um 5 Commits.
  (benutzen Sie "git push", um lokale Commits zu publizieren)
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        gelöscht:       .vscode/tasks.json
```

* Datei ist noch vorhanden, wird jedoch nicht als ungetrackte Datei angezeigt, weil das Verzeichnis *.vscode*
 in der Datei `.gitignore` aufgeführt ist.

```bash
$ git commit
[create-vufind-docker-docs 2eb44bb] removed ide config
 1 file changed, 28 deletions(-)
 delete mode 100644 .vscode/tasks.json
```

## Die Historie

Die Entwicklung des Projektes veranschaulicht die Historie der Versionskontrolle.
Sie hilft, den Ursprung von Implementationen, Autoren und Bemerkungen anzuzeigen
und nachzuvollziehen.

### `git log` zeigt gesamte Historie eines Branches

```bash
$ git log
commit 2eb44bbe8c11455cd4b24b3adda8ff2918d55855
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 16:49:00 2017 +0200

    removed ide config

commit e705442108cc12c254c239d010cc5fe0eeb9808e
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 16:46:17 2017 +0200

    added /.vscode/ to .gitignore

commit 4b9c46afca616341275cf831fe8b1fb237d4f1d0
Merge: 1f6537c 67daf34
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 16:44:18 2017 +0200

    Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

commit 67daf3464058aeb0a955a5acecb8dfc59fda22e4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 15:32:12 2017 +0200

    created .gitignore by adding /site

commit 1f6537c5c7045a6baad78beb6537d1cb7aa564dd
Merge: 1f01e40 82c53ee
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 15:26:14 2017 +0200

    Merge branch 'master' of git.ub.intern.uni-leipzig.de:git-workshop into create-vufind-docker-docs

commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config

commit ef542025b53c461576f37f43c0951df72a2b92aa
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:13 2017 +0200

    initially added vufind-docker documentation

commit 82c53ee44523e928adc7a58a91586c5469748da4
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:58:23 2017 +0200

    * moved brief mkdocs docu to separate page
    * created new index page
    * modified title

commit 10aeed70270a918e137c7e693a14f79197023977
Author: Jen Barber <jen@reynholm-industries.co.uk>
Date:   Wed Apr 5 10:49:15 2017 +0200

    created mkdocs structure
```

* zeigt neben eineindeutigen Commit-Hash, Commit-Autor und Commit-Datum, zusätzlich
 die Commit-Bemerkung
* Sortierung erfolgt nach Anwendung des Commits und muss nicht dem Commit-Datum
 entsprechen

```bash
$ git log --follow .vscode/tasks.json
commit 2eb44bbe8c11455cd4b24b3adda8ff2918d55855
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Thu Apr 27 16:49:00 2017 +0200

    removed ide config

commit 1f01e40096b96b33baab268ac1823ef5a99c5f1c
Author: Richmond Avenal <richmond@reynholm-industries.co.uk>
Date:   Thu Apr 27 14:42:34 2017 +0200

    added ide config
```

* zeigt nur die Commit-Historie, welche die angegebene Datei betrifft, auch, wenn sie nicht mehr in der Versionkontrolle vorhanden ist
* die Datei muss im Arbeitsbereich vorhanden sein

## Diffs

Diffs - Unterschiede - sind kumulative Änderungen zu einer Basis, welche im
maschinell verarbeitbaren Format angezeigt werden. Diffs können als Text-Datei
gespeichert und verteilt werden. Die Einarbeitung in die Arbeitskopie erfolgt
mittels `git apply` oder dem Unix-Kommandozeilen-Tool [`patch`][1].

```bash
$ git status
Auf Branch master
Ihr Branch ist auf dem selben Stand wie 'origin/master'.
zum Commit vorgemerkte Änderungen:
  (benutzen Sie "git reset HEAD <Datei>..." zum Entfernen aus der Staging-Area)

        geändert:       docs/git.md

Änderungen, die nicht zum Commit vorgemerkt sind:
  (benutzen Sie "git add <Datei>...", um die Änderungen zum Commit vorzumerken)
  (benutzen Sie "git checkout -- <Datei>...", um die Änderungen im Arbeitsverzeichnis zu verwerfen)

        geändert:       docs/git.md


```

### `git diff` zeigt die Unterschiede zur Arbeitskopie

```bash
$ git diff
diff --git a/mkdocs.yml b/mkdocs.yml
index 3212117..3dca70e 100644
--- a/mkdocs.yml
+++ b/mkdocs.yml
@@ -1 +1,2 @@
 site_name: Workshop Dokumentationen
+theme: readthedocs
```

### `git diff --staged` zeigt die Unterschiede der Staging-Area zur Arbeitskopie

```bash
$ git add mkdocs.yml
$ git diff --staged
diff --git a/mkdocs.yml b/mkdocs.yml
index 3212117..3dca70e 100644
--- a/mkdocs.yml
+++ b/mkdocs.yml
@@ -1 +1,2 @@
 site_name: Workshop Dokumentationen
+theme: readthedocs

```

### `git diff master..origin/master` zeigt Unterschiede zweier Branches an

```bash
$ git commit
[master 298221d] changed theme to readthedocs
 1 file changed, 1 insertion(+)

$ git diff master..origin/master
diff --git a/mkdocs.yml b/mkdocs.yml
index 3dca70e..3212117 100644
--- a/mkdocs.yml
+++ b/mkdocs.yml
@@ -1,2 +1 @@
 site_name: Workshop Dokumentationen
-theme: readthedocs
```

* Vorsicht bei der Reihenfolge: erst kommt die Basis, dann der Ziel-Branch. Die Änderungen, die sich ergeben würden den Basis-Branch an den Ziel-Branch angleichen.

### `git show [commit]` zeigt Metadaten und Änderungen des angegebenen Commits

```
$ git show
commit 298221dd4482dc69057a0e938086e5c432f1a247
Author: Roy Trenneman <roy@reynholm-industries.co.uk>
Date:   Fri Apr 28 11:30:02 2017 +0200

    changed theme to readthedocs

diff --git a/mkdocs.yml b/mkdocs.yml
index 3212117..3dca70e 100644
--- a/mkdocs.yml
+++ b/mkdocs.yml
@@ -1 +1,2 @@
 site_name: Workshop Dokumentationen
+theme: readthedocs
```

* wird kein Commit angegeben, wird der letzte Commit des aktuellen Branches angezeigt

## Weiterführende Links

* [https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
* [http://de.gitready.com/](http://de.gitready.com/)
* [https://try.github.io/](https://try.github.io/)