# Umsetzungsstand — Usability-Fixes Quba

> Dieses Dokument erfasst alle umgesetzten Änderungen aus den Usability-Tests.
> Tabelle mit allen offenen Punkten: [`03_tabelle-priorisierung.md`](03_tabelle-priorisierung.md)

---

## Erledigte Punkte

### ☑️ P1 — Navigation-Reiter visuell trennen
**Problem:** Linke Navigation-Reiter wurden in 5 von 6 Testrunden nicht gesehen.

**Lösung:** Trennlinie (`VerticalDivider`) zwischen Navigationsleiste und Hauptinhalt eingefügt.

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/AppLayout.kt`
  - Import `VerticalDivider` hinzugefügt
  - `VerticalDivider()` zwischen `AppNavigation()` und `AppContent()` eingefügt

---

### ☑️ P2 — Ersteller/Empfänger-Dialog: Pflichtfelder kennzeichnen
**Problem:** Tester wussten nicht, welche Felder im „Ersteller bearbeiten"-Dialog ausgefüllt werden müssen, damit der „E-Rechnung erzeugen"-Button erscheint. Tester schrieben direkt ins Auswahlfeld statt den `+`-Dialog zu nutzen.

**Lösung:**
1. Hinweistext unter dem Auswahlfeld (solange kein Eintrag gewählt): „Mit + einen neuen Eintrag anlegen."
2. Im Dialog selbst:
   - **Name** mit `*`-Pflichtmarkierung versehen (immer)
   - **USt-Id-Nr** und **Steuer-Nr** mit `*`-Pflichtmarkierung versehen (nur für Ersteller, nicht für Empfänger)
   - Hinweistext unter den Feldern: „* Mindestens eine der beiden Angaben ist erforderlich."
3. `isCustomer`-Parameter durch `TradePartyFieldWithAdd` → `TradePartyDialog` weitergereicht
4. Empfänger-Feld in `CreateSection.kt` als `isCustomer = true` markiert (keine USt/Steuer-Pflicht)

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/TradePartyField.kt`
  - Eingabefeld ersetzt durch read-only `MaterialTextField` mit `MutableInteractionSource` + `collectIsPressedAsState` — Klick auf das Feld öffnet denselben Dialog wie der `+`-Button
  - Ausgewählter Eintrag zeigt Namen + Clear-Button (`Icons.Default.Clear`)
  - Parameter `isCustomer: Boolean = false` zu `TradePartyFieldWithAdd` hinzugefügt
  - `isCustomer` an `TradePartyDialog` weitergegeben
  - Hinweistext-Block unterhalb des Auswahlfeldes ergänzt (bei leerem Eintrag)
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/TradePartyDialog.kt`
  - Import `AppTradePartyDialogGeneralTaxHint` hinzugefügt
  - Name-Feld: `requiredIndicator = true`
  - USt-Id-Nr-Feld: `requiredIndicator = !isCustomer`
  - Steuer-Nr-Feld: `requiredIndicator = !isCustomer`
  - Hinweistext unter VAT/Tax-Zeile (nur wenn `!isCustomer`)
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/sections/CreateSection.kt`
  - Empfänger-Feld: `isCustomer = true` gesetzt
- `manager/src/commonMain/composeResources/values/App.xml`
  - `AppTradePartyFieldHint` = „Use + to add a new entry."
  - `AppTradePartyDialogGeneralTaxHint` = „* At least one of the two is required."
- `manager/src/commonMain/composeResources/values-de/App.xml`
  - `AppTradePartyFieldHint` = „Mit + einen neuen Eintrag anlegen."
  - `AppTradePartyDialogGeneralTaxHint` = „* Mindestens eine der beiden Angaben ist erforderlich."
- `manager/src/commonMain/composeResources/values-fr/App.xml`
  - `AppTradePartyFieldHint` = „Utilisez + pour ajouter une nouvelle entrée."
  - `AppTradePartyDialogGeneralTaxHint` = „* Au moins l'une des deux informations est requise."

---

### ☑️ P3 — Fehlende Pflichtfelder benennen
**Problem:** Wenn der „E-Rechnung erzeugen"-Button nicht erscheint, war unklar welche Felder noch fehlen.

