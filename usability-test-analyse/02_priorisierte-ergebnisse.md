# Priorisierte Ergebnisse — Usability-Test Analyse Quba

> Grundlage: Feedback aus 6 Testrunden + Häufigkeitstabelle aus `Feedback-Tester.ods`
>
> **Umsetzbarkeit** bezieht sich auf den Aufwand im Kotlin/Compose Multiplatform Codebase.
> - **Hoch** = geringe Komplexität, schnell umsetzbar
> - **Mittel** = überschaubarer Aufwand, möglicherweise mehrere Dateien
> - **Schwer** = hoher Aufwand oder architektonische Abhängigkeiten

---

## Priorität 1 — Kritisch (Workflow-Blocker)

Diese Probleme haben in mehreren Runden den Test-Ablauf blockiert und sind direkt mit der Kernfunktionalität verbunden.

---

### P1 — Linke Navigation-Reiter werden nicht gesehen

**Häufigkeit:** 3× nicht gesehen + 2× falsche Reiterauswahl = **5 Runden betroffen**
**Runden:** 1, 3, 4, 5 (nicht gesehen), 1, 5 (falsch gewählt)

**Problem:**
Die linke Seitenleiste mit den Bereichen „Erzeugen", „Prüfen", „Visualisieren" und „Optionen" wird von Testern häufig übersehen oder nicht als Navigation erkannt. Tester haben daraufhin den falschen Bereich genutzt (z. B. „Erzeugen" statt „Visualisieren").

**Auswirkung:** Tester können die App nicht sinnvoll bedienen, da sie den falschen Workflow starten.

**Umsetzbarkeit:** Hoch
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../AppLayout.kt`

**Mögliche Lösungsansätze:**
- Seitenleiste visuell prominenter gestalten (Beschriftung, Breite, Kontrast)
- Beschriftungen auch bei schmaler Ansicht sichtbar lassen
- Aktiven Bereich deutlicher hervorheben
- Alternativ: Top-Navigation statt Seitennavigation prüfen

---

### P2 — Ersteller/Empfänger Hinzufügen-Workflow unklar

**Häufigkeit:** **4 Runden betroffen** (häufigster Einzelpunkt)
**Runden:** 1, 3, 5, 6

**Problem:**
Tester verstehen nicht, dass Ersteller und Empfänger zuerst über den „+"-Button als Dialog angelegt werden müssen. Stattdessen versuchen sie, Daten direkt in das Dropdown-Feld zu schreiben. Eingegebene Werte verschwinden dann. Das „+"-Symbol wird erst nach einiger Zeit entdeckt oder gar nicht.

**Auswirkung:** Kernworkflow „Rechnung erstellen" wird blockiert — ohne Ersteller/Empfänger ist keine E-Rechnung erstellbar.

**Umsetzbarkeit:** Hoch
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../gui/TradePartyField.kt`
- `manager/src/commonMain/kotlin/.../sections/CreateSection.kt`

