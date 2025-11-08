# Anleitung: GitHub Release v1.2.0 erstellen

## Schritt-für-Schritt Anleitung

### 1. Pull Request mergen

Zuerst den Pull Request auf GitHub mergen:
- Gehe zu: https://github.com/iChalix/blvcube-klipper/pulls
- Finde den Pull Request für `claude/create-release-documentation-011CUveQNkYryYkNfWe9yfyJ`
- Review und merge den PR in den main/master Branch

### 2. GitHub Release erstellen

Nachdem der PR gemerged wurde:

1. **Gehe zur Releases-Seite:**
   - Navigiere zu: https://github.com/iChalix/blvcube-klipper/releases
   - Klicke auf "Draft a new release"

2. **Tag erstellen:**
   - Tag version: `v1.2.0`
   - Target: `main` (oder dein Standard-Branch)
   - Klicke auf "Create new tag: v1.2.0 on publish"

3. **Release-Details eingeben:**
   - **Release title:** `v1.2.0 - Precision Enhancement`
   - **Release description:** Kopiere den Inhalt aus `RELEASE_NOTES_v1.2.0.md`

4. **Optional - Previous tags:**
   - Falls vorhanden, wähle `v1.1.0` als "Previous tag"
   - Dies generiert automatisch einen Changelog-Link

5. **Veröffentlichen:**
   - Überprüfe alle Eingaben
   - Klicke auf "Publish release"

### 3. Lokalen Tag aktualisieren (Optional)

Falls du den lokalen Tag löschen und neu erstellen möchtest:

```bash
# Lokalen Tag löschen
git tag -d v1.2.0

# Nach dem GitHub Release, Tag vom Remote holen
git fetch --tags

# Überprüfen
git tag -l
```

## Alternative: GitHub CLI (falls verfügbar)

Falls du später die GitHub CLI installierst, kannst du Releases auch so erstellen:

```bash
# Installation der GitHub CLI
# Siehe: https://cli.github.com/

# Authentifizierung
gh auth login

# Release erstellen
gh release create v1.2.0 \
  --title "v1.2.0 - Precision Enhancement" \
  --notes-file RELEASE_NOTES_v1.2.0.md \
  --target main

# Release mit automatisch generiertem Changelog
gh release create v1.2.0 \
  --title "v1.2.0 - Precision Enhancement" \
  --notes-file RELEASE_NOTES_v1.2.0.md \
  --generate-notes \
  --target main
```

## Release-Dateien im Repository

- **RELEASE_NOTES_v1.2.0.md** - Vollständige Release-Notes (kopiere diesen Inhalt)
- **RELEASES.md** - Alle Release-Notes für alle Versionen
- **CHANGELOG.md** - Technischer Changelog im Standard-Format
- **README.md** - Aktualisiert mit v1.2.0 Information

## Nächste Schritte nach dem Release

1. **Announcement** (optional):
   - Teile das Release in relevanten Communities
   - BLV Cube Discord/Forums
   - Klipper Community

2. **Cleanup**:
   - Lösche den Feature-Branch nach dem Merge
   - Aktualisiere lokales Repository: `git pull --tags`

3. **Testing**:
   - Teste das Release mit einer frischen Installation
   - Überprüfe, dass alle Links in der Dokumentation funktionieren

## Überprüfung

Nach dem Release solltest du folgendes sehen:

- ✅ Release v1.2.0 auf https://github.com/iChalix/blvcube-klipper/releases
- ✅ Tag v1.2.0 im Repository
- ✅ Vollständige Release-Notes sichtbar
- ✅ Changelog zwischen v1.1.0 und v1.2.0 (falls v1.1.0 existiert)

## Support

Bei Problemen:
- Überprüfe GitHub-Berechtigungen für das Repository
- Stelle sicher, dass du Schreibrechte hast
- Kontaktiere den Repository-Eigentümer bei Zugriffsproblemen