**Lösung:** Fehlermeldung im Erstellen-Bereich listet nun die konkreten fehlenden Felder namentlich auf, z. B. „Fehlend: Ersteller, Lieferdatum".

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/sections/CreateSection.kt`
  - Import `AppCreateErrorInvalidFields` + `AppCreateErrorSenderTaxMissing` hinzugefügt
  - `NotificationBar` zeigt dynamisch fehlende Felder inkl. Fall: Ersteller gesetzt aber USt-Id-Nr und Steuer-Nr beide leer
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/TradePartyDialog.kt`
  - `isTradePartyValid` berechnet: Name nicht leer + (isCustomer ODER vatID/taxID nicht leer)
  - „Übernehmen"-Button ist `enabled = isTradePartyValid` — blockiert Speichern ohne Pflichtfelder
- `manager/src/commonMain/composeResources/values/AppCreate.xml`
  - `AppCreateErrorInvalidFields` = „Missing: %1$s"
  - `AppCreateErrorSenderTaxMissing` = „VAT/Tax-ID (creator)"
- `manager/src/commonMain/composeResources/values-de/AppCreate.xml`
  - `AppCreateErrorInvalidFields` = „Fehlend: %1$s"
  - `AppCreateErrorSenderTaxMissing` = „USt-Id-Nr / Steuer-Nr (Ersteller)"
- `manager/src/commonMain/composeResources/values-fr/AppCreate.xml`
  - `AppCreateErrorInvalidFields` = „Manquant : %1$s"
  - `AppCreateErrorSenderTaxMissing` = „N° TVA / N° fiscal (émetteur)"

---

### ☑️ P4 + P14 — Datumsfeld direkt anklickbar + Kalender-Icon
**Problem:** Tester versuchten direkt ins Datumsfeld zu schreiben. Das Bleistift-Icon war unintuitiv — ein Kalender-Icon wurde erwartet. Das Datum-Feld öffnete nur beim Klick auf das Icon, nicht beim Klick auf das Feld selbst.

**Lösung:**
1. Klick auf das Textfeld (nicht nur auf das Icon) öffnet jetzt den Kalender-Dialog
2. Icon geändert von `Edit` (Bleistift) auf `DateRange` (Kalender)

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/DateField.kt`
  - Imports: `MutableInteractionSource`, `collectIsPressedAsState`, `LaunchedEffect` hinzugefügt
  - Import `Icons.Default.Edit` entfernt, `Icons.Default.DateRange` hinzugefügt
  - `interactionSource` mit `collectIsPressedAsState` + `LaunchedEffect` ergänzt — öffnet Dialog bei Klick auf das Feld
  - `TextField` erhält `interactionSource = interactionSource`
  - Icon geändert zu `Icons.Default.DateRange`

---

### ☑️ P5 — Suchfeld (Strg+F) immer erreichbar + ESC zum Schließen
**Problem:** Nutzer sahen das Such-Icon nicht, da es nur als kleines Icon in der Aktionsleiste erscheint. Nach Klick auf den PDF- oder HTML-Viewer (JCEF) verlor Compose den Keyboard-Focus — Ctrl+F funktionierte danach nicht mehr. ESC zum Schließen fehlte.

**Lösung:**
1. Globaler AWT `KeyEventDispatcher` in `VisualsSection` via `DisposableEffect` — fängt Ctrl+F und ESC auf AWT-Ebene ab, bevor JCEF sie verarbeitet
2. ESC-Handler direkt in `CompactSearchBar` für den Fall, dass Compose-Focus vorhanden ist

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/sections/VisualsSection.kt`
  - Imports: `KeyboardFocusManager`, `KeyEventDispatcher`, `DisposableEffect` hinzugefügt
  - `DisposableEffect(Unit)` mit globalem AWT-KeyListener: Ctrl+F → `state.isSearchOpen = true`, ESC → `state.isSearchOpen = false`
  - Listener wird in `onDispose` sauber deregistriert
  - `CompactSearchBar`: ESC-Fall in `onPreviewKeyEvent` ergänzt → ruft `onClose()` auf

---

### ☑️ P12 — Prüfen-Icon geändert (Lupe → Haken)
**Problem:** Das Icon für den „Prüfen"-Bereich war eine Lupe (`Search`) — identisch mit dem Such-Icon. Nutzer verwechselten die beiden Bedeutungen.

