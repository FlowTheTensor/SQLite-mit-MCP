# Anleitung für Schüler: SQLite MCP Server (FastMCP)

## Schnellstart-Anleitung

### 1. Virtuelle Umgebung erstellen (empfohlen)

Öffne PowerShell im Projektordner und führe aus:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

### 2. Abhängigkeiten installieren

```powershell
pip install -r requirements.txt
```

Das installiert FastMCP und alle benötigten Python-Pakete.

### 3. Datenbank erstellen

```powershell
python create_database.py
```

Du solltest die Meldung sehen: "✓ Datenbank erfolgreich erstellt"

### 4. In Claude Desktop einbinden

**Option A - Automatische Konfiguration (empfohlen):**

Führe einfach das Konfigurations-Script aus:

```powershell
python generate_config.py
```

Das Script zeigt dir die Konfiguration an und speichert sie in `claude_desktop_config.json`.

**Dann:**
- Drücke in Claude Desktop Ctrl +
- Unter `Entwickler` auf `Config bearbeiten`
- Öffne die Datei `claude_desktop_config.json` mit einem Texteditor
- Kopiere den Inhalt aus der generierten `claude_desktop_config.json` hinein

---

**Option B - Manuelle Konfiguration:**

**Konfigurationsdatei öffnen:**
- Drücke `Windows + R`
- Gib ein: `%APPDATA%\Claude`
- Öffne die Datei `claude_desktop_config.json` mit einem Texteditor

**Server hinzufügen (mit venv):**

```json
{
  "mcpServers": {
    "sqlite-schule": {
      "command": "DEIN_PFAD_HIER\\venv\\Scripts\\python.exe",
      "args": [
        "DEIN_PFAD_HIER\\src\\index.py"
      ]
    }
  }
}
```

**Pfad herausfinden:**

Im Projektordner in PowerShell:
```powershell
Get-Location
```

Kopiere den Pfad und ersetze `DEIN_PFAD_HIER` damit.

**Wichtig:** Claude Desktop benötigt **absolute Pfade** - relative Pfade funktionieren nicht!

### 5. Claude Desktop neu starten

Schließe Claude Desktop komplett und starte es neu.

### 6. Testen

Stelle Claude eine Frage wie:
```
Welche Schüler gibt es in der Datenbank?
```

Claude sollte jetzt die Datenbank abfragen können!

## Beispiel-Fragen zum Ausprobieren

### Einfach:
- "Zeige alle Schüler"
- "Welche Lehrer gibt es?"
- "Liste alle Kurse auf"

### Mittel:
- "Zeige alle Schüler aus Klasse 10a"
- "Welche Noten hat Max Mustermann?"
- "Wer unterrichtet Informatik?"

### Fortgeschritten:
- "Berechne den Notendurchschnitt von Anna Schmidt"
- "Welche Schüler haben in Mathematik eine 1 vor dem Komma?"
- "Zeige alle Klausurnoten mit Schülernamen und Kursnamen"

## Datenbank-Struktur verstehen

Die Datenbank hat 4 Tabellen:

📚 **schueler**: Schülerinformationen
- id, vorname, nachname, klasse, geburtsdatum, email

👨‍🏫 **lehrer**: Lehrerinformationen  
- id, vorname, nachname, fach, raum

📖 **kurse**: Kursinformationen
- id, kursname, lehrer_id, raum, wochentag, uhrzeit

📝 **noten**: Noten
- id, schueler_id, kurs_id, note, datum, art

## Häufige Probleme

**Claude antwortet, aber ohne Datenbankzugriff?**
→ Server wurde nicht richtig konfiguriert oder Claude nicht neu gestartet

**"ModuleNotFoundError: No module named 'fastmcp'" Fehler?**
→ `pip install -r requirements.txt` ausführen
→ Stelle sicher, dass die venv aktiviert ist oder nutze den venv-Python-Pfad in der Konfiguration

**Datenbank leer?**
→ `python create_database.py` ausführen

**Python-Befehl nicht gefunden?**
→ Versuche `py` statt `python`
→ Stelle sicher, dass Python installiert ist

## Was passiert im Hintergrund?

1. Du stellst Claude eine Frage
2. Claude erkennt, dass es Datenbankinfos braucht
3. Claude ruft eines der Tools auf:
   - `list_tables` - Welche Tabellen gibt es?
   - `describe_table` - Wie sieht eine Tabelle aus?
   - `query_database` - SQL-Abfrage ausführen
4. Der MCP-Server führt die Abfrage aus
5. Claude bekommt das Ergebnis und antwortet dir

## Aufgaben zum Experimentieren

1. Stelle 5 verschiedene Fragen an die Datenbank
2. Lass dir die Struktur aller Tabellen zeigen
3. Frage nach dem besten Schüler in einem Fach
4. Lass Claude eine komplexe Abfrage mit mehreren Tabellen erstellen
5. Experimentiere mit Aggregationen (Durchschnitt, Anzahl, etc.)

Viel Erfolg! 🚀
