# Usability-Feedback — Priorisierungstabelle

> Alle Punkte aus 6 Testrunden gesammelt, kategorisiert und nach Priorität sortiert.
> **Häufigkeit** = Anzahl Runden, in denen der Punkt aufkam.

---

## Legende

| Priorität | Bedeutung |
|-----------|-----------|
| 🔴 Kritisch | Workflow-Blocker — verhindert Kernfunktionen |
| 🟠 Hoch | Starke Usability-Probleme, häufig genannt |
| 🟡 Mittel | Verbesserungen mit klarem Mehrwert |
| 🟢 Niedrig | Nice-to-Have, selten genannt |

| Umsetzbarkeit | Bedeutung |
|---------------|-----------|
| ✅ Einfach | Wenig Aufwand, 1–2 Dateien |
| ⚙️ Mittel | Überschaubarer Aufwand, mehrere Dateien |
| 🔧 Komplex | Hoher Aufwand, Architektur-Änderungen |

---

## Gesamttabelle

| # | Prio | Status | Problem | Bereich | Häufigkeit | Umsetzbarkeit | Betroffene Datei(en) |
|---|------|--------|---------|---------|-----------|---------------|----------------------|
| P1 | 🔴 Kritisch | ☑️ Erledigt | Linke Navigation-Reiter werden nicht gesehen | Navigation | 5× | ✅ Einfach | `AppLayout.kt` |
| P2 | 🔴 Kritisch | ☑️ Erledigt | Ersteller/Empfänger/Rechnungsposten: gespeicherte Kontakte per Dropdown auswählbar; `+`-Button für neue Einträge; Pflichtfelder im Dialog sichtbar | Erstellen | 4× | ✅ Einfach | `TradePartyField.kt`, `TradePartyDialog.kt`, `ProductField.kt`, `CreateSection.kt` |
| P3 | 🔴 Kritisch | ☑️ Erledigt | Fehlende Pflichtfelder beim Erstellen nicht sichtbar markiert (inkl. USt/Steuer, Währung, Rechnungsposten) | Erstellen | 4× | ✅ Einfach | `CreateSection.kt`, `TradePartyDialog.kt` |
| P4 | 🔴 Kritisch | ☑️ Erledigt | Direkt ins Datumsfeld schreiben nicht möglich — Bleistift-Icon als Blocker | Erstellen | 3× | ✅ Einfach | `DateField.kt`, `DateDialog.kt` |
| P5 | 🟠 Hoch | ☑️ Erledigt | Suchfeld (Strg+F) wird nicht gesehen / nach Klick funktionslos; Strg+F öffnete durch veraltete Key-Handler immer in Visualisierung statt in der aktiven Section — alle drei Legacy-Handler entfernt/korrigiert (main.kt AWT-Dispatcher, Chrome.desktop.kt JCEF-Handler, AppLayout.kt) | Visualisieren / Prüfen | 6× | ⚙️ Mittel | `VisualsSection.kt`, `CheckSection.kt`, `AppLayout.kt`, `AppState.kt`, `Chrome.desktop.kt`, `main.kt` |
| P6 | 🟠 Hoch | ⬜ Offen | Fehler im Prüfen-Bereich werden übersehen — keine Hervorhebung, schlechte Gruppierung | Prüfen | 5× | ⚙️ Mittel | `CheckSection.kt` |
| P7 | 🟠 Hoch | ⬜ Offen | Graue Schrift / schlechter Kontrast — Barrierefreiheitsproblem | Design | 3× | ✅ Einfach | `Color.kt`, `Type.kt` |
| P8 | 🟠 Hoch | ⬜ Offen | Fehlermeldungen gemischt Englisch/Deutsch — inkonsistent | Allgemein | 1× | ⚙️ Mittel | Strings, Validierungsreport |
| P9 | 🟠 Hoch | ☑️ Erledigt | Datei-Auswahl-Button soll mittig sein, nicht oben rechts | Erstellen / Prüfen / Visualisieren | 2× | ✅ Einfach | `CreateSection.kt`, `CheckSection.kt`, `VisualsSection.kt` |
| P10 | 🟡 Mittel | ⬜ Offen | Tab-Navigation im Erstellen-Formular fehlt | Erstellen | 2× | ⚙️ Mittel | `CreateSection.kt` |
| P11 | 🟡 Mittel | ⬜ Offen | Dark Mode: HTML-Bereich bleibt weiß, App ist dunkel | Visualisieren / Prüfen | 1× | ⚙️ Mittel | `WebViewer.kt` |
| P12 | 🟡 Mittel | ☑️ Erledigt | Prüfen-Icon ist Lupe — gleich wie Such-Icon, verwirrend | Navigation | 1× | ✅ Einfach | `AppState.kt` |
| P13 | 🟡 Mittel | ☑️ Erledigt | Währungsauswahl: Euro (€) schwer zu finden — kein Sprung per Buchstabe | Erstellen | 1× | ✅ Einfach | `CurrencyField.kt` |
| P14 | 🟡 Mittel | ☑️ Erledigt | Bleistift-Icon beim Datum — Kalender-Icon erwartet | Erstellen | 1× | ✅ Einfach | `DateField.kt` |
| P15 | 🟡 Mittel | ☑️ Erledigt | Zwischen Visualisieren und Prüfen wechseln ohne Datei neu auswählen — Auto-Sync entfernt; Rechtsklick auf Tab → „In Visualisierung öffnen" / „In Prüfen öffnen" | Navigation | 1× | ⚙️ Mittel | `TabContextMenu.kt`, `CheckSection.kt`, `VisualsSection.kt` |
| P16 | 🟡 Mittel | ⬜ Offen | BT-21 Begriffe in Visualisierung unklar — Tooltips/Erklärungen fehlen | Visualisieren | 1× | ⚙️ Mittel | HTML-Visualisierungs-Template |
| P17 | 🟡 Mittel | ⬜ Offen | Suche durchsucht auch die HTML-Navigationsleiste (Übersicht, Positionen …) | Visualisieren | 1× | ⚙️ Mittel | `WebViewer.kt` |
| P18 | 🟡 Mittel | ⬜ Offen | Fehler sollen über Warnungen stehen (nach Schweregrad sortiert) | Prüfen | 1× | ✅ Einfach | `CheckSection.kt` |
| P19 | 🟡 Mittel | ⬜ Offen | Klick auf Fehler → betroffene Stelle in Visualisierung hervorheben | Prüfen / Visualisieren | 2× | 🔧 Komplex | `CheckSection.kt`, `VisualsSection.kt`, `WebViewer.kt` |
| P20 | 🟡 Mittel | ☑️ Erledigt | Keine Meldung wenn PDF ohne XML (normale PDF) visualisiert wird | Visualisieren | 1× | ✅ Einfach | `VisualsSection.kt` |
| P21 | 🟢 Niedrig | ⬜ Offen | Onboarding-Tutorial beim ersten App-Start | Allgemein | 1× | ⚙️ Mittel | `Preferences.kt` + neues Overlay |
| P22 | 🟢 Niedrig | ⬜ Offen | Viel Leerraum in Prüfen-Zusammenfassung (zwischen Profil und Mitteilung) | Prüfen | 1× | ✅ Einfach | `CheckSection.kt` Layout |
| P23 | 🟢 Niedrig | ⬜ Offen | Rot-Grün-Schwäche nicht berücksichtigt (XML-Visualisierung) | Visualisieren | 1× | ⚙️ Mittel | HTML-Stylesheet |
| P24 | 🟢 Niedrig | ☑️ Erledigt | Info ob Bearbeitungshistorie gespeichert wird fehlt | Visualisieren | 1× | ✅ Einfach | `VisualsSection.kt` |
| P25 | 🟢 Niedrig | ⬜ Offen | Mächtigere Suche gewünscht (Vor/Zurück, Case-Sensitivity, ganze Wörter) | Visualisieren | 1× | ⚙️ Mittel | `VisualsSection.kt` |
| P26 | 🟢 Niedrig | ⬜ Offen | Kurzbefehl: gleicher Tag für Start und Ende des Leistungszeitraums | Erstellen | 1× | ⚙️ Mittel | `DateField.kt`, `CreateSection.kt` |
| P27 | 🟢 Niedrig | ☑️ Erledigt | Klick auf leere Drop-Zone öffnet Datei-Dialog — Mehrfachauswahl möglich (Visualisieren / Prüfen: Multi-Select; Erstellen: Single-Select PDF) | Erstellen / Prüfen / Visualisieren | 1× | ✅ Einfach | `CreateSection.kt`, `CheckSection.kt`, `VisualsSection.kt` |
| P28 | 🟢 Niedrig | ⬜ Offen | Drag-and-Drop: XML-Datei in Erstellen verursacht Fehler ohne Erklärung | Erstellen | 1× | ✅ Einfach | `DragAndDrop.kt` |
| P29 | 🟢 Niedrig | ⬜ Offen | Einfache vs. komplexe Ansicht anbieten | Allgemein | 1× | 🔧 Komplex | Großes UI-Redesign |
| P30 | 🟢 Niedrig | ☑️ Erledigt | Suchfunktion auch im Prüfen-Bereich | Prüfen | 1× | ⚙️ Mittel | `CheckSection.kt`, `CheckSectionState.kt`, `VisualsSection.kt` |
| P31 | 🟢 Niedrig | ☑️ Erledigt | Tabs per Drag & Drop umsortieren — Chrome-Stil: sofortiger Drag-Start (kein langes Drücken), gezogener Tab schwebt über anderen (zIndex + scale), Geschwister-Tabs weichen in Echtzeit animiert aus, Zielposition wird erst beim Loslassen committet | Visualisieren / Prüfen | — | ⚙️ Mittel | `VisualsSection.kt`, `VisualsSectionState.kt`, `CheckSection.kt`, `CheckSectionState.kt` |
| P32 | 🟢 Niedrig | ☑️ Erledigt | Tabs nach App-Neustart wiederherstellen | Visualisieren | — | ⚙️ Mittel | `DokumentTab.kt`, `VisualsSectionState.kt`, `Preferences.kt`, `main.kt` |
| P33 | 🟡 Mittel | ☑️ Erledigt | Multi-Tab auch im Prüfen-Bereich (Chrome-Stil: animierte Breite, + Button, Scrollbar bei Überlauf) | Prüfen | — | ⚙️ Mittel | `CheckSection.kt`, `CheckSectionState.kt` |
| P34 | 🟡 Mittel | ☑️ Erledigt | Rechtsklick-Kontextmenü auf Tabs: Schließen / Andere schließen / Tabs rechts schließen | Prüfen / Visualisieren | — | ⚙️ Mittel | `CheckSection.kt`, `VisualsSection.kt`, `TabContextMenu.kt` |
| P35 | 🟡 Mittel | ☑️ Erledigt | Kontextmenü versteckt sich hinter JCEF-Fenster (z-order) — Fix: natives Swing-Popup | Prüfen / Visualisieren | — | ✅ Einfach | `TabContextMenu.kt` |
| P36 | 🟢 Niedrig | ☑️ Erledigt | „+"-Button klebt immer am rechten Rand — soll direkt neben dem letzten Tab stehen (Chrome-Stil) | Visualisieren / Prüfen | — | ✅ Einfach | `VisualsSection.kt`, `CheckSection.kt` |
| P37 | 🔴 Kritisch | ☑️ Erledigt | Dialog (Ersteller/Empfänger): Buttons am unteren Rand abgeschnitten — Fenster muss manuell vergrößert werden | Erstellen | 2× | ✅ Einfach | `TradePartyDialog.kt` |
| P38 | 🟠 Hoch | ☑️ Erledigt | „Dauerhaft speichern"-Button: deaktivierter Zustand visuell nicht von aktivem zu unterscheiden | Erstellen | 1× | ✅ Einfach | `TradePartyDialog.kt` |
| P39 | 🔴 Kritisch | ☑️ Erledigt | „Erzeugen"-Button erscheint nie, auch wenn alle Pflichtfelder ausgefüllt sind — `AppState.actions()` wurde nirgends aufgerufen | Erstellen | 1× | ✅ Einfach | `CreateSection.kt` |
| P40 | 🟢 Niedrig | ☑️ Erledigt | App-Beenden-Button: falsches Icon (×) — Shutdown-Symbol erwartet | Navigation | 1× | ✅ Einfach | `AppLayout.kt` |
| P41 | 🔴 Kritisch | ☑️ Erledigt | In Einstellungen gespeicherte Kontakte erscheinen nicht im Erstellen-Bereich zur Auswahl — `TradePartyFieldWithAdd` ignorierte die übergebene Kontaktliste | Erstellen | 2× | ✅ Einfach | `TradePartyField.kt` |
| P42 | 🟡 Mittel | ☑️ Erledigt | Sidebar hat keine Auf-/Zuklapp-Funktion — Icon-only und Icon+Beschriftung (Jira-Stil) gewünscht | Navigation | 1× | ⚙️ Mittel | `AppLayout.kt` |
| P43 | 🟠 Hoch | ☑️ Erledigt | Einstellungen: Theme-Auswahl-Buttons sehen immer grau/deaktiviert aus — aktiver Zustand nicht erkennbar | Einstellungen | 1× | ✅ Einfach | `SettingsSection.kt` |
| P44 | 🟡 Mittel | ☑️ Erledigt | Einstellungen: Workspace / Kontakt / E-Rechnung sehen aus wie normale Menüpunkte, nicht wie Kategorieüberschriften | Einstellungen | 1× | ✅ Einfach | `SettingsSection.kt` |
| P45 | 🟡 Mittel | ☑️ Erledigt | Tab-Design einheitlich Chrome-Stil in Visualisieren und Prüfen — Prüfen-Tabs hatten alten Border-Stil (blauer Rand, blauer Hintergrund); beide Bereiche jetzt: grauer Strip-Hintergrund, weißer aktiver Tab mit Schatten, transparente inaktive Tabs, Status-Icon links (Spinner / Haken / ×-je nach Validierungsergebnis), × rechts | Visualisieren / Prüfen | — | ✅ Einfach | `VisualsSection.kt`, `CheckSection.kt` |
| P46 | 🟢 Niedrig | ☑️ Erledigt | „Geteilt"-Label im Ansicht-Umschalter war hardcodiert auf Deutsch — fehlte EN/FR Übersetzung; Sidebar-Buttons „Zuklappen/Aufklappen" ebenfalls hardcodiert | Visualisieren / Navigation | — | ✅ Einfach | `VisualsSection.kt`, `AppLayout.kt`, `AppVisualisation.xml` (DE/EN/FR), `App.xml` (DE/EN/FR) |
| P47 | 🟡 Mittel | ☑️ Erledigt | Theme-Schnellumschalter in der Sidebar — Hell/Auto/Dunkel direkt umschaltbar ohne Umweg über Einstellungen; zugeklappt: einzelnes Icon (zyklisch); aufgeklappt: 3-Segment-Pill | Navigation | — | ✅ Einfach | `AppLayout.kt` |
| P48 | 🟢 Niedrig | ☑️ Erledigt | Leere Drop-Zonen neu gestaltet — zentrierte gestrichelte Box mit Icon und Text statt unsichtbarem Bereich; Drop-Texte mehrsprachig per Ressourcensystem | Erstellen / Prüfen / Visualisieren | — | ✅ Einfach | `CreateSection.kt`, `CheckSection.kt`, `VisualsSection.kt`, `AppCreate.xml`, `AppCheck.xml`, `AppVisualisation.xml` (DE/EN/FR) |

---

## Schnellste Gewinne (Einfach umsetzbar + Kritisch/Hoch)

| # | Problem | Aufwand |
|---|---------|---------|
| P1 | Navigation-Reiter visuell prominenter machen | Styling in `AppLayout.kt` |
| P2 | „+"-Button besser beschriften / erklären | Text-Änderung in `TradePartyField.kt` |
| P3 | Fehlende Felder rot umranden | Logik in `CreateSection.kt` |
| P4 | Kalender-Icon + direkte Texteingabe im Datumsfeld | `DateField.kt` |
| P5 | Suchfeld prominenter anzeigen | Layout in `VisualsSection.kt` |
| P7 | Kontrast erhöhen (WCAG AA) | Farbwerte in `Color.kt` |
| P9 | Datei-Auswahl-Button auch mittig | Layout-Anpassung |
| P12 | Prüfen-Icon → Haken-Symbol | Icon tauschen |
| P14 | Datum-Icon → Kalender | Icon tauschen |
| P18 | Fehler vor Warnungen sortieren | Sortierung in `CheckSection.kt` |
| P20 | Hinweis bei PDF ohne XML | 1 Info-Text in `VisualsSection.kt` |