**Lösung:** Icon für `CHECK`-Bereich von `Icons.Default.Search` auf `Icons.Default.CheckCircle` geändert.

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/AppState.kt`
  - Import `Icons.Default.Search` durch `Icons.Default.CheckCircle` ersetzt
  - `activeIcon` und `inactiveIcon` für `CHECK` auf `Icons.Default.CheckCircle` gesetzt

---

### ☑️ P13 — Währungsauswahl: Tippen zum Filtern
**Problem:** Die Währungsauswahl war ein einfaches Dropdown — ohne Filtermöglichkeit. Euro (€) war am Ende der Liste, da es nach Symbol sortiert wurde, nicht nach Name.

**Lösung:** `CurrencyField` von `DropDown` auf `AutoCompleteField` umgestellt. Nutzer können jetzt „Euro" oder „EUR" tippen, die Liste filtert sich sofort.

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/CurrencyField.kt`
  - `DropDown(...)` ersetzt durch `AutoCompleteField(...)` mit identischen Parametern
  - `entry = currency` statt `value = currency` (AutoCompleteField-API)

---

### ☑️ P2-Erweiterung — Rechnungsposten-Feld direkt anklickbar
**Problem:** Das Eingabefeld für den Rechnungsposten (Artikel/Position) öffnete den Dialog nur über den `+`-Button. Nutzer erwarteten dasselbe Verhalten wie bei Ersteller/Empfänger.

**Lösung:** `ProductFieldWithAdd` wurde analog zu `TradePartyFieldWithAdd` umgebaut:
1. AutoCompleteField ersetzt durch read-only `MaterialTextField` mit `MutableInteractionSource` + `collectIsPressedAsState`
2. Klick auf das Feld öffnet denselben Dialog wie der `+`-Button
3. Bei gewähltem Produkt: Name anzeigen + Clear-Button (`Icons.Default.Clear`)
4. Bei leerem Feld: Hinweistext „Mit + einen neuen Eintrag anlegen."

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/gui/ProductField.kt`
  - Imports: `MutableInteractionSource`, `collectIsPressedAsState`, `LaunchedEffect`, `Column`, `MaterialTextField`, `Icons.Default.Clear`, `PointerIcon`, `pointerHoverIcon`, `padding`, `stringResource`, `AppTradePartyFieldHint` hinzugefügt
  - `ProductFieldWithAdd`: `AutoCompleteField` ersetzt durch read-only `MaterialTextField` mit `fieldInteractionSource`
  - `LaunchedEffect(isFieldPressed)` öffnet Dialog bei Klick auf das Feld
  - Clear-Button wenn Produkt ausgewählt
  - Hinweistext wenn kein Produkt ausgewählt

---

### ☑️ P3-Erweiterung — Währung und Rechnungsposten in Hinweisbox
**Problem:** Die Hinweisbox zeigte nicht, wenn Währung oder Rechnungsposten fehlen. Außerdem erschien die Box nicht, wenn alle Pflichtfelder gefüllt waren aber noch keine Rechnungsposten hinterlegt wurden.

**Lösung:**
1. Bedingung für Sichtbarkeit der Hinweisbox erweitert: `(!isValid || state.invoiceItems.isEmpty()) && isSelectedPdfArchiveUsable`
2. `Währung` in `missingFields` ergänzt (wenn `state.invoiceCurrency == null`)
3. `Rechnungsposten` in `missingFields` ergänzt (wenn `state.invoiceItems.isEmpty()`)
4. Neue String-Ressourcen in allen 3 Sprachvarianten

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/sections/CreateSection.kt`
  - Imports: `AppCreateErrorCurrencyMissing`, `AppCreateErrorItemsMissing` hinzugefügt
  - `AnimatedVisibility`-Bedingung: `!isValid` → `(!isValid || state.invoiceItems.isEmpty())`
  - `missingFields` um Währung- und Rechnungsposten-Prüfung erweitert
- `manager/src/commonMain/composeResources/values/AppCreate.xml`
  - `AppCreateErrorCurrencyMissing` = „currency"
  - `AppCreateErrorItemsMissing` = „items"
- `manager/src/commonMain/composeResources/values-de/AppCreate.xml`
  - `AppCreateErrorCurrencyMissing` = „Währung"
  - `AppCreateErrorItemsMissing` = „Rechnungsposten"
- `manager/src/commonMain/composeResources/values-fr/AppCreate.xml`
  - `AppCreateErrorCurrencyMissing` = „devise"
  - `AppCreateErrorItemsMissing` = „postes de facture"

---

