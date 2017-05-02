# VuFind Workshop

## Vorbereitung

* VPN-Zugang zu internen UBL-Netzwerk
* SSH-Key der Nutzer im Git-Repo hinterlegen, Rechte geben
* Git, Docker, docker-compose installieren
* Ports freischalten auf Solr4Entwickler

## GIT einrichten

* Benutzerinformationen mit `git config`

```bash
git config --global push.default simple
git config --global user.email <emailadresse>
git config --global user.name <name>
```

* standard-Editor für commit-Messages mit `git config --global core.editor <binary>`

## Instanzen-Branch auschecken

* Repository clonen `git clone git@git.ub.intern.uni-leipzig.de:vufind2`
* Branch auschecken `git checkout instance/<isil>`

## Vufind-Konfigurationshierarchie

* `dev`-Konfiguration aus Instanzenkonfiguration erzeugen

```bash
find vufind2/de_l152/dev/ -name \*.sample | while read -r file; do cp -v "$file" "${file%%.sample}"; done;
```

* Solr-Index anpassen in `<isil>/dev/config/vufind/config.ini`

```ini
[Index]
url = http://172.18.85.142:8085/solr
```

* Session-Storage überschreiben in `<isil>/dev/config/vufind/config.ini` (temporär hinzufügen, bis Datenbank erzeugt wurde)

```ini
[Session]
type = file
```

* Apache2-Konfiguration anpassen (notwendig, wenn eigene Module entwickelt werden)
  * `SetEnv VUFIND_LOCAL_DIR /usr/local/vufind2/<isil>/dev`
  * `SetEnv VUFIND_LOCAL_MODULES finc`

## Docker-Container starten

* `docker-compose up`
  * erstellt und startet Container-Definition aus `docker-compose.yml`
* `docker-compose stop`
  * stoppt den Container, ohne ihn zu löschen
* `docker-compose start`
  * startet den bereits erstellten Container
* `docker-compose down`
  * stoppt den Container und entfernt alle in der `docker-compose.yml` definierten Netzwerke und Container

## VuFind aufrufen

* http(s)://localhost/

## Datenbank enrichten

* http(s)://localhost/Install
