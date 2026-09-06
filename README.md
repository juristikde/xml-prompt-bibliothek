# Prompt Bibliothek

<div align="center">

**Kuratierte Sammlung strukturierter XML-Prompts – durchsuchbar, filterbar, kopierfertig.**  
Passt zum [Prompt Baukasten](https://juristikde.github.io/xml-prompt-baukasten) · rein clientseitig · mehrsprachig

[![Offline-fähig](https://img.shields.io/badge/offline-fähig*-75C46B?style=flat-square)](#technik)
[![No Backend](https://img.shields.io/badge/backend-keins-lightgrey?style=flat-square)](#technik)
[![License: MIT](https://img.shields.io/badge/license-MIT-00ACD7?style=flat-square)](#lizenz)

\*Katalog und Prompt-Dateien müssen lokal oder per HTTPS erreichbar sein.

**[➜ Live-Bibliothek](https://juristikde.github.io/xml-prompt-bibliothek/)** · **[➜ Prompt Baukasten](https://juristikde.github.io/xml-prompt-baukasten)**

</div>

---

## Inhaltsverzeichnis

- [Überblick](#überblick)
- [Features](#features)
- [Quickstart](#quickstart)
- [Bedienung](#bedienung)
- [Datenformat](#datenformat)
- [Eigene Bibliothek betreiben](#eigene-bibliothek-betreiben)
- [Prompts einreichen (GitHub)](#prompts-einreichen-github)
- [URL-Parameter](#url-parameter)
- [Technik](#technik)
- [Mitwirken](#mitwirken)
- [Lizenz](#lizenz)

---

## Überblick

Die **Prompt Bibliothek** ist ein Katalog-Viewer für strukturierte LLM-Prompts. Sie lädt Metadaten aus `prompts/index.json` und den eigentlichen Prompt-Text aus `prompts/{id}.json` – alles im Browser, ohne Server-Logik.

Typischer Workflow:

1. In der Bibliothek suchen und filtern  
2. Prompt öffnen (`>_`) und kopieren  
3. Im [Prompt Baukasten](https://juristikde.github.io/xml-prompt-baukasten) unter **XML anzeigen → Übernehmen** einfügen und weiterbearbeiten  

Zwei Nutzungswege:

| Weg | Für wen | Was du tust |
| --- | --- | --- |
| **Eigene Instanz** | Personen, Teams, Unternehmen | Repo/Dateien kopieren, eigenen `prompts/`-Ordner pflegen, Seite selbst hosten |
| **Öffentliche Sammlung** | Community-Beiträge | Pull Request mit umfangreichem Prompt (≥ 10 000 Zeichen) und korrektem `index.json`-Eintrag |

---

## Features

- **Durchsuchbare Tabelle** (Desktop) und **Karten** (Mobil) mit virtueller Liste bei vielen Einträgen  
- **Filter:** Freitext + Sprache, Anwender, Zielsetzung, Ausgabeformat, Modell („getestet mit“), Autor (Combo-Felder mit Vorschlägen)  
- **Mehrfach-Sortierung** (Spaltenklick, mit Shift/Ctrl/Cmd weitere Ebenen)  
- **Prompt-Ansicht** mit Kopieren und optionalem Zeilenumbruch  
- **ID-Generator** („Adresse“) im Navi-Stil für neue Einträge  
- **i18n** analog zum Baukasten (`lang/*.json`, Hash `setLanguage=…`)  
- **Mobile Sheets:** Prompt, Metadaten, Filter – gleiches Interaktionsmodell wie der Baukasten  

Kein Tracking, kein Account, keine Prompt-Inhalte auf fremden Servern (außer dem Host deiner Dateien).

---

## Quickstart

**Live**

→ [juristikde.github.io/xml-prompt-bibliothek](https://juristikde.github.io/xml-prompt-bibliothek/)

**Lokal**

```bash
# im Projektroot (neben index.html und prompts/)
python3 -m http.server 8000
# → http://localhost:8000/
```

Ohne lokalen Server können `fetch`-Aufrufe auf `prompts/*.json` je nach Browser eingeschränkt sein – deshalb einen einfachen HTTP-Server nutzen.

---

## Bedienung

| Element | Desktop | Mobil |
| --- | --- | --- |
| Suche | Suchfeld oben | Suchfeld + **Filter**-Sheet |
| Filter | Combo-Zeile unter der Suche | Sheet von unten, Anwenden / Zurücksetzen |
| Eintrag öffnen | Symbol **`>_`** | **`>_`** öffnet Prompt-Sheet |
| Metadaten | Spalten in der Tabelle | Button **Mehr** → Meta-Sheet |
| Sprache | Toolbar | Optionen-Menü |
| Neue ID | Button / Optionen | Optionen → ID erzeugen |

Kopieren legt den Prompt-Text in die Zwischenablage. Im Baukasten: XML-Ansicht öffnen, einfügen, **Übernehmen**.

---

## Datenformat

Die Bibliothek erwartet genau diese Struktur:

```
prompts/
├── index.json                          # Katalog (Metadaten aller Einträge)
├── hoch-steil-nebel-42-a.json          # Prompt-Körper pro ID
├── tief-kalt-sturm-108-c.json
└── …
```

### `index.json`

Zwei Formen werden akzeptiert:

```json
{
  "prompts": [
    {
      "id": "hoch-steil-nebel-42-a",
      "title": "Kundenservice Onlineshop",
      "symbol": "🛒",
      "language": "de",
      "audience": "Support-Teams",
      "purpose": "Freundliche, regelkonforme Kundenantworten",
      "outputFormat": "Text",
      "testedWith": "GPT-4o, Claude 3.5",
      "updatedAt": "2026-03-15",
      "autor": "Max Mustermann",
      "keywords": ["support", "ecommerce", "deutsch"]
    }
  ]
}
```

oder ein reines Array `[ { … }, { … } ]`.

| Feld | Pflicht | Beschreibung |
| --- | --- | --- |
| `id` | ja | Eindeutige Dateiname-Basis (ohne `.json`), idealerweise vom ID-Generator |
| `title` | empfohlen | Anzeigetitel in Liste und Modal |
| `language` / `sprache` | empfohlen | ISO-639-1 (`de`, `en`, …) – Filter & Labels |
| `audience` | optional | Zielgruppe (z. B. “Support”, “Legal team”) |
| `purpose` | optional | Wofür der Prompt gedacht ist |
| `outputFormat` | optional | Erwartetes Ausgabeformat (z. B. `Text`, `Python Skript`, `Javascript`, `Bild`, `PDF`) |
| `testedWith` | optional | Modelle / Umgebungen, mit denen getestet wurde |
| `updatedAt` | empfohlen | `YYYY-MM-DD` |
| `autor` | optional | Urheber oder Team |
| `keywords` / `schlagworte` | optional | Array oder kommaseparierter String – fließt in die Suche |
| `symbol` | optional | Emoji/Kurzzeichen (Fallback `📄`) |

Feldnamen sind bewusst **englisch** (international nutzbar). Beim Einlesen werden ältere deutsche Keys (`anwender`, `zielsetzung`, `getestetMit`, `ausgabeformat`) noch als Fallback akzeptiert – neue Einträge bitte nur mit den englischen Namen anlegen.

**`outputFormat`:** freier Text, empfohlen einheitliche Kurzlabels wie `Text`, `Python Skript`, `Javascript`, `Bild`, `PDF` (Filter und Sortierung nutzen den exakten Wert bzw. Teilstring).

Es gibt **keinen** Prompt-Volltext in `index.json` – der bleibt in der Einzeldatei. So bleibt der Katalog schlank und die virtuelle Liste schnell.

### `prompts/{id}.json`

```json
{
  "prompt": "<prompt>\n  <rolle>…</rolle>\n  …\n</prompt>"
}
```

- Schlüssel **`prompt`** (String) ist verbindlich.  
- Inhalt typischerweise wohlgeformtes XML, das der Baukasten einlesen kann.  
- Andere Felder in dieser Datei werden von der aktuellen UI ignoriert (Metadaten gehören nach `index.json`).

### ID-Schema

Der eingebaute Generator erzeugt IDs der Form:

```text
{adjektiv}-{adjektiv}-{strasse}-{nummer}-{suffix}
```

Beispiel: `hoch-steil-nebel-42-a`

So bleiben Dateinamen URL-sicher, sprechbar und kollisionsarm. Für Beiträge an die öffentliche Sammlung bitte diesen Generator (Button in der UI) nutzen und die ID nicht frei erfinden, sofern keine triftigen Gründe dagegen sprechen.

---

## Eigene Bibliothek betreiben

Für private oder Unternehmens-Bibliotheken brauchst du **keinen** Pull Request an dieses Repo.

1. **Fork** oder Kopie der Bibliotheks-Seite (HTML + `lang/` + leeres bzw. eigenes `prompts/`)  
2. Eigene Prompts als `prompts/{id}.json` ablegen  
3. Jeden Eintrag in `prompts/index.json` mit Metadaten listen  
4. Statisch hosten (GitHub Pages, interner Webserver, S3, …)  
5. Optional: im [Prompt Baukasten](https://github.com/juristikde/xml-prompt-baukasten) die Bibliotheks-URL auf eure Instanz zeigen  

Empfehlungen intern:

- Einheitliche `language`-Codes und `updatedAt`  
- `testedWith`, `purpose` und `outputFormat` pflegen – erleichtert Filter und Onboarding  
- Keine Secrets in Prompt-Texten (API-Keys, interne URLs, personenbezogene Daten)  
- Versionierung über Git; bei Bedarf Review-Prozess im eigenen Repo  

Die UI lädt nur relative Pfade unter `prompts/` – absolute Fremd-URLs sind nicht vorgesehen. Wer Kataloge mischen will, merged die JSON-Dateien vor dem Deploy.

---

## Prompts einreichen (GitHub)

Beiträge zur **öffentlichen** Sammlung laufen über Pull Requests. Qualität vor Quantität: ein durchdachter, langer, getesteter Prompt ist mehr wert als viele kurze Skizzen.

### Mindestanforderungen

| Kriterium | Vorgabe |
| --- | --- |
| **Länge** | Prompt-Text (**`prompt`-String**) mindestens **10 000 Zeichen** |
| **Struktur** | Sinnvolles XML (oder klar strukturierter Text), idealerweise im Stil des Prompt Baukastens (`<prompt>`, Rollen, Regeln, Beispiele, …) |
| **Metadaten** | Vollständiger, korrekter Eintrag in `index.json` (siehe Tabelle oben) |
| **ID** | Neu, eindeutig, bevorzugt mit dem ID-Generator der Live-Seite erzeugt |
| **Datei** | `prompts/{id}.json` mit genau einem nutzbaren `prompt`-Feld |
| **Sprache** | `language` muss zum Inhalt passen |
| **Aktualität** | `updatedAt` auf das Einreichungsdatum (UTC-Datum `YYYY-MM-DD`) |
| **Rechte** | Du darfst den Text unter der Repo-Lizenz (MIT) veröffentlichen; keine fremden urheberrechtlich geschützten Volltexte ohne Erlaubnis |

Kürzere Entwürfe bitte nicht als PR an die Hauptbibliothek – nutze dafür eine eigene Instanz oder Issue-Diskussion.

### Checkliste vor dem PR

1. In der Live-Bibliothek **ID erzeugen** und notieren  
2. Prompt im Baukasten bauen, validieren, exportieren  
3. Zeichenzahl prüfen (`prompt`.length ≥ 10000)  
4. Datei anlegen:

   ```bash
   # Beispiel
   cat > prompts/hoch-steil-nebel-42-a.json << 'EOF'
   {
     "prompt": "<prompt>\n  … dein vollständiger Prompt …\n</prompt>"
   }
   EOF
   ```

5. `index.json` ergänzen (Array `prompts` um ein Objekt erweitern, Kommas beachten)  
6. Lokal mit `python3 -m http.server` testen: Eintrag sichtbar, Filter ok, `>_` zeigt den Text, Kopieren funktioniert  
7. PR mit kurzer Beschreibung: Zielgruppe, getestete Modelle, Besonderheiten  

### Beispiel-Metadaten (PR-tauglich)

```json
{
  "id": "hoch-steil-nebel-42-a",
  "title": "Juristischer Recherche-Assistent (DE)",
  "symbol": "⚖️",
  "language": "de",
  "audience": "Legal Tech / Kanzlei-Recherche",
  "purpose": "Strukturierte Fallaufbereitung mit Quellenhinweisen und klarer Trennung von Fakt und Bewertung",
  "outputFormat": "Text",
  "testedWith": "Claude 3.5 Sonnet, GPT-4o",
  "updatedAt": "2026-09-06",
  "autor": "Dein Name oder Handle",
  "keywords": ["recht", "recherche", "deutsch", "xml-prompt"]
}
```

### Was wir ablehnen oder nachbessern lassen

- Prompt unter 10 000 Zeichen ohne Absprache  
- Nur `index.json` ohne `prompts/{id}.json` (oder umgekehrt)  
- Doppelte oder ungültige IDs  
- Leere / Platzhalter-Metadaten (`title`: „test“, fehlende `language`)  
- Offensichtlich maschinell zugespammte oder unsichere Inhalte (Malware-Anleitungen, Betrug, etc.)  
- Kaputtes JSON (Trailing Commas, falsches Encoding)

---

## URL-Parameter

Steuerung über den Hash (wie im Baukasten):

```
#setLanguage=de&promptLineWrap=true
```

| Parameter | Werte | Wirkung |
| --- | --- | --- |
| `setLanguage` | ISO-639-1 | UI-Sprache (`lang/{code}.json`) |
| `promptLineWrap` | `true` / `false` | Zeilenumbruch in der Prompt-Ansicht |

`setLanguage` sollte der erste Hash-Parameter bleiben.

---

## Technik

| Aspekt | Umsetzung |
| --- | --- |
| UI | Eine HTML-Datei, Design analog Prompt Baukasten |
| Katalog | `GET prompts/index.json` |
| Körper | `GET prompts/{id}.json` → Feld `prompt` |
| Liste | Virtuelles Rendering ab Schwellenwert (Desktop-Tabelle / Mobile-Karten) |
| i18n | `lang/*.json`, Keys unter `library.*` |
| Zustand | Kein LocalStorage für den Katalog; frischer Load bei jedem Besuch |

Verwandtes Projekt: **[xml-prompt-baukasten](https://github.com/juristikde/xml-prompt-baukasten)** – Editor zum Erstellen und Pflegen der XML-Struktur.

---

## Mitwirken

- **Bugs / UI:** Issue oder PR an die HTML/`lang`-Dateien  
- **Neue öffentliche Prompts:** PR nach den Regeln unter [Prompts einreichen](#prompts-einreichen-github)  
- **Eigene Sammlungen:** Fork und eigene `prompts/` – kein Zwang, upstream zu mergen  

Bitte UI-Texte bei Änderungen in allen betroffenen Sprachdateien nachziehen; Katalog-Inhalte (Titel, Prompt-Text) werden nicht über i18n übersetzt.

---

## Lizenz

MIT – siehe [`LICENSE`](LICENSE).  
Eingereichte Prompts gelten mit dem PR als unter derselben Lizenz veröffentlicht, sofern im PR nichts Abweichendes vereinbart wird.