### ☑️ P20 — Hinweis bei PDF ohne ZUGFeRD-XML
**Problem:** Wenn eine normale PDF (ohne eingebettetes E-Rechnung-XML) im Visualisieren-Bereich geöffnet wird, erscheinen weder HTML-Vorschau noch XML-Tab. Der Nutzer sieht nur die PDF und weiß nicht, warum nichts passiert.

**Lösung:** In `CurrentTabContent` wird ein `NotificationBar` über dem PDF-Viewer angezeigt, sobald `!hasCode` gilt (kein XML und kein HTML gefunden, kein Ladevorgang aktiv).

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`
  - `ViewMode.PDF_ONLY` Case umgebaut: nun in `Column`, oben `NotificationBar` wenn `!hasCode`, darunter `PdfViewer` mit `weight(1f)`
- `manager/src/commonMain/composeResources/values/AppVisualisation.xml` — `AppVisualisationNoXml` ergänzt
- `manager/src/commonMain/composeResources/values-de/AppVisualisation.xml` — DE-Übersetzung ergänzt

---

### ☑️ P24 — Info: Geöffnete Dokumente werden nicht gespeichert
**Problem:** Nutzer fragten, ob geöffnete Tabs/Dokumente beim Beenden der App erhalten bleiben. Die Antwort ist Nein — es wird nur der zuletzt verwendete Ordner gespeichert, nicht die Dokumente selbst. Es gab keinen Hinweis darauf.

**Lösung:** In `VisualsSectionActions` ein Info-Icon (`Icons.Default.Info`) mit `Tooltip` ergänzt. Das Icon erscheint neben dem Such-Icon, wenn mindestens ein Dokument geöffnet ist.

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`
  - Imports: `Icons.Default.Info`, `AppVisualisationSessionInfo`, `Tooltip` hinzugefügt
  - `VisualsSectionActions`: Info-Icon mit Tooltip nach dem Such-Icon eingefügt
- `manager/src/commonMain/composeResources/values/AppVisualisation.xml` — `AppVisualisationSessionInfo` ergänzt
- `manager/src/commonMain/composeResources/values-de/AppVisualisation.xml` — DE-Übersetzung ergänzt

---

### ☑️ P30 — Suchfunktion im Prüfen-Bereich
**Problem:** Im Prüfen-Bereich gibt es rechts HTML- und XML-Viewer, aber keine Suchleiste. Die Viewer-Komponenten (`WebViewer`, `XmlViewer`) unterstützen technisch bereits eine Suche — sie wurde nur nicht verdrahtet.

