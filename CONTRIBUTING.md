# Git-Regeln

Damit am Ende nicht alles durcheinander ist, halten sich alle an diese Regeln.

## Grundregeln

- **Nie direkt auf `main` committen.** Alles läuft über Branches und Pull Requests.
- **Nur im eigenen TP-Ordner arbeiten.** Änderungen an fremden Ordnern oder an Dateien im Hauptverzeichnis vorher mit der Gruppe absprechen.
- **Vor dem Arbeiten immer pullen**, in GitHub Desktop oder mit `git pull`.
- **Keine Passwörter, Tokens oder WLAN-Daten ins Repo.** Dafür gibt es `.env` und `secrets.h`, beide stehen in der `.gitignore`. Vorlagen wie `.env.example` dürfen rein.

## Ablauf für ein neues Feature

```bash
git switch main
git pull
git switch -c feature/tp3-rest-api-controller

# arbeiten, testen, dann nur den eigenen Ordner committen
git add tp3-api-dashboard/
git commit -m "feat(tp3): Controller für Messwerte angelegt"

git push -u origin feature/tp3-rest-api-controller
```

Danach auf GitHub einen Pull Request nach `main` aufmachen.

In GitHub Desktop geht das über *Current Branch → New Branch*, dann wie gewohnt committen, *Publish branch* und *Create Pull Request*.

## Branches

Schema: `<typ>/tp<nummer>-<kurze-beschreibung>`

| Typ | Wofür |
|---|---|
| `feature/` | neue Funktion |
| `fix/` | Fehler beheben |
| `docs/` | nur Dokumentation |

Beispiele:

- `feature/tp3-rest-api-controller`
- `fix/tp1-sensor-timeout`
- `docs/schnittstellen-messwerte` (betrifft mehrere TPs, deshalb ohne Nummer)

Dabei gilt:

- Alles klein schreiben, Wörter mit Bindestrich trennen, keine Umlaute und keine Leerzeichen. `Feature/...` und `feature/...` sind für Git zwei verschiedene Branches, unter Windows gibt das Ärger.
- Ein Branch ist für genau ein Feature da.
- Nach dem Merge wird der Branch gelöscht.

## Commits

Wir halten uns an [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).

```
<typ>(<scope>): <beschreibung>
```

- **Typ** klein geschrieben, siehe Tabelle unten.
- **Scope** ist das Teilprojekt: `tp1`, `tp2`, `tp3` oder `tp4`. Betrifft ein Commit das ganze Repo, fällt der Scope weg.
- **Beschreibung** auf Deutsch, kurz (höchstens etwa 72 Zeichen), ohne Punkt am Ende. Wir schreiben im Partizip (*eingebaut*, *behoben*, *ergänzt*), damit alles gleich aussieht.

| Typ | Wann | Beispiel |
|---|---|---|
| `feat` | neue Funktion | `feat(tp3): GET /api/messwerte/aktuell eingebaut` |
| `fix` | Fehler behoben | `fix(tp1): Timeout beim Auslesen des Sensors behoben` |
| `docs` | Doku oder Kommentare | `docs: Schnittstelle zwischen TP2 und TP3 beschrieben` |
| `refactor` | Code umgebaut, Verhalten bleibt gleich | `refactor(tp2): SQL-Abfragen in eigene Klasse ausgelagert` |
| `style` | nur Formatierung, keine Logik | `style(tp3): C#-Namenskonventionen im Controller angewendet` |
| `test` | Tests hinzugefügt oder angepasst | `test(tp3): Unit Tests für MesswerteController ergänzt` |
| `chore` | Konfiguration, Abhängigkeiten, Wartung | `chore: .gitignore um Python-Dateien erweitert` |

Ein Commit ist eine zusammenhängende Änderung. Nachrichten wie `Update`, `fixes` oder `jetzt aber wirklich` gibt es nicht.

### Breaking Changes

Wenn eine Änderung ein anderes Teilprojekt kaputt macht (z. B. ein API-Endpunkt wird umbenannt), kommt ein `!` hinter den Scope. Die Gruppe wird vorher informiert und `docs/schnittstellen.md` im selben PR angepasst.

```
refactor(tp3)!: Endpunkt /api/current in /api/messwerte/aktuell umbenannt
```

## Pull Requests

- Der **Titel** folgt demselben Format wie Commits, z. B. `feat(tp3): REST-API für Messwerte eingebaut`. Beim Mergen wird der Titel zur Commit-Nachricht auf `main`.
- Die **Beschreibung** kommt aus der Vorlage und wird ausgefüllt.
- **Gemergt wird erst, wenn**
  - mindestens eine andere Person aus der Gruppe zugestimmt hat (Approve),
  - das Feature funktioniert und getestet ist (Unit Tests oder von Hand, steht in der PR-Beschreibung),
  - es keine Konflikte mit `main` gibt.
- Gemergt wird mit **Squash and merge**. So steht auf `main` pro Feature genau ein sauberer Commit.

## Branch auf den aktuellen Stand bringen

Wenn sich `main` geändert hat, während du an deinem Branch arbeitest:

```bash
git switch main
git pull
git switch feature/tp3-rest-api-controller
git merge main
```

Konflikte lösen, testen, committen, pushen. Bei Konflikten in fremden Ordnern nicht raten, sondern kurz mit der Person reden.

## Was nicht ins Repo gehört

- Build-Ordner (`bin/`, `obj/`, `build/`, `__pycache__/`)
- Virtuelle Umgebungen (`.venv/`)
- IDE-Einstellungen (`.vs/`, `.idea/`)
- Zugangsdaten jeder Art

Die `.gitignore` deckt das ab. Ist trotzdem etwas Falsches drin gelandet: Bescheid sagen und nicht einfach weiter committen. Ein Passwort, das einmal gepusht wurde, gilt als verbrannt und muss geändert werden, auch wenn die Datei danach gelöscht wird.

