# pCloud Sync – Benutzerhandbuch

Dieses Handbuch beschreibt die Bedienung von pCloud Sync (pCloudSyncClient) aus Sicht der Anwenderin und des Anwenders
(Stand 1.5.1). Die technischen Hintergründe stehen in der [Technischen Referenz](TECHNICAL_REFERENCE.md).

## Inhalt

1. [Grundbegriffe](#1-grundbegriffe)
2. [Installation und Erststart](#2-installation-und-erststart)
3. [Anmeldung](#3-anmeldung)
4. [Das Tray-Menü](#4-das-tray-menü)
5. [Das Aktivitätsfenster](#5-das-aktivitätsfenster)
6. [Einstellungen](#6-einstellungen)
7. [Mehrere Konten](#7-mehrere-konten)
8. [Arbeiten im Explorer](#8-arbeiten-im-explorer)
9. [Kontextmenü „pCloud ▸“](#9-kontextmenü-pcloud-)
10. [Vorschaubilder für Nur-online-Dateien](#10-vorschaubilder-für-nur-online-dateien)
11. [Freigeben](#11-freigeben)
12. [Versionen](#12-versionen)
13. [Speicherplatz: lokal oder online](#13-speicherplatz-lokal-oder-online)
14. [Ordner auswählen (selektive Synchronisation)](#14-ordner-auswählen-selektive-synchronisation)
15. [Known Folder Move – Desktop, Dokumente & Co.](#15-known-folder-move--desktop-dokumente--co)
16. [Löschen, Papierkorb und die Rückfrage bei vielen Löschungen](#16-löschen-papierkorb-und-die-rückfrage-bei-vielen-löschungen)
17. [Ransomware-Schutz](#17-ransomware-schutz)
18. [Konflikte](#18-konflikte)
19. [Namen mit Sonderzeichen](#19-namen-mit-sonderzeichen)
20. [Ausschlüsse](#20-ausschlüsse)
21. [Bandbreite und Parallelität](#21-bandbreite-und-parallelität)
22. [Automatische Pause](#22-automatische-pause)
23. [Updates](#23-updates)
24. [Neu anmelden und Konto trennen](#24-neu-anmelden-und-konto-trennen)
25. [Fehlerbehebung](#25-fehlerbehebung)
26. [Deinstallation](#26-deinstallation)
27. [Sicherheit und Datenschutz](#27-sicherheit-und-datenschutz)
28. [Lizenz](#28-lizenz)
29. [Sprache, Hilfe und Info](#29-sprache-hilfe-und-info)

---

## 1. Grundbegriffe

**Konto.** Ein verbundenes pCloud-Konto. pCloud Sync kann mehrere Konten nebeneinander bedienen (siehe
[7](#7-mehrere-konten)); jedes hat seinen eigenen Sync-Ordner, seinen eigenen pCloud-Ordner, seinen eigenen
gespeicherten Stand und seine eigene Anmeldung. Ein Konto kann nur einmal verbunden werden.

**Sync-Ordner.** Der lokale Ordner, in dem ein Konto abgebildet wird (Standard `D:\Cloud\pCloud`, wenn `D:` ein festes
NTFS-Laufwerk ist, sonst `%USERPROFILE%\pCloud`; weitere Konten bekommen `… (2)`, `… (3)` vorgeschlagen). Er wird bei
Windows als *Sync-Root* registriert; der Explorer zeigt ihn mit dem pCloud-Symbol und dem Namen „pCloud – <E-Mail>“ in
der Navigationsleiste.

**pCloud-Ordner.** Der Ordner in pCloud, der synchronisiert wird. `/` bedeutet: der gesamte pCloud-Speicher. Für erste
Versuche eignet sich ein Unterordner wie `/SyncTest`. Einzelne Ordner darunter lassen sich abwählen (siehe
[14](#14-ordner-auswählen-selektive-synchronisation)).

**Platzhalter (Online-Datei).** Eine Datei, die im Explorer mit Größe und Datum erscheint, aber keinen Platz belegt.
Ihr Inhalt liegt nur in pCloud und wird geladen, sobald ein Programm sie öffnet. Im Explorer trägt sie das
Wolken-Symbol.

**Lokal verfügbar.** Eine Datei, deren Inhalt auf dem PC liegt (grünes Häkchen). Sie kann wieder zur Online-Datei werden,
wenn Speicher gebraucht wird – es sei denn, sie ist *immer auf diesem Gerät behalten* (ausgefülltes grünes Häkchen).

**Status-Symbole im Explorer.** Die Spalte *Status* (Ansicht → Details) und die Overlays zeigen den Zustand:

| Symbol | Bedeutung |
|---|---|
| ☁ Wolke | nur online, kein Platz belegt |
| ✓ Häkchen (Umriss) | lokal verfügbar, kann bei Bedarf wieder freigegeben werden |
| ✓ Häkchen (ausgefüllt) | immer auf diesem Gerät behalten |
| ⟳ Pfeile | wird gerade übertragen |

**Der Client** ist die Tray-Anwendung `pCloudSync.exe`: das Wolken-Symbol im Infobereich der Taskleiste. Alles läuft
im Hintergrund, es gibt kein Hauptfenster – Bedienung über das Tray-Menü, das Aktivitätsfenster und das Kontextmenü im
Explorer. Ein Prozess bedient alle Konten.

---

## 2. Installation und Erststart

1. Setup starten (`pCloudSync-Setup-<Version>.exe` von <https://github.com/scorsten/pCloudSync-Releases/releases>; daneben
   liegt eine `SHA256SUMS.txt` zum Prüfen des Downloads). Es installiert nur für den angemeldeten Benutzer nach
   `%LOCALAPPDATA%\Programs\pCloudSync` und braucht keine Administratorrechte.
   - Optional: *pCloud Sync mit Windows starten* (Autostart).
   - Für das Windows-11-Kontextmenü fragt das Setup **einmalig** per UAC, ob das Signaturzertifikat des Shell-Pakets
     unter *Vertrauenswürdige Personen* eingetragen werden darf. Ohne Zustimmung gibt es nur das klassische Kontextmenü
     (Umschalt + Rechtsklick bzw. *Weitere Optionen anzeigen*); die Vorschaubilder für Online-Dateien (Abschnitt
     [10](#10-vorschaubilder-für-nur-online-dateien)) hängen nicht davon ab.
   - Sind Setup und Programmdateien nicht signiert (kein Zertifikat in der CI hinterlegt), warnt SmartScreen beim
     ersten Start des Setups: *Weitere Informationen* → *Trotzdem ausführen*.
2. Beim ersten Start öffnen sich die **Einstellungen** mit dem Bereich *Konten*; das erste Konto ist bereits angelegt:
   - Lokalen Ordner und pCloud-Ordner prüfen. Beides lässt sich später nur nach *Konto trennen* ändern.
   - Im Bereich *Anmeldung* das Anmeldeverfahren wählen (siehe [Anmeldung](#3-anmeldung)).
   - *Speichern und anmelden*.
3. Nach der Anmeldung beginnt der **erste Abgleich**. Der Client liest den Ordnerbaum aus pCloud und legt Platzhalter an –
   sie erscheinen im Explorer bereits, während das Einlesen läuft. Bei sehr großen Konten (mehr als eine Million
   Elemente) dauert das Einlesen mehrere Minuten; das Aktivitätsfenster zeigt den Fortschritt
   („… 4283 Ordner, 174807 Dateien eingelesen“).
4. Danach steht das Tray-Symbol auf **Aktuell**. Änderungen auf beiden Seiten werden ab jetzt laufend abgeglichen.
   Weitere Konten kommen über *Konto hinzufügen …* im Tray-Menü dazu (siehe [7](#7-mehrere-konten)).

**Update von 0.1.x:** Nichts zu tun. Beim ersten Start wird das bisherige Konto zum Profil `default`; Anmeldung,
gespeicherter Stand und Sync-Ordner bleiben unverändert (Protokoll: „Einstellungen auf Kontoprofile umgestellt“).

**Updates allgemein:** pCloud Sync prüft täglich auf neue Versionen und installiert sie auf Wunsch selbst (siehe
[23](#23-updates)). Wer ein Setup von Hand startet, wählt vorher im Tray-Menü **Beenden**. Das Setup ersetzt die Dateien;
der lokale Zustand bleibt erhalten, der nächste Start gleicht nur lokal ab (rund 40 Sekunden bei 1,5 Mio. Elementen).

---

## 3. Anmeldung

Das Anmeldeverfahren gilt für alle Konten (Einstellungen → *Anmeldung*); angemeldet wird je Konto über *Anmelden …* im
Tray-Menü oder *Speichern und anmelden* in den Einstellungen.

### 3.1 E-Mail und Passwort (Standard)

Keine Vorbereitung bei pCloud nötig. Das Passwort wird **nicht übertragen**: Der Client holt von pCloud einen Digest und
sendet `sha1(passwort + sha1(email) + digest)` – genau wie der offizielle pCloud-Client. Gespeichert wird nur das
zurückgelieferte Anmelde-Token im Windows-Anmeldeinformationsspeicher.

Zwei-Faktor-Anmeldung: Ist sie im pCloud-Konto aktiv, fragt der Dialog nach dem Code aus der Authenticator-App.
Alternativ *Code per SMS senden* oder *Wiederherstellungscode verwenden*. *Diesem Gerät vertrauen* (Standard: an)
erspart die Abfrage beim nächsten Mal.

EU- und US-Rechenzentrum werden automatisch erkannt (der Client probiert `api.pcloud.com` und `eapi.pcloud.com`).

### 3.2 Eigene pCloud-App (OAuth)

Für alle, die keinen Token aus der Passwort-Anmeldung wollen oder ohnehin eine App bei pCloud registriert haben.

1. Auf <https://docs.pcloud.com> anmelden → *My applications* → *New app*.
2. Namen vergeben, unter *Settings* als Redirect-URI `http://localhost:53682/callback` eintragen (Port ist in den
   Einstellungen änderbar). *Allow implicit grant* wird nicht benötigt und kann auf *Disallow* stehen.
3. Nach der Freigabe durch pCloud **Client-ID** und **Client-Secret** in die Einstellungen eintragen.
4. Haken bei *Redirect-URI ist in der pCloud-App eingetragen* setzen. Ist er gesetzt, öffnet sich der Browser, du erlaubst
   den Zugriff, und die Anmeldung schließt sich von selbst ab. Ohne Redirect-URI zeigt pCloud stattdessen einen Code
   an, den du in den Dialog *Code aus dem Browser einfügen* kopierst (auch die komplette Adresszeile funktioniert).
5. *Speichern und anmelden*.

Das Client-Secret liegt ebenfalls nur im Anmeldeinformationsspeicher, nicht in `settings.json`. Dieselbe App dient allen
Konten.

### 3.3 Neu anmelden (Zugang ersetzen, ohne zu trennen)

Tray-Menü → im Abschnitt des Kontos **Neu anmelden (z. B. mit eigener pCloud-App) …** oder in den Einstellungen
*Speichern und neu anmelden*. Der Client meldet sich mit dem gewählten Verfahren neu an und **ersetzt nur den Zugang**.
Sync-Ordner, Platzhalter und der gespeicherte Stand bleiben unverändert; nach dem Start folgt nur ein kurzer lokaler
Abgleich.

Es muss dasselbe pCloud-Konto sein. Meldest du dich mit einem anderen Konto an, lehnt der Client das ab („Das ist ein
anderes Konto“) und die bestehende Verbindung bleibt. Für einen Kontowechsel zuerst *Konto trennen* – oder das andere
Konto über *Konto hinzufügen …* zusätzlich verbinden. Ein Konto, das bereits in einem anderen Abschnitt verbunden ist,
wird abgelehnt („Dieses Konto ist bereits verbunden“).

---

## 4. Das Tray-Menü

Rechtsklick auf das Wolken-Symbol im Infobereich. Oben steht der Gesamtstatus, darunter folgt **je Konto ein
Abschnitt**, dann die allgemeinen Einträge:

| Eintrag | Wirkung |
|---|---|
| **Aktuell: …** / **2 Konten – Synchronisiere** | Gesamtstatus (nur Anzeige). Bei einem Konto Zustand und Meldung, bei mehreren die Anzahl und der „dringendste“ Zustand (Fehler vor Rückfrage vor Synchronisiere vor Offline vor Pausiert). Der Tooltip des Symbols zeigt bei mehreren Konten je Konto den Zustand („stefan: Aktuell · firma: Pausiert“). Ohne Konto: „Kein Konto verbunden – „Konto hinzufügen““. |
| **Konto: … – 3,2 von 5,4 TB belegt** bzw. **name@example.org – Aktuell** | Kopfzeile des Konto-Abschnitts (nur Anzeige): bei einem Konto die E-Mail und der belegte Speicherplatz des Kontos, bei mehreren E-Mail bzw. Sync-Ordner samt Zustand (der Speicherplatz steht dann im Tooltip der Zeile). Der Wert wird stündlich aktualisiert. |
| **pCloud-Ordner öffnen** | Öffnet den Sync-Ordner des Kontos im Explorer (Doppelklick auf das Symbol öffnet den Ordner des ersten Kontos). |
| **Pausieren ▸ 2 Stunden / 8 Stunden / 24 Stunden / Bis ich fortsetze** | Hält alle Übertragungen und Abgleiche dieses Kontos an – für die gewählte Dauer oder bis du *Fortsetzen* wählst. Platzhalter lassen sich weiterhin öffnen (Laden bei Bedarf läuft weiter). |
| **Fortsetzen** / **Fortsetzen (sonst automatisch um 17:30)** / **Trotzdem fortsetzen** | Beendet die Pause; bei einer Pause auf Zeit steht dabei, wann es von selbst weitergeht. *Trotzdem fortsetzen* erscheint bei einer automatischen Pause (siehe [22](#22-automatische-pause)). |
| **Jetzt vollständig abgleichen** | Liest den ganzen Ordnerbaum des Kontos aus pCloud neu und vergleicht ihn mit dem lokalen Stand. Bei großen Konten dauert das Minuten; im Normalbetrieb nicht nötig, weil der Änderungsstrom alles live liefert. |
| **Ordner auswählen …** | Öffnet die [Ordnerauswahl](#14-ordner-auswählen-selektive-synchronisation) (nur bei laufender Verbindung). |
| **Papierkorb (Browser)** | Öffnet den pCloud-Papierkorb dieses Kontos auf my.pcloud.com (gelöschte Dateien wiederherstellen, siehe [16](#16-löschen-papierkorb-und-die-rückfrage-bei-vielen-löschungen)). |
| **Rewind – Zeitpunkt wiederherstellen (Browser)** | Öffnet pCloud Rewind: den Stand des Kontos zu einem früheren Zeitpunkt ansehen und wiederherstellen (Umfang je nach pCloud-Tarif). |
| **Anmelden …** / **Neu anmelden …** | Anmeldung bzw. Zugang ersetzen (siehe [3.3](#33-neu-anmelden-zugang-ersetzen-ohne-zu-trennen)). |
| **Konto trennen …** / **Konto entfernen …** | Trennt den PC von diesem Konto (siehe [24](#24-neu-anmelden-und-konto-trennen)). Ein nie angemeldetes Konto wird nur aus der Liste entfernt. |
| **Konto hinzufügen …** | Legt ein weiteres Konto an und öffnet die Einstellungen (siehe [7](#7-mehrere-konten)). |
| **Aktivität und Protokoll …** | Öffnet das [Aktivitätsfenster](#5-das-aktivitätsfenster) (auch per Linksklick auf das Symbol). |
| **3 Konflikte lösen …** | Nur sichtbar, wenn es Konfliktkopien gibt (alle Konten zusammen): öffnet den [Konfliktdialog](#18-konflikte). |
| **Bekannte Ordner nach pCloud verschieben (KFM) …** | Öffnet den [KFM-Dialog](#15-known-folder-move--desktop-dokumente--co); bei mehreren verbundenen Konten fragt er zuerst nach dem Zielkonto. |
| **Einstellungen …** | Öffnet die [Einstellungen](#6-einstellungen). |
| **Lizenz …** / **Lizenz … (Testphase: noch 12 Tage)** | Lizenzstatus, Schlüssel eingeben, PC freigeben (siehe [28](#28-lizenz)). |
| **pCloud im Browser öffnen** | Öffnet <https://my.pcloud.com>. |
| **Nach Updates suchen …** / **Update auf 1.0.5 installieren …** | Prüft sofort auf eine neue Version bzw. installiert die bereits gefundene (siehe [23](#23-updates)). |
| **Hilfe …** | Kurzhilfe in der eingestellten Sprache, mit Link zu diesem Handbuch (siehe [29](#29-sprache-hilfe-und-info)). |
| **Über pCloud Sync …** | Version, Lizenzstatus, Lizenzbedingungen, Diagnoseangaben für den Support (siehe [29](#29-sprache-hilfe-und-info)). |
| **Beenden** | Beendet den Client sauber (alle Konten) und entfernt das Kontextmenü „pCloud ▸“ (wie bei Dropbox); der nächste Start legt es wieder an. Die Platzhalter und ihre Statussymbole bleiben erhalten – Nur-online-Dateien lassen sich ohne laufenden Client aber nicht öffnen. |

Das Symbol wechselt mit dem Zustand: Wolke mit Häkchen (aktuell), mit Pfeilen (synchronisiert), pausiert, offline,
Fehler bzw. Rückfrage. Bei mehreren Konten bestimmt das Konto mit dem dringendsten Zustand das Symbol. Wichtige
Ereignisse (Fehler, abgelaufene Anmeldung, Freigabe-Anfragen, neue Konflikte) erscheinen als Benachrichtigung; bei
Freigabe-Anfragen und Konflikten führt ein Klick darauf direkt weiter.

---

## 5. Das Aktivitätsfenster

Oben der aktuelle Status in Worten und eine Zeile mit Konto, Warteschlange (lokale Änderungen, die noch zu verarbeiten
sind), laufenden Uploads und Downloads sowie dem Zeitpunkt des letzten Vollabgleichs. Bei mehreren Konten werden die
Zahlen summiert, die Konten aufgezählt und die Meldungen aneinandergereiht („stefan: Aktuell · firma: Synchronisiere …“).
Darunter das laufende Protokoll (die letzten 1000 Zeilen, alle Konten gemeinsam).

Schaltflächen:

- **Protokollordner öffnen** – `%LOCALAPPDATA%\PCloudSync\logs` mit den Tagesprotokollen (14 Tage).
- **Auswahl kopieren** – markierte Zeilen (oder alle) in die Zwischenablage, z. B. für eine Fehlermeldung.
- **Vollständig abgleichen** – wie im Tray-Menü, für alle Konten.
- **Schließen**.

Typische Zeilen und was sie bedeuten:

| Zeile | Bedeutung |
|---|---|
| `Angemeldet als … (5411,1 von 18432,0 GiB belegt)` | Anmeldung erfolgreich, Kontostand |
| `Mit Sync-Root verbunden: D:\Cloud\pCloud` | Engine dieses Kontos bedient den Ordner (je Konto eine Zeile) |
| `Erster Abgleich – Placeholder werden bereits beim Einlesen angelegt.` | Erster Lauf ohne gespeicherten Stand |
| `pCloud liefert den Baum nicht in einem Stück (Fehler 1101, normal bei großen Konten) – lese ordnerweise (8 parallel).` | Großes Konto; ordnerweises Einlesen |
| `Lokaler Abgleich in 46,3s: 1539682 Elemente in pCloud, 0 neu, …` | Start-Abgleich gegen den gespeicherten Stand |
| `Vollabgleich in …: … Konflikte, … zum Hochladen, … lokale Verschiebungen, … lokale Löschungen` | Ergebnis eines vollständigen Abgleichs |
| `Hochgeladen: Ordner\Datei.txt` | Lokale Änderung ist in pCloud |
| `Neu (pCloud): …` / `Aktualisiert (pCloud): …` / `Verschoben (pCloud): …` | Änderung aus pCloud angewandt |
| `Lokal gelöscht von explorer.exe: … → wird in pCloud in den Papierkorb verschoben` | Löschung erkannt, Karenzzeit läuft |
| `Konflikt: lokale Version gesichert als … (Konflikt PC 2026-09-28 0915).docx` | Beidseitige Änderung |
| `Ordner ausgeschlossen: Archiv (in pCloud bleibt alles erhalten).` / `Ordner wieder eingeschlossen: Archiv – wird aus pCloud nachgezogen.` | Ordnerauswahl angewendet |
| `WARN Nicht hochgeladen: „Archiv“ ist von der Synchronisation ausgeschlossen …` | Lokal angelegter Ordner trägt den Namen eines abgewählten pCloud-Ordners |
| `WARN Ransomware-Verdacht: In den letzten 10 Minuten wurden 142 Dateien verändert, davon …` | Schutz hat angeschlagen, Rückfrage folgt |
| `Massenänderung bestätigt – Synchronisation läuft weiter.` / `Massenänderung nicht bestätigt – Synchronisation pausiert.` | Antwort auf die Rückfrage |
| `Update verfügbar: v1.0.5 (laufend: 1.0.0).` / `Setup pCloudSync-Setup-1.0.5.exe heruntergeladen und Prüfsumme bestätigt.` | Auto-Update |
| `Kontextmenü: share D:\…` / `Kontextmenü ausführen: …` | Befehl aus dem Explorer angekommen und ausgeführt |
| `Immer auf diesem Gerät behalten abgeschlossen: 12 Dateien, 340,2 MB` | Hintergrundauftrag fertig |
| `WARN Übergaben laufen auf Thread …` / `UI-Warteschlange seit … nie leer` | Diagnose der Oberfläche (siehe Fehlerbehebung) |

---

## 6. Einstellungen

Die Einstellungen sind in vier Bereiche gegliedert (Liste links): *Konten* gilt je Konto, *Anmeldung*,
*Synchronisation* und *Allgemein* gelten für alle Konten. *Speichern* übernimmt alles, ohne die laufende
Synchronisation zu unterbrechen – nur Änderungen an Ordnern, Parallelität der Übertragungen, Lösch-Schwelle oder
Vollabgleich-Intervall (nur in `settings.json`) starten die betroffene Engine neu.

### 6.1 Konten

Oben die Auswahlliste der Konten (E-Mail bzw. „Neues Konto“ und Sync-Ordner) mit *Konto hinzufügen*; darunter die
Werte des gewählten Kontos. *Speichern und anmelden* bzw. *Speichern und neu anmelden* gilt für das gewählte Konto.

| Option | Bedeutung | Standard |
|---|---|---|
| Lokaler Ordner | Sync-Ordner; NTFS, fest, kein anderer Sync-Ordner, keine Junction, nicht innerhalb des Ordners eines anderen Kontos. Nur ohne verbundenes Konto änderbar („Zum Ändern der Ordner zuerst das Konto trennen.“). | `D:\Cloud\pCloud` bzw. `%USERPROFILE%\pCloud`, weitere Konten `… (2)` |
| pCloud-Ordner | Pfad in pCloud (`/` = alles). Nur ohne verbundenes Konto änderbar. | `/` |
| Neue Dateien: Nur online | Neu in pCloud eingetroffene Dateien erscheinen als Platzhalter | ● |
| … Lokal neu angelegte Dateien nach dem Hochladen wieder nur online halten | Eine hier erstellte Datei wird nach dem Upload (mit 10 Minuten Verzögerung, falls sie nicht mehr benutzt wird) wieder zur Online-Datei | an |
| Neue Dateien: Immer lokal | Neu in pCloud eingetroffene Dateien werden automatisch geladen und behalten | |
| Max. Cache | Obergrenze in GB für automatisch geladene (nicht fest behaltene) Dateien; 0 = unbegrenzt. Die am längsten nicht benutzten werden zuerst wieder nur online gehalten. Der Hinweis zeigt die aktuelle Belegung. | 0 |
| Rückfrage ab Löschungen | Ab so vielen lokal fehlenden Elementen fragt der Client, statt in pCloud zu löschen (mindestens 5) | 50 |
| Nicht hochladen | Eigene Ausschlussmuster, mit `;` getrennt, `*` und `?` erlaubt (siehe [20](#20-ausschlüsse)) | leer |

Der Hinweis darunter zeigt, ob pCloud-Ordner abgewählt sind („3 pCloud-Ordner sind abgewählt“); geändert wird das im
Tray-Menü über *Ordner auswählen …* (siehe [14](#14-ordner-auswählen-selektive-synchronisation)). Ordner mit
*Immer auf diesem Gerät behalten* oder *Speicherplatz freigeben* haben Vorrang vor den Speicher-Regeln (siehe
[13](#13-speicherplatz-lokal-oder-online)).

### 6.2 Anmeldung

| Option | Bedeutung | Standard |
|---|---|---|
| E-Mail und Passwort | Anmeldung per Digest-Verfahren, keine App nötig | ● |
| Eigene pCloud-App (OAuth) | Anmeldung über eine bei pCloud registrierte App | |
| Client-ID / Client-Secret | Zugangsdaten der App (Secret nur im Anmeldeinformationsspeicher) | leer |
| Redirect-URI ist in der pCloud-App eingetragen | Anmeldung ohne Code-Eingabe | aus |
| Redirect-Port | Port der Redirect-URI `http://localhost:<Port>/callback` | 53682 |

### 6.3 Synchronisation

| Option | Bedeutung | Standard |
|---|---|---|
| Parallele Downloads | Gleichzeitige Downloads (Laden bei Bedarf, „Immer behalten“), je Konto | 6 |
| Parallele Uploads | Gleichzeitige Uploads, je Konto | 3 |
| Parallele Ordnerabfragen | Beim ordnerweisen Einlesen großer Konten. Wirkt sofort, auch auf ein laufendes Einlesen. | 8 |
| Upload höchstens / Download höchstens | Mbit/s, 0 = unbegrenzt. Gilt je Konto für alle Übertragungen gemeinsam und sofort. Die Download-Grenze gilt auch beim Öffnen von Dateien. | 0 |
| Bei getakteter Verbindung pausieren | Automatische Pause bei Mobilfunk, Hotspot oder „getaktet“ in den Windows-Netzwerkeinstellungen (siehe [22](#22-automatische-pause)) | an |
| Im Energiesparmodus pausieren | Automatische Pause, solange der Windows-Energiesparmodus aktiv ist | an |
| Ransomware-Schutz | Bei verdächtigen Massenänderungen Uploads anhalten und nachfragen (siehe [17](#17-ransomware-schutz)) | an |
| Geänderte große Dateien blockweise hochladen (nur geänderte Teile) | Nur die geänderten Blöcke einer Datei übertragen, die in pCloud schon liegt (siehe [21](#21-bandbreite-und-parallelität)) | an |
| Blockweise ab … MB | Ab dieser Dateigröße lohnt sich der blockweise Upload; kleinere Dateien gehen vollständig hoch | 8 |

### 6.4 Allgemein

| Option | Bedeutung | Standard |
|---|---|---|
| Sprache | Sprache der Oberfläche: *Automatisch (Windows-Anzeigesprache)* oder Deutsch, English, Français, Español, Português (Portugal), Português (Brasil), Nederlands, Italiano, Türkçe (siehe [29](#29-sprache-hilfe-und-info)) | Automatisch |
| Mit Windows starten | Autostart-Eintrag für den Benutzer | wie im Setup gewählt |
| Pausiert starten | Der Client startet, überträgt aber nichts, bis *Fortsetzen* gewählt wird | aus |
| Ausführliches Protokoll | Zusätzliche Debug-Zeilen (Hydration je Bereich, Übergaben, Wiederholungen, Vorschaubilder) | aus |
| Täglich nach Updates suchen und anbieten | Prüfung gegen die GitHub Releases (siehe [23](#23-updates)) | an |

---

## 7. Mehrere Konten

pCloud Sync verbindet beliebig viele pCloud-Konten gleichzeitig – etwa ein privates und ein geschäftliches. Jedes Konto
bekommt:

- einen eigenen **Sync-Ordner** (eigener Sync-Root, im Explorer als „pCloud – <E-Mail>“),
- einen eigenen **pCloud-Ordner** und eine eigene Ordnerauswahl,
- eigene **Speicher-Optionen** (Neue Dateien, Max. Cache), Lösch-Schwelle und Ausschlussmuster,
- einen eigenen gespeicherten Stand (`state.db` bzw. `state-<Kennung>.db`) und eine eigene Anmeldung im
  Windows-Anmeldeinformationsspeicher.

Gemeinsam für alle Konten gelten Anmeldeverfahren (und pCloud-App), Parallelität, Bandbreitengrenzen, Auto-Pause,
Ransomware-Schutz, Autostart, Protokoll und Updates.

**Konto hinzufügen.** Tray-Menü → *Konto hinzufügen …* (oder in den Einstellungen → *Konten* → *Konto hinzufügen*).
Die Einstellungen öffnen sich mit dem neuen Konto: lokalen Ordner (Vorschlag `… (2)`) und pCloud-Ordner wählen, dann
*Speichern und anmelden*. Die Ordner zweier Konten dürfen nicht ineinander liegen („Ordner überschneiden sich“).
Dasselbe pCloud-Konto kann nur einmal verbunden werden („Dieses Konto ist bereits verbunden“) – für einen zweiten
Ordner desselben Kontos den pCloud-Ordner des vorhandenen Kontos anpassen.

**Bedienung.** Das Tray-Menü zeigt je Konto einen Abschnitt mit Öffnen, Pausieren, Abgleichen, Ordnerauswahl, Anmelden
und Trennen (siehe [4](#4-das-tray-menü)); Pausieren wirkt je Konto, die automatische Pause auf alle. Das Kontextmenü
„pCloud ▸“ funktioniert in allen Sync-Ordnern, jeder Befehl geht an das Konto, zu dessen Ordner das Element gehört. KFM
fragt bei mehreren Konten, in welches Konto die bekannten Ordner verschoben werden sollen. Rückfragen (viele Löschungen,
Ransomware-Verdacht) nennen das betroffene Konto im Titel. Das Aktivitätsfenster fasst alle Konten zusammen.

**Konto entfernen.** *Konto trennen …* im Abschnitt des Kontos (siehe [24](#24-neu-anmelden-und-konto-trennen)); ein
Konto, das nie angemeldet war, heißt im Menü *Konto entfernen …* und wird nur aus der Liste genommen, der Ordner auf dem
PC bleibt unverändert. Nach dem Trennen verschwindet der Abschnitt aus dem Menü.

**Update von 0.1.x.** Das bisherige Konto wird beim ersten Start automatisch zum ersten Profil (Kennung `default`);
Anmeldung, `state.db` und Sync-Ordner bleiben, wie sie sind.

---

## 8. Arbeiten im Explorer

Im Sync-Ordner arbeitest du wie in jedem anderen Ordner:

- **Öffnen** einer Online-Datei lädt sie. Der Explorer zeigt den Fortschritt; große Dateien werden bereichsweise geladen,
  Programme können also z. B. ein Video abspielen, bevor es vollständig da ist.
- **Speichern, Anlegen, Kopieren** → Upload nach kurzer Beruhigungszeit (1,5 s nach der letzten Änderung). Während des
  Uploads bleibt die Datei benutzbar.
- **Umbenennen und Verschieben** innerhalb des Sync-Ordners → wird in pCloud nachvollzogen, ohne die Datei neu hochzuladen.
- **Verschieben aus dem Sync-Ordner heraus** (auch in den Papierkorb) → in pCloud wird das Element in den pCloud-Papierkorb
  verschoben (siehe [16](#16-löschen-papierkorb-und-die-rückfrage-bei-vielen-löschungen)).
- **Löschen** → dito, nach einer Karenzzeit von 15 Sekunden.
- **Zwischen den Ordnern zweier Konten** besser **kopieren** statt verschieben: Ein Element, das seinen Sync-Ordner
  verlässt, gilt für das Ursprungskonto als gelöscht (pCloud-Papierkorb), und ein verschobener Platzhalter trägt für das
  Zielkonto eine fremde Identität – nur vollständig geladene Dateien werden dort als neue Datei hochgeladen.

Solange der Client läuft, verhalten sich Online-Dateien für Programme wie normale Dateien. Ist der Client beendet oder
pausiert *und* offline, schlägt das Öffnen einer Online-Datei fehl („Die Datei ist nicht verfügbar“).

**Statusflyout (Windows 11):** Im Sync-Ordner zeigt der Explorer links in der Adressleiste das pCloud-Symbol. Ein Klick
darauf öffnet ab Windows 11 Build 23504 (2023er Insider-Builds, ab 24H2 überall) ein Flyout mit Zustand („Aktuell“,
„Synchronisiere“, „Pausiert“ …), dem belegten Speicherplatz und Schaltflächen: *pCloud im Browser öffnen*,
*Pausieren* bzw. *Fortsetzen*, *Aktivität und Protokoll*, *Einstellungen*. Das Flyout gehört zum Windows-11-Paket
(siehe [9](#9-kontextmenü-pcloud-)); läuft pCloud Sync nicht, meldet es „pCloud Sync läuft nicht“.

**Windows-Suche:** Der Sync-Ordner wird beim Verbinden in den Windows-Suchindex aufgenommen, die Suche im Explorer und
im Startmenü findet also auch Nur-online-Dateien nach ihrem Namen. Beim Trennen des Kontos wird der Ordner wieder aus
dem Index entfernt.

**Statusspalte – Freigaben und Konflikte:** Neben dem Synchronisationssymbol (Wolke, Häkchen) zeigt die Spalte *Status*
bis zu zwei weitere Symbole:

| Symbol | Bedeutung (Tooltip) |
|---|---|
| Zwei Personen | *Von dir freigegeben* – ein eigener Ordner, den du an andere freigegeben hast, samt Inhalt; *Für dich freigegeben* – ein Ordner aus dem Konto eines anderen. |
| Zwei Personen mit Schloss | *Für dich freigegeben – nur lesen*. |
| Kette | *Öffentlicher Link* – Datei oder Ordner mit Link oder Upload-Link (bei freigegebenen Ordnern: *Von dir freigegeben · Öffentlicher Link*). |
| Gelbes Warndreieck | *Konfliktkopie* – mit Rechtsklick › *pCloud* › *Konflikt lösen …* auflösen (siehe [18](#18-konflikte)). |

Die Symbole erscheinen nur in der Ansicht **Details** (Spalte *Status*), nicht bei großen Symbolen oder Kacheln –
wie bei OneDrive. Die Symbole kommen aus der Shell-DLL (am Sync-Root als *CustomStateHandler* eingetragen – beim ersten Start von 1.3
einmalig per UAC-Abfrage). Freigaben und Links liest pCloud Sync alle 15 Minuten aus pCloud; was du im Freigeben-Dialog anlegst oder
entfernst, erscheint sofort.

**Schreibgeschützte Freigaben:** Ordner, die andere pCloud-Nutzer nur zum Lesen freigegeben haben, erscheinen mit
Schreibschutz-Attribut. Änderst du eine Datei darin trotzdem (nach Aufheben des Schreibschutzes), bleibt die Änderung
lokal – der Client lädt nichts hoch und schreibt einmalig eine Warnung ins Protokoll („Nur Leserechte in dieser
Freigabe …“).

---

## 9. Kontextmenü „pCloud ▸“

Rechtsklick auf Dateien oder Ordner im Sync-Ordner. Unter Windows 11 erscheint das Untermenü im neuen, kurzen Menü
(wenn das signierte Paket installiert ist) und im klassischen Menü (*Weitere Optionen anzeigen* oder
Umschalt + Rechtsklick). Mit installiertem Paket stammt der Eintrag in beiden Menüs aus dem Paket; ohne Paket
(Windows 10, UAC abgelehnt) schreibt der Client einen klassischen Registry-Eintrag. Das Menü gibt es nur, solange
pCloud Sync läuft – nach *Beenden* verschwindet es. Bei Mehrfachauswahl werden die Elemente gemeinsam verarbeitet. Bei
mehreren Konten gilt das Menü in allen Sync-Ordnern.

| Befehl | Dateien | Ordner | Wirkung |
|---|:-:|:-:|---|
| **Immer auf diesem Gerät behalten** | ✓ | ✓ | Lädt die Elemente (bei Ordnern rekursiv) und hält sie dauerhaft lokal – auch neue Dateien, die später in diesem Ordner eintreffen. Läuft im Hintergrund; das Protokoll meldet den Abschluss. |
| **Speicherplatz freigeben** | ✓ | ✓ | Verwirft den lokalen Inhalt (in pCloud bleibt alles) und markiert die Elemente als „nur online“. Neue Dateien in einem so markierten Ordner bleiben ebenfalls online. |
| **Öffentlichen Link kopieren** | ✓ | ✓ | Erstellt einen pCloud-Link ohne Optionen und legt ihn in die Zwischenablage. |
| **Freigeben …** | ✓ | ✓ | Öffnet den [Freigeben-Dialog](#11-freigeben). |
| **Versionen …** | ✓ | – | Öffnet den [Versionen-Dialog](#12-versionen). |
| **In pCloud (Browser) anzeigen** | ✓ | ✓ | Öffnet den Ordner in der pCloud-Weboberfläche (EU: `e.pcloud.com`, US: `my.pcloud.com`). |
| **Konflikt lösen …** | ✓ | – | Nur auf Konfliktkopien („… (Konflikt PC 2026-09-29 1015).docx“): öffnet den [Konfliktdialog](#18-konflikte) mit dieser Datei. |

Voraussetzung ist ein laufender, verbundener Client. Andernfalls erscheint „pCloud Sync läuft nicht“ bzw. „pCloud Sync
ist nicht mit pCloud verbunden (bitte anmelden)“; ein Element außerhalb aller Sync-Ordner meldet „… liegt nicht im
pCloud-Ordner“. Ein Element, das noch nicht hochgeladen ist, meldet „ist (noch) nicht in pCloud“.

---

## 10. Vorschaubilder für Nur-online-Dateien

In der Miniaturansicht des Explorers zeigen Online-Dateien normalerweise nur ein Symbol – ihr Inhalt liegt ja nicht auf
dem PC. pCloud Sync liefert stattdessen ein **Vorschaubild aus pCloud**, ohne die Datei herunterzuladen: für Bilder,
Videos und Dokumente, für die pCloud Vorschaubilder erzeugt (Archive, Programme und unbekannte Formate behalten ihr
Symbol). Lokal vorhandene Dateien behandelt der Explorer wie gewohnt selbst.

Voraussetzungen:

- Die **Shell-DLL** `PCloudSyncShell.dll` aus dem Setup. Der Client registriert den Vorschaubild-Handler beim Start als
  COM-Klasse des Benutzers und trägt ihn am Sync-Root ein – direkt oder, falls Windows den Eintrag ohne erhöhte Rechte
  verweigert, einmalig nach einer UAC-Abfrage (eine Ablehnung merkt sich der Client und fragt nicht erneut). Das
  signierte Windows-11-Paket registriert die Klasse zusätzlich.
- Ein **laufender, verbundener Client**: Das Vorschaubild wird von der laufenden Anwendung bei pCloud abgeholt (höchstens
  5 Sekunden je Anfrage; eine Pause stört nicht, offline gibt es keins – der Explorer zeigt dann das Symbol).

Der Explorer fragt Größen zwischen 32 und 2560 Pixeln an; der Client holt von pCloud ein quadratisch eingepasstes,
unbeschnittenes JPEG zwischen 32 und 1024 Pixeln, das nur verkleinert, nie vergrößert wird. Der Explorer merkt sich
Vorschaubilder in seinem eigenen Cache.

**Tempo in großen Ordnern:** Die erste Anfrage in einem Ordner lädt die Vorschaubilder der umliegenden Dateien voraus
(300 danach, 100 davor in Namensreihenfolge, 10 gleichzeitig; neue Anfragen beim Scrollen haben Vorrang). Geladene
Bilder liegen in einer Ablage auf der Platte (`%LOCALAPPDATA%\PCloudSync\thumbs`, höchstens 200 MB) und werden
außerdem im Hintergrund in den Vorschaubild-Cache von Windows geschrieben – beim Scrollen findet der Explorer sie dort
und zeigt sie sofort, ohne nachzuladen. Weit hinten in einem Ordner mit Tausenden Bildern dauert es beim ersten Mal
kurz, bis das Fenster nachgezogen ist.

Erscheinen keine Vorschaubilder für Online-Dateien, prüfe bei laufendem Client in PowerShell die COM-Registrierung des
Handlers und den Eintrag am Sync-Root:

```powershell
Get-ItemProperty "HKCU:\Software\Classes\CLSID\{04079C3A-8CDD-45B8-BB23-00E5518D5D82}\InprocServer32" -ErrorAction SilentlyContinue | Select-Object '(default)', ThreadingModel; Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SyncRootManager' | Where-Object { $_.PSChildName.StartsWith('PCloudSync!') } | ForEach-Object { '{0}  ThumbnailProvider={1}' -f $_.PSChildName, $_.GetValue('ThumbnailProvider') }
```

Erwartet wird der Pfad der DLL im Installationsordner und je Konto ein Eintrag mit `ThumbnailProvider =
{04079C3A-8CDD-45B8-BB23-00E5518D5D82}`. Das Protokoll (Aktivitätsfenster) meldet beim Start je Konto „Vorschaubild-
Handler am Sync-Root eingetragen (HKLM)“ bzw. eine Warnung mit dem Grund; mit *Ausführliches Protokoll* kommt je Anfrage
des Explorers eine Zeile dazu („Vorschaubild für … (256 px, … Bytes)“ bzw. „Kein Vorschaubild für Datei …“). Der
Explorer merkt sich Symbole – nach der ersten Registrierung hilft ein Neustart des Explorers (`Stop-Process -Name
explorer`) oder ein Wechsel der Ansicht.

---

## 11. Freigeben

Der Dialog zeigt, was du freigibst, und bietet je nach Typ bis zu vier Karten:

**Bestehende Freigaben** (oben)
- Liste der vorhandenen Links, Upload-Links und – bei Ordnern – der Personen mit Zugriff bzw. offenen Einladungen, mit
  Rechten (*nur lesen* / *lesen und schreiben*), Erstellungs- und Ablaufdatum.
- *Link kopieren* – den gewählten Link erneut in die Zwischenablage legen.
- *Nur lesen* / *Lesen und schreiben erlauben* – Rechte einer angenommenen Freigabe umstellen.
- *Löschen* bzw. *Freigabe beenden* – Link oder Upload-Link löschen, Freigabe für eine Person beenden oder eine
  Einladung zurückziehen (mit Rückfrage).

**Link für alle, die ihn kennen** (Dateien und Ordner)
- *Läuft ab am* – Datum, ab dem der Link nicht mehr funktioniert.
- *Höchstens … Downloads* – Download-Limit.
- *Passwort* – nur mit pCloud Premium; ohne Premium meldet pCloud einen Fehler, der Link wird dann nicht erstellt.
- *Link erstellen und kopieren* – der Link erscheint im Feld und liegt in der Zwischenablage.
- *Über Windows teilen …* – erstellt den Link (mit den gewählten Optionen) und öffnet das Teilen-Fenster von Windows:
  E-Mail, Outlook, Teams, Nearby Sharing (an PCs in der Nähe) oder andere Apps, die Links empfangen. Ein schon
  erstellter Link wird wiederverwendet, solange du die Optionen nicht änderst.

**Nur für bestimmte Personen** (bei Dateien): pCloud gibt nur Ordner an Personen frei; ein Link ist immer nur zum
Ansehen und Herunterladen. *In eigenen Ordner verschieben und freigeben …* legt neben der Datei einen Ordner mit ihrem
Namen an (z. B. `koeln` für `koeln.jpg`), verschiebt sie hinein – dieselbe Datei, Links und Versionen bleiben –, wartet,
bis pCloud beides kennt, und öffnet den Freigeben-Dialog des Ordners. *Ordner „…“ freigeben …* gibt stattdessen den
Ordner frei, in dem die Datei liegt.

**Personen einladen** (nur Ordner)
- E-Mail-Adresse, *Nur lesen* oder *Lesen und schreiben*, optionale Nachricht. pCloud sendet eine Einladung; die Person
  sieht den Ordner in ihrem pCloud-Konto.

**Upload-Link** (nur Ordner)
- Andere können Dateien in diesen Ordner hochladen, ohne den Inhalt zu sehen. Optional ein Hinweistext. pCloud liefert
  außerdem eine E-Mail-Adresse, an die sich Dateien senden lassen (steht nach dem Erstellen in der Statuszeile).

**Freigabe-Anfragen von anderen:** Lädt dich jemand zu einem Ordner ein, zeigt pCloud Sync eine Benachrichtigung
(„max@example.org möchte „Projekt“ mit dir teilen …“). Ein Klick darauf öffnet die Freigaben in pCloud im Browser, wo du
die Einladung annimmst; danach erscheint der Ordner im Sync-Ordner – nur zum Lesen freigegebene Ordner mit Schreibschutz
(siehe [8](#8-arbeiten-im-explorer)).

---

## 12. Versionen

Zeigt die früheren Versionen einer Datei aus pCloud (Datum, Größe). *Diese Version wiederherstellen* macht die gewählte
Version in pCloud wieder zur aktuellen; der bisherige Stand bleibt selbst als Version erhalten. Die lokale Datei wird
über den Änderungsstrom automatisch aktualisiert. Hat die Datei noch nicht hochgeladene lokale Änderungen, verweigert der
Client das Wiederherstellen, bis sie synchron ist.

---

## 13. Speicherplatz: lokal oder online

Drei Ebenen bestimmen, ob eine Datei lokal liegt:

1. **Ausdrückliche Markierung** per Kontextmenü (*Immer behalten* / *Speicherplatz freigeben*). Sie gilt für das Element
   und vererbt sich auf alles darunter: Eine neue Datei, die in einem „Immer behalten“-Ordner eintrifft, wird geladen; in
   einem „Speicherplatz freigeben“-Ordner bleibt sie online.
2. **Einstellung „Neue Dateien“** (je Konto) für alles ohne Markierung: *Nur online* oder *Immer lokal*.
3. **Cache-Obergrenze** (je Konto): Automatisch geladene Dateien (durch Öffnen oder *Immer lokal*) zählen in den Cache.
   Über der Grenze werden die am längsten nicht benutzten wieder online gehalten – frühestens 5 Minuten nach der
   letzten Benutzung. Fest behaltene Dateien zählen nicht.

Beispiel für ein Notebook mit wenig Platz: *Neue Dateien: Nur online*, Cache 20 GB, und nur den Ordner
`Dokumente\Aktuell` auf *Immer auf diesem Gerät behalten*.

Beispiel für einen Desktop-PC mit großer Platte: *Neue Dateien: Immer lokal*, Cache 0 (unbegrenzt).

Wer ganze pCloud-Ordner auf diesem PC gar nicht sehen will (auch nicht als Platzhalter), wählt sie ab (siehe
[14](#14-ordner-auswählen-selektive-synchronisation)).

**Sonderfall Verknüpfungen:** In per KFM umgeleiteten Ordnern (z. B. Desktop) bleiben `.lnk`, `.url` und `desktop.ini`
immer lokal, damit Desktop und Startmenü auch ohne laufenden Client funktionieren.

---

## 14. Ordner auswählen (selektive Synchronisation)

Tray-Menü → Abschnitt des Kontos → *Ordner auswählen …* (nur bei laufender Verbindung). Der Dialog „Welche pCloud-Ordner
sollen auf diesem PC erscheinen?“ zeigt den Ordnerbaum des pCloud-Ordners mit Kästchen; Unterordner werden beim
Aufklappen aus pCloud gelesen. Ein abgewähltes Kästchen nimmt alle Unterordner mit. *Übernehmen* wendet die Auswahl an.
pCloud-Crypto-Ordner (clientseitig verschlüsselt) stehen nicht zur Auswahl und werden nie synchronisiert – lokal wären
Namen und Inhalte nur Chiffrat.

Was passiert:

- **Abgewählte Ordner** verschwinden aus dem Sync-Ordner – Platzhalter und gespeicherter Stand werden entfernt, **in
  pCloud bleibt alles erhalten**. Änderungen aus pCloud unterhalb solcher Ordner werden nicht mehr abgebildet, auch
  nicht beim vollständigen Abgleich.
- **Wieder angehakte Ordner** werden samt Inhalt aus pCloud nachgezogen (als Platzhalter, nach den üblichen
  Speicher-Regeln).
- Der Ausschluss ist an den Ordner in pCloud gebunden, nicht an seinen Namen: Wird der Ordner in pCloud umbenannt oder
  verschoben, bleibt er abgewählt. Ein in pCloud gelöschter und neu angelegter Ordner gleichen Namens ist ein neuer
  Ordner und erscheint wieder.
- Die Auswahl gilt je Konto und nur für diesen PC (`settings.json`).

**Abgelehnt** wird ein Ausschluss, solange unterhalb des Ordners noch etwas auf den Upload wartet – noch nicht
verarbeitete lokale Änderungen, ein laufender Upload oder Dateien, die noch nicht in pCloud sind („Ordner kann noch
nicht ausgeschlossen werden“ mit dem Grund, z. B. „Unter „Archiv“ sind 3 Datei(en) noch nicht in pCloud (z. B.
Archiv\neu.txt). Bitte warten, bis alles synchron ist.“). Warte, bis das Konto auf **Aktuell** steht, und versuche es
erneut; so geht nie etwas verloren, was nur auf diesem PC liegt.

**Gleichnamiger lokaler Ordner.** Legst du im Sync-Ordner einen Ordner an, der in pCloud bereits existiert und dort
abgewählt ist, wird er **nicht hochgeladen**: Das Protokoll warnt einmal („Nicht hochgeladen: „Archiv“ ist von der
Synchronisation ausgeschlossen … Lokale Inhalte bleiben nur auf diesem PC.“). Entweder den Ordner wieder einschließen
(dann wird der pCloud-Inhalt nachgezogen und der lokale Inhalt hochgeladen) oder den lokalen Ordner umbenennen.

Abgrenzung: *Nicht hochladen* (siehe [20](#20-ausschlüsse)) hält **lokale** Dateien und Ordner nach Namensmustern vom
Upload fern; *Speicherplatz freigeben* (siehe [13](#13-speicherplatz-lokal-oder-online)) lässt Platzhalter sichtbar und
gibt nur den Inhalt frei; *Ordner auswählen* blendet **pCloud-Ordner** auf diesem PC ganz aus.

---

## 15. Known Folder Move – Desktop, Dokumente & Co.

Tray-Menü → *Bekannte Ordner nach pCloud verschieben (KFM) …*. Sind mehrere Konten verbunden, fragt der Client zuerst, in
welches Konto die Ordner verschoben werden sollen. Die Liste zeigt Desktop, Dokumente, Bilder, Musik, Videos und
Downloads mit Status, aktuellem Pfad und Ziel in pCloud. Jede Entscheidung gilt **je Ordner und nur für diesen PC** – so
kann der Desktop auf einem PC in pCloud liegen und auf einem anderen lokal bleiben.

| Status | Bedeutung |
|---|---|
| Verschiebbar – am Standardort | Liegt im Benutzerprofil, kann verschoben werden |
| Verschiebbar – lokal umgeleitet | Zeigt bereits auf einen anderen lokalen Ordner |
| ✔ in pCloud | Bereits umgeleitet in den Sync-Ordner |
| Nur Hinweis – Junction/Symlink | Der Ordner (oder ein Elternordner) ist eine Verknüpfung, z. B. zu Dropbox. Wird nicht angefasst. |
| Nur Hinweis – anderer Sync-Ordner | Liegt in OneDrive, Dropbox o. Ä. Wird nicht angefasst. |
| Nur Hinweis – Netzwerkpfad | Ordnerumleitung per Gruppenrichtlinie. Wird nicht angefasst. |

**Ausgewählte nach pCloud verschieben.** Kästchen anhaken, Schaltfläche wählen. Vorher prüft der Client den Inhalt und
warnt vor Outlook-Datendateien, virtuellen Festplatten, Entwicklerordnern (`node_modules`, `.git`, `.vs`) und sehr großen
Dateien – solche Inhalte besser vorher woanders ablegen oder ausschließen. Dann werden die Inhalte nach
`<Sync-Ordner>\<Name>` verschoben (auch laufwerksübergreifend) und der Ordner per `SHSetKnownFolderPath` umgeleitet. Schlägt
ein Schritt fehl, wird alles zurückverschoben. Programme mit geöffneten Dateien vorher schließen.

**Markierten nur umleiten ….** Für Ordner, deren Inhalt **schon in pCloud liegt** (z. B. weil er auf einem anderen PC
verschoben wurde): Ordner in der Liste anklicken (nicht das Kästchen), Schaltfläche wählen, Zielordner in pCloud
auswählen. Es wird nichts verschoben oder kopiert; die bisherigen Inhalte und eventuelle Junctions bleiben unverändert am
alten Ort.

**Ausgewählte zurücksetzen.** Leitet wieder auf den ursprünglichen Ort um. *Beim Zurücksetzen Inhalte zurückkopieren*
(Standard: an) kopiert die Inhalte zurück – das lädt Online-Dateien herunter. Bei „nur umgeleiteten“ Ordnern wird nie
kopiert, weil die alten Inhalte noch da sind. Die Kopie in pCloud bleibt in jedem Fall erhalten.

Nach jeder Änderung: ab- und wieder anmelden (oder den Explorer neu starten), damit alle Programme die neuen Pfade
verwenden.

---

## 16. Löschen, Papierkorb und die Rückfrage bei vielen Löschungen

- Was du lokal löschst, verschiebt der Client in pCloud in den **pCloud-Papierkorb** (dort 30 Tage wiederherstellbar) –
  nach einer Karenzzeit von 15 Sekunden. Machst du die Löschung in dieser Zeit rückgängig (Strg+Z, Papierkorb), passiert
  in pCloud nichts. *Papierkorb (Browser)* im Tray-Menü öffnet den pCloud-Papierkorb direkt; *Rewind* stellt bei Bedarf
  den Stand eines ganzen Zeitpunkts wieder her.
- Was in pCloud gelöscht wird, verschwindet lokal (Platzhalter werden entfernt; lokal geänderte, noch nicht hochgeladene
  Inhalte werden vorher als Konfliktkopie gesichert).
- **Rückfrage bei vielen Löschungen:** Fehlen beim Abgleich mehr Elemente als die Schwelle (Standard 50, je Konto),
  fragt der Client („… Elemente fehlen lokal (<Konto>)“), bevor er etwas in pCloud löscht:
  - *Auch in pCloud löschen* – die Elemente wandern in den pCloud-Papierkorb.
  - *Lokal wiederherstellen* – nichts wird gelöscht, die Elemente erscheinen wieder als Online-Dateien.
  Wähle **Lokal wiederherstellen**, wenn du die Elemente nicht selbst gelöscht hast (z. B. nach einem Laufwerkswechsel).
  Bis zur Antwort steht die Synchronisation dieses Kontos.

---

## 17. Ransomware-Schutz

Ransomware verschlüsselt in kurzer Zeit viele Dateien und benennt sie meist um (`bericht.docx` → `bericht.docx.locked`).
Ein Sync-Client würde diese Fassungen brav nach pCloud hochladen. pCloud Sync beobachtet deshalb jede lokale Änderung,
bevor sie hochgeladen wird, und **hält die Uploads an, sobald der Schub nach Massenverschlüsselung aussieht** – in pCloud
bleiben die unversehrten Fassungen die aktuellen. Laden bei Bedarf und Änderungen aus pCloud laufen weiter.

Einstellungen → *Synchronisation* → *Ransomware-Schutz* (Standard: an, gilt für alle Konten).

**Woran der Client Verdacht schöpft** (Beobachtungsfenster: die letzten 10 Minuten). Eine Änderung gilt als auffällig,
wenn

1. eine Datei in eine **unbekannte Dateiendung** umbenannt wird oder eine neue bzw. geänderte Datei eine unbekannte
   Endung trägt,
2. an den **vollständigen alten Namen etwas angehängt** wird (`bericht.docx.locked`, `foto.jpg.id-1234.[mail].bkp`) –
   ausgenommen genau ein üblicher Zusatz wie `.bak`, `.gz`, `.gpg` oder eine Nummer (`app.log.1`), oder
3. eine normalerweise **unkomprimierte Datei** (txt, csv, log, json, xml, doc, xls, bmp, wav …) mit Inhalt überschrieben
   wird, der **verschlüsselt wirkt** (Zufallsverteilung der ersten 64 KB). Ohnehin komprimierte Formate (docx, pdf, jpg,
   zip …) werden nicht geprüft – eine Konvertierung txt → docx gilt nicht als verschlüsselt.

**Alarm** gibt es, wenn im Fenster mindestens 30 Änderungen liegen und davon 60 % auffällig sind, wenn eine einzige
unbekannte oder angehängte Endung mindestens 30 Dateien betrifft (auch inmitten vieler normaler Änderungen), oder wenn
25 verschiedene unbekannte Endungen auftauchen. Als **bekannt** gelten eine eingebaute Liste sehr üblicher Endungen
(Office, Bilder, Audio, Video, Archive, Code, Text …), alle Endungen, die im Konto bereits vorkommen, und Endungen, die
ein ganzes Fenster lang ohne Alarm benutzt wurden – ein paar Dateien eines neuen Programms werden so gelernt, ein Schub
`.locked` nie. 500 Fotos kopieren, ein Git-Checkout oder ein entpacktes ZIP lösen keinen Alarm aus.

**Die Rückfrage.** Das Symbol wechselt auf Achtung, der Status meldet „Verdächtige Massenänderung erkannt – Bestätigung
erforderlich“, und ein Dialog „Verdächtige Massenänderung (<Konto>)“ fasst zusammen („In den letzten 10 Minuten wurden
142 Dateien verändert, davon 131 in eine neue, unbekannte Dateiendung (.locked) umbenannt.“) und nennt Beispiele:

- **Pausieren und PC prüfen** – nichts wird hochgeladen; das Konto steht auf „Pausiert – verdächtige Massenänderung
  (bitte PC prüfen)“. Die lokalen Änderungen bleiben in der Warteschlange und werden erst nach *Fortsetzen* im
  Tray-Menü übertragen.
- **Weiter synchronisieren** – die Änderungen sind gewollt (ein Programm hat viele Dateien umbenannt oder konvertiert).
  Die beanstandeten Endungen gelten fortan als bekannt, derselbe Schub alarmiert nicht erneut.

Im Zweifel pausieren. Die Rückfrage kommt erst, wenn die Schwelle erreicht ist – die ersten Änderungen des Schubs können
also bereits in pCloud sein; dort lassen sich mit *Rewind* (Weboberfläche) frühere Stände wiederherstellen, einzelne
Dateien über *Versionen …*.

**War der PC tatsächlich befallen:** nicht fortsetzen. Den PC bereinigen (oder neu aufsetzen), dann das Konto **trennen**
(siehe [24](#24-neu-anmelden-und-konto-trennen)) – das verwirft die wartenden Änderungen, ohne sie zu übertragen, und
löst die lokalen Dateien von pCloud, sodass sie gefahrlos gelöscht werden können. Lokal veränderte Dateien nicht im
Explorer löschen, solange das Konto verbunden ist: Die Löschungen würden nach dem Fortsetzen ebenfalls nach pCloud
übertragen. Anschließend in pCloud den Stand prüfen bzw. per *Rewind* zurücksetzen und das Konto neu verbinden.

Grenzen: Die Erkennung ist eine Heuristik über Dateiendungen und Inhaltsstichproben. Ein Schädling, der komprimierte
Formate (docx, jpg, pdf, zip …) verschlüsselt, ohne die Dateien umzubenennen, ist damit nicht erkennbar.

---

## 18. Konflikte

Wurde eine Datei auf diesem PC **und** in pCloud geändert, bevor beide Seiten abgeglichen waren, behält der Client beide
Fassungen: Die pCloud-Fassung liegt am Originalnamen, die lokale als
`Name (Konflikt <PC-Name> <Datum> <Uhrzeit>).ext` daneben – und wird ebenfalls hochgeladen, damit sie auf allen Geräten
sichtbar ist. Nichts geht verloren; welche Fassung gilt, entscheidest du im **Konfliktdialog**:

- Eine Benachrichtigung meldet jeden neuen Konflikt („„Bericht.docx“ wurde hier und in pCloud gleichzeitig geändert …“);
  ein Klick öffnet den Dialog. Beim Start meldet pCloud Sync einmal, wenn noch Konflikte offen sind.
- Außerdem: Tray-Menü › *N Konflikte lösen …* oder Rechtsklick auf die Konfliktkopie › *pCloud* › *Konflikt lösen …*.
  In der Statusspalte des Explorers tragen Konfliktkopien ein gelbes Warndreieck.

Der Dialog listet alle offenen Konflikte aller Konten (Datei, Ordner, Rechner, Zeitpunkt; Mehrfachauswahl möglich):

| Schaltfläche | Wirkung |
|---|---|
| **Diese Kopie behalten** | Der Inhalt der Konfliktkopie wird in das Original geschrieben (dieselbe Datei – in pCloud eine neue Version; der vorherige Stand bleibt unter *Versionen*), die Kopie wird gelöscht. Ist das Original ein Platzhalter, wird es dafür zuerst geladen. Gibt es das Original nicht mehr, wird die Kopie einfach umbenannt. |
| **Original behalten** | Die Konfliktkopie wird gelöscht (in pCloud landet sie im Papierkorb). |
| **Beide behalten** | Die Kopie bekommt einen neutralen Namen, z. B. `Bericht (PC 2026-09-29 1015).docx`, und gilt nicht mehr als Konflikt. |
| **Beide öffnen** | Öffnet Original und Kopie in ihren Programmen – zum Vergleichen, bevor du dich entscheidest. |

Erkannt werden auch Konfliktkopien, die ein anderer PC angelegt und über pCloud verteilt hat. Ist eine der Dateien
gerade in einem Programm geöffnet, meldet der Dialog das; nach dem Schließen erneut versuchen.

Keine Konflikte entstehen bei bloßem Umbenennen, bei nur teilweise geladenen Dateien oder wenn sich Größe und
Änderungszeit der lokalen Datei nicht geändert haben: In diesen Fällen übernimmt der Client einfach den pCloud-Stand.

---

## 19. Namen mit Sonderzeichen

pCloud erlaubt Zeichen, die Windows in Dateinamen verbietet (etwa aus macOS oder Linux hochgeladen). Solche Namen zeigt
der Client lokal mit gleich aussehenden Ersatzzeichen; in pCloud bleibt der Originalname, und beim Umbenennen oder
Hochladen wird zurückübersetzt:

| in pCloud | lokal |
|---|---|
| `< > : " / \ \| ? *` | `＜ ＞ ： ＂ ／ ＼ ｜ ？ ＊` (Vollbreiten-Zeichen) |
| Steuerzeichen (Tab usw.) | `␉` usw. |
| Leerzeichen am Ende | `␠` |
| Punkt am Ende | `．` |
| Ersatzzeichen selbst im Namen | mit `‛` markiert, z. B. `schon‛：voll` |

Übersprungen werden nur reservierte Gerätenamen (`CON`, `NUL`, `COM1` …) und Namen, die sich nur in Groß-/Kleinschreibung
unterscheiden (das zweite Element wird im Protokoll gemeldet). Ein lokal angelegter Name mit `／` wird wörtlich hochgeladen,
weil `/` in pCloud der Pfadtrenner ist.

---

## 20. Ausschlüsse

Immer ausgeschlossen (werden nie hochgeladen, bleiben nur auf diesem PC):

- Outlook-Datendateien `*.pst`, `*.ost`, `*.nst`
- Temporäre und Sperrdateien: `*.tmp`, `~$…` (Office), `.~lock.…` (LibreOffice), `~WRL*.tmp`, `*.crdownload`,
  `*.partial`, `*.part`, `desktop.ini`, `Thumbs.db`, `.DS_Store`

Eigene Muster in *Einstellungen → Konten → Nicht hochladen* (je Konto): Datei- **oder Ordnernamen**, mit `;` getrennt,
`*` und `?` erlaubt, z. B. `node_modules; *.vhdx; Temp; .git`. Ein ausgeschlossener Ordner wird samt Inhalt ignoriert.
Pfadangaben (`Projekte\bin`) sind nicht möglich, nur Namen.

Ausschlüsse betreffen **lokale** Elemente. Um pCloud-Ordner auf diesem PC auszublenden, dient *Ordner auswählen* (siehe
[14](#14-ordner-auswählen-selektive-synchronisation)).

---

## 21. Bandbreite und Parallelität

Alle Werte gelten für alle Konten (Einstellungen → *Synchronisation*):

- **Upload/Download höchstens (Mbit/s):** Grenze je Konto für alle seine Übertragungen gemeinsam, gilt sofort – auch für
  laufende Übertragungen und für das Laden beim Öffnen. 0 = unbegrenzt. Sinnvoll etwa 60–80 % der
  Anschlussgeschwindigkeit, damit Videokonferenzen nicht leiden; bei mehreren Konten entsprechend teilen.
- **Parallele Downloads/Uploads:** Wie viele Übertragungen je Konto gleichzeitig laufen. Eine Änderung startet die Engines
  neu.
- **Parallele Ordnerabfragen:** Nur beim ordnerweisen Einlesen großer Konten relevant. Höhere Werte (16–32) beschleunigen
  das Einlesen deutlich, belasten aber die pCloud-API. Wirkt sofort, auch während ein Einlesen läuft
  (Protokoll: „Ordnerabfragen: jetzt 28 parallel“).
- **Geänderte große Dateien blockweise hochladen** (Standard: an, ab 8 MB): Liegt eine Datei bereits in pCloud und wird
  lokal geändert, überträgt der Client nur die Blöcke, die sich unterscheiden (Blockgröße 4 MB, bei sehr großen
  Dateien mehr), und schreibt sie in die vorhandene Datei – als neue Revision, der Versionsverlauf bleibt. Die
  Blockhashes des letzten Uploads merkt er sich; fehlen sie, fragt er die Prüfsummen je Block bei pCloud ab. Zum Schluss
  prüft er die Prüfsumme der ganzen Datei gegen pCloud; weicht sie ab oder hat sich die Datei in pCloud inzwischen
  geändert, lädt er sie vollständig hoch. Das Aktivitätsfenster zeigt „Änderung blockweise hochgeladen: … (2 von 120
  Blöcken, 8 von 480 MB)“. Grenzen: Eingefügte oder entfernte Bytes verschieben alles dahinter – das wird dann neu
  übertragen; für Dateien, die ein Programm komplett neu schreibt (viele Office-Formate, komprimierte Dateien), bringt
  es nichts, kostet aber auch kaum etwas. Sinnvoll vor allem für Datenbanken, VM-Festplatten, Archive mit angehängten
  Daten, große Protokolle.

---

## 22. Automatische Pause

pCloud Sync hält die Synchronisation von selbst an, solange eine dieser Bedingungen gilt (Einstellungen →
*Synchronisation*, beide Standard: an, für alle Konten):

- **Getaktete Verbindung** – Windows stuft die Internetverbindung als getaktet ein: Mobilfunk und Smartphone-Hotspot, ein
  WLAN oder LAN, das in den Windows-Netzwerkeinstellungen *als getaktete Verbindung festgelegt* ist, ein fast oder ganz
  aufgebrauchtes Datenlimit sowie Roaming.
- **Energiesparmodus** – der Windows-Stromsparmodus ist eingeschaltet (Akku unter der eingestellten Schwelle oder von Hand
  aktiviert).

Der Client erfährt von Änderungen sofort über Windows-Ereignisse und prüft zusätzlich einmal pro Minute nach. Solange die
Bedingung gilt, zeigen Symbol und Status **Pausiert – getaktete Verbindung** bzw. **Pausiert – Energiesparmodus** (bei
beidem: „getaktete Verbindung, Energiesparmodus“); Uploads, Downloads neuer Dateien und Abgleiche warten, **Dateien
öffnen funktioniert weiterhin** (Laden bei Bedarf läuft, gedrosselt durch die Download-Grenze). Endet die Bedingung,
läuft die Synchronisation von selbst weiter („Fortgesetzt – Abgleich …“), beginnend mit einem kurzen lokalen Abgleich.
Beginnt sie bereits vor dem Start, startet das Konto pausiert.

**Trotzdem fortsetzen** im Abschnitt des Kontos übersteuert die Pause bis zum Ende der aktuellen Bedingung – zum
Beispiel, wenn der Hotspot-Tarif ohnehin reicht. Beginnt danach eine neue Bedingung (oder dieselbe erneut), pausiert das
Konto wieder. Eine **manuelle Pause** hat Vorrang: Wer selbst *Pausieren* gewählt hat, bleibt pausiert, auch wenn
Bedingungen kommen und gehen.

**Pause auf Zeit:** *Pausieren* im Abschnitt des Kontos bietet 2, 8 oder 24 Stunden sowie *Bis ich fortsetze*. Bei
einer Pause auf Zeit heißt der Menüpunkt danach „Fortsetzen (sonst automatisch um 17:30)“; nach Ablauf läuft die
Synchronisation von selbst weiter. Ein Neustart von pCloud Sync beendet jede Pause (die Uhr läuft nur in der laufenden
Sitzung); wer pausiert starten möchte, nutzt die Einstellung *Pausiert starten* (siehe [6.4](#64-allgemein)).

---

## 23. Updates

pCloud Sync prüft täglich, ob auf <https://github.com/scorsten/pCloudSync-Releases/releases> eine neuere Version liegt
(Einstellungen → *Allgemein* → *Täglich nach Updates suchen und anbieten*, Standard: an). Die erste Prüfung läuft rund zwei
Minuten nach dem Start, wenn die letzte länger als einen Tag her ist; *Nach Updates suchen …* im Tray-Menü prüft sofort.
Die Prüfung ist eine einzelne Anfrage an die GitHub-API ohne Anmeldung; übertragen wird nur die eigene Versionsnummer.

Gibt es eine neuere Version, erscheint „pCloud Sync 1.0.5 ist verfügbar“ mit den Release-Notizen:

- **Jetzt installieren** – das Setup wird nach `%LOCALAPPDATA%\PCloudSync\updates` geladen (Status: „Update wird geladen
  … 63 %“), gegen die `SHA256SUMS.txt` des Releases geprüft und still installiert. pCloud Sync beendet sich dazu selbst;
  das Setup ersetzt die Dateien, behält die Autostart-Einstellung und **startet pCloud Sync danach wieder**. Der lokale
  Zustand bleibt erhalten; nach dem Start folgt der übliche kurze lokale Abgleich. Laufende Übertragungen werden beim
  nächsten Start fortgesetzt.
- **Später** – bei der automatischen Prüfung wird diese Version nicht mehr angeboten, bleibt aber im Tray-Menü als
  *Update auf 1.0.5 installieren …* verfügbar. Bei einer manuellen Prüfung erscheint der Hinweis beim nächsten Mal wieder.

Stimmt die Prüfsumme nicht, wird das heruntergeladene Setup verworfen („Prüfsumme des Setups stimmt nicht“). Enthält ein
Release keine Prüfsummen-Datei, installiert der Client ohne Prüfung und vermerkt das im Protokoll. Während eines
Trennens wird nicht aktualisiert. Wer lieber selbst installiert, lädt das Setup von der Releases-Seite (siehe
[2](#2-installation-und-erststart)).

Eine Code-Signatur der Programmdateien ist für die Aktualisierung aus dem Client heraus nicht erforderlich; der Download
wird über die Prüfsumme abgesichert. Signiert sind die Builds nur, wenn in der CI ein Zertifikat hinterlegt ist.

---

## 24. Neu anmelden und Konto trennen

**Neu anmelden** (Zugang ersetzen): siehe [3.3](#33-neu-anmelden-zugang-ersetzen-ohne-zu-trennen). Der Ordner bleibt.

**Konto trennen** (Tray-Menü, Abschnitt des Kontos): trennt den PC vollständig von diesem Konto.

- Vollständig auf diesem PC vorhandene Dateien werden zu normalen Dateien.
- Reine Online-Dateien werden lokal entfernt (in pCloud bleibt alles erhalten).
- Der Sync-Ordner wird bei Windows abgemeldet, der gespeicherte Stand gelöscht, das Token gelöscht (bei
  E-Mail/Passwort-Anmeldung auch bei pCloud abgemeldet).
- Noch nicht übertragene lokale Änderungen und Löschungen werden **verworfen**, nicht mehr hochgeladen.
- Das Konto verschwindet aus dem Tray-Menü; andere Konten laufen weiter.

Vorher per KFM umgeleitete Ordner zurücksetzen. Bei großen Konten dauert das Trennen lange (jede Online-Datei wird
einzeln entfernt); der Fortschritt steht im Aktivitätsfenster. Während des Trennens sind Einstellungen, Anmelden und
Beenden gesperrt. Wird es dennoch unterbrochen (z. B. Neustart), setzt der Client es beim nächsten Start fort – und
synchronisiert dieses Konto bis dahin nicht, damit die entfernten Online-Dateien nie als Löschung nach pCloud gelangen.

Der Dialog bietet als Alternative direkt *Nur neu anmelden* an, falls du eigentlich nur das Anmeldeverfahren wechseln
wolltest. Ein Konto, das nie angemeldet war, heißt im Menü *Konto entfernen …* und wird nur aus der Liste genommen.

---

## 25. Fehlerbehebung

**Erste Anlaufstelle** ist immer das Aktivitätsfenster bzw. `%LOCALAPPDATA%\PCloudSync\logs`. Die letzten relevanten
Zeilen liefert:

```powershell
Select-String -Path "$env:LOCALAPPDATA\PCloudSync\logs\*" -Pattern 'WARN|ERROR|startet \(PID' | Select-Object -Last 40 | ForEach-Object Line
```

| Symptom | Ursache / Lösung |
|---|---|
| „pCloud Sync läuft bereits“ beim Start | Eine Instanz läuft (Symbol im Infobereich, ggf. hinter dem Pfeil). |
| Status **Offline – neuer Versuch folgt** | Kein Netz beim Start. Der Client versucht es automatisch erneut (20 s, 40 s, … bis 5 min), je Konto. |
| Status **Anmeldung abgelaufen – bitte neu anmelden** | Token ungültig (Passwort geändert, App widerrufen). Tray-Menü → Abschnitt des Kontos → *Anmelden …*. |
| Status **Pausiert – getaktete Verbindung**, obwohl du im WLAN bist | Windows stuft die Verbindung als getaktet ein (Einstellungen → Netzwerk und Internet → Eigenschaften der Verbindung → *Getaktete Verbindung*). Dort abschalten, *Trotzdem fortsetzen* wählen oder die Option in den Einstellungen ausschalten (siehe [22](#22-automatische-pause)). |
| Rückfrage **Verdächtige Massenänderung**, aber die Änderungen sind gewollt | *Weiter synchronisieren* – die Endungen gelten danach als bekannt. Bei wiederholten Fehlalarmen eines Programms den Schutz in den Einstellungen abschalten (siehe [17](#17-ransomware-schutz)). |
| Online-Datei lässt sich nicht öffnen | Client beendet oder offline. Client starten bzw. Verbindung prüfen. |
| Keine Vorschaubilder für Online-Dateien | Nur bei laufendem Client und nur für Bilder, Videos und Dokumente. COM-Registrierung und Eintrag am Sync-Root prüfen, Protokollzeile „Vorschaubild-Handler …“ lesen (siehe [10](#10-vorschaubilder-für-nur-online-dateien)). |
| Kontextmenü-Befehl tut nichts | Aktivitätsfenster prüfen: erscheint `Kontextmenü: …`? Wenn nicht, ist die Registrierung veraltet – Client neu starten: Er registriert das Menü beim Start neu und repariert seit 1.5.1 auch ein nach einem Update defektes Windows-11-Paket (Protokoll „Kontextmenü-Paket …“). Zustand prüfen: `Get-AppxPackage PCloudSyncClient.Shell` – `Status` muss `Ok` sein, `Version` zur App passen. |
| „… liegt nicht im pCloud-Ordner“ | Das Element liegt in keinem Sync-Ordner eines verbundenen Kontos (z. B. Konto noch nicht gestartet). |
| Rückfrage „… Elemente fehlen lokal“ obwohl nichts gelöscht wurde | *Lokal wiederherstellen* wählen. Ursachen: unterbrochenes Trennen, Laufwerk zwischenzeitlich nicht verfügbar. |
| „Ordner kann noch nicht ausgeschlossen werden“ | Unterhalb wartet noch etwas auf den Upload. Warten, bis das Konto auf *Aktuell* steht (offene Dateien schließen), dann erneut *Ordner auswählen …*. |
| `Nicht hochgeladen: „…“ ist von der Synchronisation ausgeschlossen` | Lokal angelegter Ordner heißt wie ein abgewählter pCloud-Ordner. Ordner wieder einschließen oder lokal umbenennen (siehe [14](#14-ordner-auswählen-selektive-synchronisation)). |
| „Dieses Konto ist bereits verbunden“ | Dasselbe pCloud-Konto in zwei Abschnitten. Stattdessen den pCloud-Ordner des vorhandenen Kontos anpassen. |
| „Ordner überschneiden sich“ | Die Sync-Ordner zweier Konten liegen ineinander. Jedes Konto braucht einen eigenen Ordner. |
| „Update-Prüfung nicht möglich“ | Keine Verbindung zu `api.github.com` (Proxy, Firewall). Setup von Hand von der Releases-Seite laden. |
| „Prüfsumme des Setups stimmt nicht“ | Download beschädigt oder verändert; die Datei wurde verworfen. Erneut versuchen oder von Hand laden und mit `SHA256SUMS.txt` vergleichen. |
| Eine Datei wird alle 2 Minuten erneut versucht (`Lokale Änderung nicht verarbeitet`) | Meist eine von einem Programm dauerhaft geöffnete Datei. Programm schließen; Outlook-Dateien gehören in die Ausschlüsse. |
| `Der Ordner liegt innerhalb des Sync-Roots von …` | Der gewählte Ordner liegt in OneDrive/Dropbox o. Ä. Anderen Ordner wählen. |
| `UI-Warteschlange seit … nie leer` / `UI-Thread reagiert seit …` | Diagnose der Oberfläche (Explorer stark ausgelastet). Bitte die Zeilen mit dem Protokoll melden. |
| Ordnerbaum wird bei jedem Vollabgleich abgebrochen | Seit 0.1.44 fällt der Client bei jedem Fehler auf ordnerweises Lesen zurück. Ältere Version aktualisieren. |

**Für eine Support-Anfrage:** Tray-Menü → *Über pCloud Sync …* → *Diagnoseangaben kopieren* (Version, Windows, Sprache,
Lizenzart, Zahl der Konten – ohne Zugangsdaten und E-Mail-Adressen) und die Protokolldatei des Tages mitschicken.

**Zustand zurücksetzen** (letztes Mittel, Client vorher beenden): `pCloudSync.exe --reset-state` löscht den gespeicherten
Stand **aller Konten** (`state*.db*`). Der nächste Start liest die Bäume neu; lokale Dateien bleiben erhalten und werden
per Pfad zugeordnet.

**Alles abmelden** (z. B. vor manueller Deinstallation): `pCloudSync.exe --unregister` meldet alle Sync-Roots dieses
Programms ab und entfernt das Kontextmenü. Platzhalter in den Ordnern sind danach nicht mehr lesbar – vorher je Konto
*Konto trennen*.

---

## 26. Deinstallation

1. Per KFM umgeleitete Ordner zurücksetzen (Tray-Menü → KFM → *Ausgewählte zurücksetzen*).
2. Tray-Menü → je Konto *Konto trennen* und warten, bis „Konto getrennt“ erscheint. Vollständig vorhandene Dateien bleiben
   als normale Dateien im Ordner; alles andere ist weiterhin in pCloud.
3. Windows → Apps → *pCloud Sync* deinstallieren. Die Deinstallation beendet die App, meldet alle Sync-Roots und das
   Windows-11-Shell-Paket ab.
4. Optional `%LOCALAPPDATA%\PCloudSync` löschen (Protokolle, Einstellungen, heruntergeladene Setups).

---

## 27. Sicherheit und Datenschutz

- Das Passwort wird nie übertragen und nie gespeichert. Gespeichert werden nur Tokens – je Konto eines – im Windows-
  Anmeldeinformationsspeicher, verschlüsselt mit dem Benutzerprofil.
- Alle Verbindungen zu pCloud laufen über HTTPS.
- Protokolle enthalten Dateinamen und Pfade, aber keine Inhalte, Tokens oder Passwörter.
- Der Client sendet nichts an Dritte, es gibt keine Telemetrie. Einzige Ausnahme ist die tägliche Update-Prüfung bei
  GitHub (nur die eigene Versionsnummer, abschaltbar). Setups werden nur installiert, wenn ihre SHA-256-Prüfsumme mit der
  des Releases übereinstimmt.
- Der Ransomware-Schutz liest für die Inhaltsprüfung höchstens die ersten 64 KB einer geänderten Datei und nur auf diesem
  PC; er überträgt nichts.
- Vorschaubilder holt der Client über die Datei-Id von pCloud; sie liegen kurz als temporäre PNG-Datei in `%TEMP%` und
  werden von der Shell-Erweiterung nach dem Lesen gelöscht.
- Freigabelinks sind öffentlich für jeden, der sie kennt – Ablaufdatum und Download-Limit einschränken das; Passwörter
  nur mit pCloud Premium.

---

## 28. Lizenz

Nach der Installation stehen **30 Tage lang alle Funktionen** zur Verfügung (Testphase). Danach arbeitet pCloud Sync
ohne Lizenz als **Basisversion** weiter:

| Basisversion (ohne Lizenz) | Vollversion (Testphase oder Lizenz) |
|---|---|
| Ein pCloud-Konto, Platzhalter und Synchronisation in beide Richtungen | Mehrere pCloud-Konten |
| Behalten / Speicherplatz freigeben, Cache-Obergrenze | Known Folder Move (Desktop, Dokumente & Co.) |
| Öffentlichen Link kopieren, In pCloud anzeigen | Ordnerauswahl (selektive Synchronisation) |
| Auto-Pause, Bandbreite, Ransomware- und Löschschutz | Blockweiser Upload geänderter großer Dateien |
| Statusflyout, Windows-Suche, Updates | Vorschaubilder für Nur-online-Dateien, Freigeben- und Versionen-Dialog (samt *Über Windows teilen*) |
| Konflikte lösen, Freigabe- und Konfliktsymbole in der Statusspalte | |

Wählst du in der Basisversion eine Funktion der Vollversion, fragt pCloud Sync, ob du eine Lizenz eingeben möchtest.
Weitere Konten werden nicht gestartet (das erste Konto läuft immer); bestehende KFM-Umleitungen und abgewählte Ordner
bleiben wirksam. Lokal vorhandene Dateien zeigen ihr Vorschaubild weiterhin, nur Nur-online-Dateien nicht.

**Lizenz kaufen:** Tray-Menü → *Lizenz …* → *Lizenz kaufen …* öffnet die Kaufseite im Browser: **1 PC für 19 €** oder
**3 PCs für 39 €**, jeweils Einmalkauf mit allen Updates von Version 1. Bezahlt wird über Paddle (Karte, PayPal, je nach
Land weitere Verfahren); Paddle ist der Verkäufer und schickt die Rechnung per E-Mail. Nach der Zahlung erscheint der
Lizenzschlüssel auf der Kaufseite, und pCloud Sync **aktiviert ihn von selbst** – auch wenn das Lizenz-Fenster
inzwischen geschlossen ist (die App fragt 30 Minuten lang alle 10 Sekunden, danach stündlich bis zu zwei Tage). Die
Benachrichtigung „Lizenz aktiviert“ bestätigt es. Den Schlüssel von der Kaufseite aufbewahren: Er wird für einen neuen
PC gebraucht. Wurde auf einem anderen Gerät gekauft, den Schlüssel wie unten beschrieben eingeben.

**Lizenz aktivieren:** Tray-Menü → *Lizenz …*, den Schlüssel (beginnt mit `PCS1.`) einfügen, *Aktivieren*. Eine Lizenz
gilt für eine bestimmte Zahl an PCs; pCloud Sync meldet den PC dazu beim Lizenzserver an. Ist die Zahl erreicht, nennt
die Meldung die registrierten PCs – auf einem davon *Diesen PC freigeben* wählen oder die Lizenz erweitern lassen.
Eine **Generallizenz** gilt ohne Gerätezahl und ohne Anmeldung beim Server.

**Online-Prüfung:** pCloud Sync bestätigt die Lizenz alle 30 Tage beim Lizenzserver. Ohne Internet läuft die
Vollversion bis zu 30 Tage weiter; danach gilt die Basisversion, bis die nächste Prüfung gelingt (automatisch, sobald
wieder eine Verbindung besteht). Ist der Server bei der ersten Aktivierung nicht erreichbar, wird sie nachgeholt.

**PC wechseln:** Auf dem alten PC *Lizenz …* → *Diesen PC freigeben*; danach auf dem neuen aktivieren. Ist der alte
PC nicht mehr verfügbar, kann der Anbieter die Registrierung freigeben.

Übertragen werden dabei nur der Lizenzschlüssel, eine aus der Windows-Installation abgeleitete, nicht umkehrbare
Geräte-Id, der Rechnername und die Programmversion.

---

## 29. Sprache, Hilfe und Info

**Sprache:** pCloud Sync gibt es auf Deutsch, Englisch, Französisch, Spanisch, Portugiesisch (Portugal und Brasilien),
Niederländisch, Italienisch und Türkisch. Standard ist *Automatisch*: die Anzeigesprache von Windows; für andere
Sprachen gilt Englisch, für Portugiesisch entscheidet die Region (Brasilien → pt-BR, sonst pt-PT). Fest einstellen:
*Einstellungen → Allgemein → Sprache*. Die Umstellung wirkt nach *Speichern* sofort – im Tray-Menü, in allen danach
geöffneten Fenstern, im Kontextmenü „pCloud ▸“, in der Statusspalte und im Statusflyout des Explorers. Eine bereits
angezeigte Statusmeldung eines Kontos wechselt mit der nächsten Zustandsänderung.

Nicht übersetzt werden:

- das **Protokoll** (Aktivitätsfenster, Protokolldateien) – es bleibt deutsch, damit Meldungen eindeutig vergleichbar
  sind;
- die Namen von **Konfliktkopien** – `Name (Konflikt PC Datum).ext` heißt in jeder Sprache so, damit die Kopie auf
  allen PCs erkannt wird;
- dieses **Handbuch** und der **Lizenzvertrag** (deutsches Original);
- Meldungen, die **pCloud** selbst liefert (etwa Fehlertexte der Anmeldung), erscheinen so, wie pCloud sie schickt.

Das Setup wählt seine Sprache ebenfalls nach Windows.

**Hilfe:** Tray-Menü → *Hilfe …* öffnet eine Kurzhilfe in der eingestellten Sprache mit den wichtigsten Abläufen
(Erste Schritte; Nur online, behalten, Speicher freigeben; Ordner auswählen und ausschließen; Konflikte; Freigeben und
Links; Frühere Versionen; Pausieren und Schutzfunktionen; Mehrere Konten und bekannte Ordner; Lizenz; Wenn etwas nicht
klappt). *Ausführliches Handbuch (Deutsch)* öffnet dieses Handbuch im Download-Repository
(<https://github.com/scorsten/pCloudSync-Releases/blob/HEAD/USER_MANUAL.md>).

**Über pCloud Sync:** zeigt Version und Lizenzstatus, Hinweise zu Marken und verwendeten Komponenten sowie Links zu
Downloads und Neuigkeiten, zu den Lizenzbedingungen (`EULA.txt` im Programmordner) und zum Protokollordner.
*Diagnoseangaben kopieren* legt Version, Windows-Version, Sprache, Lizenzart und Zahl der Konten in die Zwischenablage –
für Support-Anfragen, ohne Zugangsdaten, Tokens oder E-Mail-Adressen.