**Lösung:** Gleiche Suche wie in Visualisieren — Strg+F öffnet, ESC schließt, Eingabe springt zum nächsten Treffer.

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/.../sections/CheckSectionState.kt`
  - Imports: `getValue`, `setValue` ergänzt
  - Neue Felder: `isSearchOpen`, `searchQuery`, `searchSequence`
- `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt`
  - `CompactSearchBar`: `private` → `internal` (für Nutzung in `CheckSection.kt`)
- `manager/src/commonMain/kotlin/.../sections/CheckSection.kt`
  - Imports: `DisposableEffect`, `LaunchedEffect`, `IconButton`, `Icons.Default.Search`, `width`, `KeyboardFocusManager`, `KeyEventDispatcher` ergänzt
  - `CheckSection`: `LaunchedEffect` zum Zurücksetzen der Suche + `DisposableEffect` für globalen AWT-Key-Listener (Strg+F / ESC)
  - `CheckSectionActions`: Such-Icon + `CompactSearchBar` ergänzt (nur wenn PDF geladen)
  - `DetailsView`: `activeSearch` berechnet, an `WebViewer(search=)` und `XmlViewer(search=)` übergeben

---

---

### ☑️ P9 — Leere Drop-Zonen zentriert + visuell sichtbar

**Problem:** Der Datei-Auswahl-Button war oben rechts in der Toolbar — nicht im Sichtfeld der leeren Inhaltsfläche. Der leere Bereich gab keinen Hinweis, dass er anklickbar oder per Drag & Drop bedienbar ist.

**Lösung:** In allen drei Bereichen (Visualisieren, Prüfen, Erstellen) wurde die leere Fläche durch eine zentrierte gestrichelte Drop-Zone ersetzt:
- Gestrichelte Box (104 × 104 dp) mit abgerundeten Ecken via `drawBehind { drawRoundRect(..., style = Stroke(pathEffect = PathEffect.dashPathEffect(...))) }`
- Bereichsspezifisches Icon (48 dp, Akzentfarbe): `Visibility` (Visualisieren), `CheckCircle` (Prüfen), `Edit` (Erstellen)
- Zwei Textzeilen (Titel + Hinweis) aus dem String-Ressourcen-System (DE/EN/FR)
- Klick auf den gesamten Bereich öffnet Datei-Dialog — Mehrfachauswahl in Visualisieren und Prüfen via `rememberFilePickerLauncher(PickerMode.Multiple())`

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/.../sections/VisualsSection.kt` — `EmptyVisualsView` neu implementiert; `emptyAreaLauncher` mit Multi-Select
- `manager/src/commonMain/kotlin/.../sections/CheckSection.kt` — `EmptyView` neu implementiert; `emptyAreaLauncher` mit Multi-Select
- `manager/src/commonMain/kotlin/.../sections/CreateSection.kt` — `EmptyView` neu implementiert; Single-Select PDF
- `manager/src/commonMain/composeResources/values/AppVisualisation.xml` — `AppVisualisationEmptyTitle`, `AppVisualisationEmptyHint`
- `manager/src/commonMain/composeResources/values-de/AppVisualisation.xml` — DE-Übersetzungen
- `manager/src/commonMain/composeResources/values-fr/AppVisualisation.xml` — FR-Übersetzungen
- `manager/src/commonMain/composeResources/values/AppCheck.xml` — `AppCheckEmptyTitle`, `AppCheckEmptyHint`
- `manager/src/commonMain/composeResources/values-de/AppCheck.xml` — DE-Übersetzungen
- `manager/src/commonMain/composeResources/values-fr/AppCheck.xml` — FR-Übersetzungen
- `manager/src/commonMain/composeResources/values/AppCreate.xml` — `AppCreateEmptyTitle`, `AppCreateEmptyHint`
- `manager/src/commonMain/composeResources/values-de/AppCreate.xml` — DE-Übersetzungen
- `manager/src/commonMain/composeResources/values-fr/AppCreate.xml` — FR-Übersetzungen

---

### ☑️ P47 (neu) — Theme-Schnellumschalter in der Sidebar

**Problem:** Hell/Dunkel/Auto umschalten war nur in den Einstellungen möglich — kein schneller Zugriff.

**Lösung:** `ThemeModeToggle`-Composable in `AppNavigation` eingefügt, zwischen dem Aufklapp-Button und dem Beenden-Button:
- **Zugeklappt:** einzelnes Icon (☀ / 🖥 / ☾), zyklisch: Auto → Hell → Dunkel → Auto
- **Aufgeklappt:** 3-Segment-Pill (32 dp hoch, `surface3` Hintergrund, `border`-Trennlinien, `accentSoft` für aktives Segment)

**Geänderte Dateien:**
- `manager/src/commonMain/kotlin/de/openindex/zugferd/manager/AppLayout.kt`
  - Imports: `border`, `BrightnessAuto`, `DarkMode`, `LightMode`, `rememberCoroutineScope`, `launch`, `RoundedCornerShape`, `height`, `size`, `fillMaxHeight`
  - Neue Composables `ThemeModeToggle` und `ThemeSegment`
  - `ThemeModeToggle(isExpanded = isExpanded)` Aufruf in `AppNavigation`

---

## Offene Punkte (nach Priorität)

| # | Prio | Problem |
|---|------|---------|
| P6 | 🟠 Hoch | Fehler im Prüfen-Bereich nicht hervorgehoben, schlecht gruppiert |
| P7 | 🟠 Hoch | Graue Schrift / schlechter Kontrast |
| P8 | 🟠 Hoch | Fehlermeldungen gemischt DE/EN |
| P10 | 🟡 Mittel | Tab-Navigation im Erstellen fehlt |
| P11 | 🟡 Mittel | Dark Mode: HTML-Bereich weiß |
| P15 | 🟡 Mittel | Visualisieren ↔ Prüfen ohne Datei neu laden |
| P16 | 🟡 Mittel | BT-21 Begriffe unklar |
| P17 | 🟡 Mittel | Suche durchsucht Navigationsleiste mit |
| P18 | 🟡 Mittel | Fehler vor Warnungen sortieren |
| P19 | 🟡 Mittel | Klick auf Fehler → Stelle in Visualisierung hervorheben |
| P21–P30 | 🟢 Niedrig | Diverse Nice-to-Have-Verbesserungen |