**Mögliche Lösungsansätze:**
- „+"-Button durch ausgeschriebenen Button „Neu anlegen" ersetzen oder ergänzen
- Placeholder-Text im Dropdown verbessern (z. B. „Ersteller auswählen oder neu anlegen")
- Tooltip / Hinweistext beim leeren Zustand

---

### P3 — Fehlende Pflichtfelder beim Erstellen nicht visuell markiert

**Häufigkeit:** 3× explizit + mehrfach indirekt genannt
**Runden:** 1, 3, 4, 5, 6

**Problem:**
Tester wissen nicht, welche Felder fehlen. Die orangefarbene Warnleiste am unteren Rand wird wahrgenommen, aber sie nennt nicht konkret welche Felder unvollständig sind. Tester möchten sehen, welche Felder rot markiert oder hervorgehoben sind.

**Auswirkung:** Tester können Rechnung nicht fertigstellen, weil sie nicht wissen, was noch fehlt.

**Umsetzbarkeit:** Hoch
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../sections/CreateSection.kt`
- `manager/src/commonMain/kotlin/.../sections/CreateSectionState.kt`
- `manager/src/commonMain/kotlin/.../gui/NotificationBar.kt`

**Mögliche Lösungsansätze:**
- Fehlende Pflichtfelder rot umranden / hervorheben
- Warnleiste konkret auflisten, welche Felder fehlen
- Fehlende Felder beim Versuch „Erzeugen" zu klicken anzeigen (falls Button noch nicht sichtbar ist)

---

### P4 — Direkt ins Datumsfeld schreiben nicht möglich

**Häufigkeit:** **3 Runden** (explizit 3× + 1× Wunsch nach Kalender-Icon)
**Runden:** 1, 4, 6

**Problem:**
Tester versuchen wiederholt, das Datum direkt in das Datumsfeld einzutippen. Die aktuelle Implementierung erfordert einen Klick auf das Bleistift-Symbol, das einen Dialog öffnet. Diese Interaktion widerspricht der Nutzererwartung aus anderen Anwendungen. Zusätzlich ist das Bleistift-Icon inkonsistent positioniert (mal innen, mal außen).

**Auswirkung:** Frustrierendes Erlebnis bei einem häufig genutzten Feld.

**Umsetzbarkeit:** Hoch
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../gui/DateField.kt`
- `manager/src/commonMain/kotlin/.../gui/DateDialog.kt`

**Mögliche Lösungsansätze:**
- Direkte Texteingabe im Datumsfeld ermöglichen (mit Validierung)
- Alternativ: Kalender-Icon statt Bleistift-Icon (intuitiver für Datumseingabe)
- Konsistente Positionierung des Icons (immer innen oder immer außen)

---

## Priorität 2 — Hoch (Usability & Klarheit)

---

### P5 — Fehler im Prüfen-Bereich: Hervorhebung und Gruppierung mangelhaft

**Häufigkeit:** 3× (Highlighting, Gruppierung, zu technisch) + 2× Klick auf Fehler = **5 Runden**
**Runden:** 1, 2, 3, 5, 6

**Problem:**
Fehler und Hinweise in der Prüfung werden übersehen. Tester möchten beim Klicken auf einen Fehler zur betroffenen Stelle in der Visualisierung springen. Zudem werden Fehlermeldungen als zu technisch empfunden, und Tester wünschen sich Fehler über Warnungen (nach Schweregrad sortiert).

**Umsetzbarkeit:** Mittel (Sortierung einfach; Click-to-highlight schwerer)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../sections/CheckSection.kt`

**Mögliche Lösungsansätze:**
- Fehler standardmäßig über Warnungen sortieren (nach Schweregrad)
- Einfachere/deutschere Fehlerbeschreibungen oder Tooltip-Erklärungen
- Klick auf Fehler → XML-Element hervorheben (Cross-Component, komplexer Aufwand)

---

### P6 — Design und Kontrast: Graue Schrift, Barrierefreiheit

**Häufigkeit:** **3 Runden**
**Runden:** 2, 3, 6

**Problem:**
Graue Schrift auf weißem Hintergrund ist schwer lesbar. Das allgemeine Design wird als unschön empfunden. Es gibt Zugänglichkeitsprobleme (Kontrast, Schriftgröße). Auch das Logo-Design wird als ungewohnt wahrgenommen (weißer Schriftzug + doppelter „Quba"-Text).

**Umsetzbarkeit:** Hoch (Farbwerte ändern)
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../theme/Color.kt`
- `manager/src/commonMain/kotlin/.../theme/Type.kt`

**Mögliche Lösungsansätze:**
- Kontrast der Texte erhöhen (WCAG AA-Standard: min. 4.5:1 für normalen Text)
- Graue Texte durch dunklere Grautöne ersetzen
- Logo-Dopplung prüfen und ggf. bereinigen

---

### P7 — Suchfeld nach Strg+F nicht sichtbar / auffindbar

**Häufigkeit:** **6 Runden** (alle Tester haben Strg+F versucht, aber das Suchfeld übersehen)
**Runden:** 1, 2, 3, 4, 5, 6

**Problem:**
Alle Tester haben Strg+F intuitiv genutzt. Das Suchfeld öffnet sich jedoch nicht prominent genug — Tester sehen es nicht oder merken nicht, dass die Suche aktiv ist. Zusätzlich gibt es einen Bug: Nach Klick auf PDF/XML kann man mit Strg+F nicht mehr suchen, obwohl das Suchfeld scheinbar noch aktiv ist.

**Umsetzbarkeit:** Hoch (UI-Darstellung) / Mittel (Bug-Fix)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`

**Mögliche Lösungsansätze:**
- Suchfeld prominenter platzieren oder animiert einblenden
- Visuelles Feedback wenn Suche aktiv ist (z. B. Hervorhebung, Zähler „3 von 12")
- Bug fixen: Fokus-Management nach Klick auf PDF/XML-Bereich

---

### P8 — Fehlermeldungen inkonsistent: Englisch und Deutsch gemischt

**Häufigkeit:** 1× explizit (Runde 4)
**Runden:** 4

**Problem:**
Fehlermeldungen kommen teils auf Englisch (aus der Mustang-Validierungsbibliothek), teils auf Deutsch (eigene App-Meldungen). Das wirkt unprofessionell und ist für Nicht-Entwickler schwer verständlich.

**Umsetzbarkeit:** Mittel (Bibliotheks-Meldungen können nicht immer übersetzt werden)
**Betroffene Dateien:** Strings-Dateien, Validierungsreport

**Mögliche Lösungsansätze:**
- Eigene App-Texte konsequent auf Deutsch (oder Systemsprache)
- Englische Bibliotheks-Meldungen mit deutschem Kontext/Erklärung versehen
- Tooltip / Erklärungstext zu technischen Fehlercodes

---

### P9 — Datei-Auswahl-Button soll mittig im Bereich stehen

**Häufigkeit:** 2 Runden
**Runden:** 2, 5

**Problem:**
Der Button zur Dateiauswahl befindet sich oben rechts. Tester erwarten ihn in der Mitte des leeren Bereichs (wie bei modernen File-Drop-Zonen üblich).

**Umsetzbarkeit:** Mittel
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../sections/CreateSection.kt`
- `manager/src/commonMain/kotlin/.../sections/CheckSection.kt`
- `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`

**Mögliche Lösungsansätze:**
- Button zusätzlich in der Mitte der Leerzone platzieren (neben Drag-and-Drop-Text)
- Doppelklick auf Leerzone zum Öffnen des Dateidialogs

---

## Priorität 3 — Mittel (Verbesserungen)

---

### P10 — Tab-Navigation in Erstellen fehlt

**Häufigkeit:** 2 Runden
**Runden:** 3, 6

**Problem:** Nutzer können im Erstellen-Formular nicht mit der Tab-Taste zwischen Feldern navigieren. Das ist eine Standard-Erwartung bei Formularen.

**Umsetzbarkeit:** Mittel (Compose `focusable`/`focusOrder` API)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../sections/CreateSection.kt`

---

### P11 — Dark Mode: HTML-Bereich und App inkonsistent

**Häufigkeit:** 1 Runde
**Runden:** 2

**Problem:** Bei aktivem Dark Mode wird der obere App-Bereich dunkel dargestellt, der HTML-Visualisierungsbereich (eingebetteter Browser, JCEF) bleibt weiß.

**Umsetzbarkeit:** Mittel (JCEF-Browser benötigt eigene Dark-Mode-Behandlung)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../gui/WebViewer.kt`

---

### P12 — Prüfen-Icon ist Lupe (gleich wie Suche-Icon)

**Häufigkeit:** 1 Runde
**Runden:** 6

**Problem:** Sowohl das „Prüfen"-Symbol als auch das „Suchen"-Symbol sind Lupen. Das irritiert, da ein Symbol zwei verschiedene Bedeutungen hat. Tester schlagen ein Haken-Symbol (✓) für „Prüfen" vor.

**Umsetzbarkeit:** Hoch (nur Icon austauschen)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../AppLayout.kt` oder Icon-Ressourcen

---

### P13 — Währungsauswahl: Sprung per Anfangsbuchstabe fehlt

**Häufigkeit:** 1 Runde
**Runden:** 1

**Problem:** Euro (€) erscheint ganz unten in der Währungsliste (nach Sonderzeichen sortiert), statt bei „E". Tester suchen lange nach Euro.

**Umsetzbarkeit:** Hoch (`AutoCompleteField.kt` bereits vorhanden)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../gui/CurrencyField.kt`

---

### P14 — Bleistift-Icon vs. Kalender-Icon beim Datum

**Häufigkeit:** 1 Runde
**Runden:** 4

**Problem:** Das Bleistift-Symbol beim Datumsfeld ist unintuitiv. Nutzer erwarten ein Kalender-Symbol für Datumseingabe.

**Umsetzbarkeit:** Hoch (nur Icon austauschen)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../gui/DateField.kt`

---

### P15 — Zwischen Visualisieren und Prüfen wechseln ohne Datei neu auswählen

**Häufigkeit:** 1 Runde
**Runden:** 5

**Problem:** Wenn man eine Datei in „Visualisieren" geöffnet hat und zu „Prüfen" wechselt, muss die Datei erneut ausgewählt werden.

**Umsetzbarkeit:** Mittel (AppState / SectionState zwischen Bereichen teilen)
**Betroffene Dateien:**
- `manager/src/commonMain/kotlin/.../utils/SectionState.kt`
- App-State-Management

---

### P16 — BT-21 Begriffe unklar (fehlende Tooltips/Erklärungen)

**Häufigkeit:** 1 Runde
**Runden:** 2

**Problem:** Technische Begriffe wie „BT-21" in der HTML-Visualisierung sind für Nicht-Fachleute unverständlich. Tooltips oder Erklärungen fehlen.

**Umsetzbarkeit:** Mittel (HTML-Visualisierung / Stylesheet anpassen)
**Betroffene Datei:** HTML-Visualisierungs-Template

---

### P17 — Suche durchsucht auch die Navigationsleiste der HTML-Ansicht

**Häufigkeit:** 1 Runde
**Runden:** 6

**Problem:** Beim Suchen in der HTML-Visualisierung werden auch die Navigationspunkte („Übersicht", „Positionen", etc.) als Treffer markiert. Das ist störend.

**Umsetzbarkeit:** Mittel (JavaScript/CSS im WebViewer anpassen)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../gui/WebViewer.kt`

---

### P18 — Fehler sollen über Warnungen stehen im Prüfen-Bereich

**Häufigkeit:** 1 Runde
**Runden:** 5

**Problem:** Fehler und Warnungen sind nicht nach Schweregrad geordnet. Tester erwarten, dass kritische Fehler oben stehen.

**Umsetzbarkeit:** Hoch (Sortierung der Liste anpassen)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../sections/CheckSection.kt`

---

### P19 — Klick auf Fehler → Stelle in Visualisierung hervorheben

**Häufigkeit:** 2 Runden
**Runden:** 1, 5

**Problem:** Tester erwarten, dass ein Klick auf einen Fehler/Hinweis im Prüfbericht zur betroffenen Stelle in der Visualisierung oder im XML springt.

**Umsetzbarkeit:** Schwer (Cross-Component-Kommunikation zwischen CheckSection und WebViewer)
**Betroffene Dateien:** `CheckSection.kt`, `VisualsSection.kt`, `WebViewer.kt`

---

### P20 — Meldung wenn PDF ohne XML visualisiert wird

**Häufigkeit:** 1 Runde
**Runden:** 5

**Problem:** Wenn eine normale PDF (ohne eingebettetes XML) in „Visualisieren" geöffnet wird, zeigt Quba nur die PDF ohne Erklärung. Nutzer verstehen den Unterschied zur E-Rechnung nicht.

**Umsetzbarkeit:** Hoch (Hinweismeldung hinzufügen)
**Betroffene Datei:** `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`

---

## Priorität 4 — Niedrig (Nice-to-Have)

| # | Problem | Runden | Umsetzbarkeit | Betroffene Datei(en) |
|---|---------|--------|---------------|----------------------|
| P21 | Onboarding-Tutorial beim ersten Start | 5 | Mittel | `Preferences.kt` + neues Overlay |
| P22 | Viel Leerraum in Prüfen-Zusammenfassung | 5 | Hoch | `CheckSection.kt` Layout |
| P23 | Rot-Grün-Schwäche bei XML-Farben | 6 | Mittel | HTML-Stylesheet |
| P24 | Info: Wird Bearbeitungshistorie gespeichert? | 3 | Hoch | Tooltip/Info-Text in `VisualsSection.kt` |
| P25 | Mächtigere Suche (Vor/Zurück, Case-Sensitivity) | 6 | Mittel | `VisualsSection.kt` |
| P26 | Kurzbefehl: gleicher Start/Ende-Tag beim Leistungszeitraum | 5 | Mittel | `DateField.kt` / `CreateSection.kt` |
| P27 | Doppelklick auf Leerzone → Datei auswählen | 6 | Hoch | `CreateSection.kt`, `CheckSection.kt` |
| P28 | Drag-and-Drop: XML-Dateien in Erstellen abfangen und erklären | 3 | Hoch | `DragAndDrop.kt` |
| P29 | Einfache vs. komplexe Ansicht | 2 | Schwer | Großes Redesign |
| P30 | Suche auch im Prüfen-Bereich | 6 | Mittel | `CheckSection.kt` + `WebViewer.kt` |

---

## Zusammenfassung

### Schnellste Gewinne (Hoch umsetzbar + kritisch/hoch)

| Priorität | Problem | Warum schnell? |
|-----------|---------|----------------|
| P1 | Navigation-Reiter unsichtbar | Nur CSS/Styling |
| P3 | Pflichtfelder nicht markiert | Rote Umrandung hinzufügen |
| P4 | Datumsfeld: direkte Eingabe | Icon + Eingabe-Logik |
| P6 | Kontrast verbessern | Farbwerte in `Color.kt` |
| P12 | Prüfen-Icon tauschen | Nur Icon-Ressource |
| P14 | Kalender-Icon statt Bleistift | Nur Icon-Ressource |
| P18 | Fehler über Warnungen | Sortierung |
| P20 | Meldung bei PDF ohne XML | 1 Hinweistext |

### Komplexeste Probleme

| Problem | Warum komplex? |
|---------|---------------|
| P19 (Klick auf Fehler → Visualisierung) | Cross-Component, Browser-App-Kommunikation |
| P11 (Dark Mode HTML) | JCEF-Browser eigene Theming-API nötig |
| P29 (Einfache vs. komplexe Ansicht) | Komplettes UI-Redesign |
| P15 (Datei zwischen Bereichen teilen) | Zustandsverwaltung über Sektionen hinweg |
