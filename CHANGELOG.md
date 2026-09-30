# Changelog

Alle nennenswerten Änderungen an pCloud Sync werden in dieser Datei festgehalten.
Das Format lehnt sich an [Keep a Changelog](https://keepachangelog.com/de/1.1.0/) an, die Versionsnummern folgen
[Semantic Versioning](https://semver.org/lang/de/).

Der Abschnitt zur jeweils veröffentlichten Version wird von der CI als Beschreibung des GitHub Releases übernommen
(Überschrift `## <Version>` bis zur nächsten `## `-Überschrift).

## 1.5.1 – 2026-09-30

### Behoben

- **Kontextmenü ohne Wirkung nach einem Update** (seit dem stillen Update auf 1.4.2): Das Setup konnte das
  Windows-11-Paket des Kontextmenüs nicht aktualisieren, solange der Explorer es benutzte. Das alte Paket blieb mit den
  ersetzten Dateien als „Modified, NeedsRemediation“ registriert; Windows startete es nicht mehr, und *Freigeben …*,
  *Versionen …* usw. taten nichts. Das Setup aktualisiert das Paket jetzt erzwungen (mit Wiederholung), und pCloud Sync
  prüft beim Start Version und Zustand des Pakets und registriert es bei Bedarf selbst neu (Protokoll:
  „Kontextmenü-Paket …“, Details in `logs\shell-menu.log`).

## 1.5.0 – 2026-09-30

### Neu

- **Mehrsprachige Oberfläche:** Deutsch, Englisch, Französisch, Spanisch, Portugiesisch (Portugal und Brasilien),
  Niederländisch, Italienisch und Türkisch. Standard ist *Automatisch* – die Windows-Anzeigesprache (andere Sprachen →
  Englisch); umstellbar unter *Einstellungen → Allgemein → Sprache*, sofort wirksam (auch im Tray-Menü, im
  Explorer-Kontextmenü, in der Statusspalte und im Statusflyout des Explorers). Übersetzt sind alle Fenster, Menüs,
  Meldungen und Benachrichtigungen; das Protokoll bleibt deutsch. Das Setup wählt seine Sprache ebenfalls nach Windows.
- **Hilfe** (Tray-Menü → *Hilfe …*): Kurzhilfe in der eingestellten Sprache zu Einrichtung, Nur-online-Dateien,
  Ordnerauswahl, Konflikten, Freigaben, Versionen, Pausieren und Schutzfunktionen, mehreren Konten, Lizenz und
  Fehlersuche; dazu der Link zum ausführlichen (deutschen) Handbuch im Download-Repository.
- **Über pCloud Sync** (Tray-Menü): Version, Lizenzstatus, Hinweise zu Marken und verwendeten Komponenten, Links zu
  Downloads, Lizenzbedingungen und Protokollordner sowie *Diagnoseangaben kopieren* für Support-Anfragen (Version,
  Windows, Sprache, Lizenzart, Zahl der Konten – ohne Zugangsdaten oder E-Mail-Adressen).

### Hinweise

- Dateinamen von Konfliktkopien bleiben in jeder Sprache `Name (Konflikt PC Datum).ext`, damit sie auf allen PCs
  erkannt werden. Der Lizenzvertrag (EULA) ist der deutsche Originaltext.
- Das Benutzerhandbuch liegt jetzt auch im Download-Repository (`USER_MANUAL.md`, wird bei jedem Release aktualisiert).

## 1.4.2 – 2026-09-30

### Neu

- **Einzelne Datei nur für bestimmte Personen freigeben:** Im Freigeben-Dialog einer Datei verschiebt *In eigenen Ordner
  verschieben und freigeben …* die Datei in einen neuen Ordner mit ihrem Namen (nach Rückfrage), wartet, bis beides in
  pCloud angekommen ist, und öffnet den Freigeben-Dialog dieses Ordners – dort *Nur lesen* oder *Lesen und schreiben*.
  pCloud selbst gibt nur Ordner an Personen frei. Die Datei bleibt dieselbe (Verschieben, kein neuer Upload; Links und
  Versionen bleiben erhalten). Das geht jetzt auch für Dateien direkt im Sync-Ordner.

## 1.4.1 – 2026-09-30

### Behoben

- Freigeben-Dialog eines Ordners: pCloud beantwortet `listuploadlinks` bei manchen Anmeldearten mit 1000 „Log in
  required“, und die ganze Liste *Bestehende Freigaben* blieb mit Fehler leer. Jetzt wird jede Liste (Links, Upload-Links,
  Personen) einzeln gelesen; was pCloud verweigert, fehlt nur und wird als Hinweis genannt. Gleiches für das Lesen der
  Freigaben für die Explorer-Statusspalte.
- Vorschaubilder: Trafen die direkte Anfrage und das Vorausladen dasselbe Bild genau im Übergang, wurde es ein zweites
  Mal bei pCloud abgerufen; jetzt liest der zweite Abruf die inzwischen gefüllte Ablage.

- `tools/Deploy-LicenseServer.ps1` und `tools/Set-PaddleShop.ps1` mit UTF-8-BOM gespeichert: Windows PowerShell 5.1
  liest Skripte ohne BOM als ANSI, der Gedankenstrich „–“ wurde dabei zu einem Anführungszeichen und brach das Parsen ab.

## 1.4.0 – 2026-09-29

### Neu

- **Lizenz kaufen** direkt aus der App: *Lizenz …* → *Lizenz kaufen …* öffnet die Kaufseite des Lizenzservers
  (1 PC 19 €, 3 PCs 39 €, Einmalkauf). Bezahlt wird über Paddle als Merchant of Record (Paddle verkauft, führt die
  Umsatzsteuer ab und stellt die Rechnung aus). Nach der Zahlung stellt der Lizenzserver den Schlüssel aus, zeigt ihn
  auf der Kaufseite, und pCloud Sync holt ihn mit einem Abholschlüssel ab und aktiviert ihn selbst – auch nach
  geschlossenem Fenster oder Neustart (bis zu zwei Tage).
- Lizenzserver: Kaufseite `api/buy` (Paddle.js-Overlay), Webhook `api/paddle` (HMAC-Signatur, idempotent je
  Transaktion), Abholung `api/claim`, Support-Abfrage `api/admin/orders`; Tabelle `Orders`. Ohne Paddle-Einstellungen
  bleibt der Verkauf aus (503).
- `tools/Set-PaddleShop.ps1` setzt die Paddle-Einstellungen (Geheimnisse verdeckt abgefragt),
  `pcs-license orders <E-Mail|txn_…>` zeigt gekaufte Lizenzen.
- Hinweise der Basisversion bieten jetzt „Lizenz kaufen oder eingeben …“ an.
- Die Karte *Lizenz kaufen* erscheint nur, wenn der Lizenzserver den Verkauf anbietet (`api/shop`, mit Angeboten und
  Preisen); vorher bleibt das Lizenz-Fenster wie bisher.

## 1.3.1 – 2026-09-29

### Neu

- **Bestehende Freigaben im Freigeben-Dialog:** Oben listet der Dialog die vorhandenen Links, Upload-Links und – bei
  Ordnern – die Personen mit Zugriff bzw. offene Einladungen (Rechte, erstellt, läuft ab). *Link kopieren*, *Nur lesen* /
  *Lesen und schreiben erlauben* (Rechte einer angenommenen Freigabe ändern) und *Löschen* / *Freigabe beenden*
  (Link löschen, Freigabe entfernen, Einladung zurückziehen).
- **Links in der Statusspalte:** Dateien und Ordner mit öffentlichem Link oder Upload-Link zeigen ein Kettensymbol
  (*Öffentlicher Link*); freigegebene Ordner erkennt pCloud Sync jetzt auch über die Freigabeliste von pCloud, nicht nur
  über die Metadaten. Gelesen wird alle 15 Minuten und sofort nach Änderungen im Freigeben-Dialog.
- Bei Dateien erklärt der Freigeben-Dialog, dass pCloud nur Ordner an Personen freigibt (dort mit *Nur lesen* oder
  *Lesen und schreiben*), und bietet *Ordner „…“ freigeben …* für den enthaltenden Ordner an.

## 1.3.0 – 2026-09-29

### Neu

- **Konflikte lösen** (wie bei OneDrive): Wird eine Datei hier und in pCloud (oder auf einem anderen Gerät) gleichzeitig
  geändert, behält pCloud Sync wie bisher beide Fassungen – neu ist die Auswahl, welche bleibt. Eine Benachrichtigung
  meldet den Konflikt (Klick öffnet den Dialog), das Tray-Menü zeigt *N Konflikte lösen …*, im Explorer steht auf der
  Konfliktkopie *pCloud › Konflikt lösen …*. Im Dialog je Konflikt: **Diese Kopie behalten** (ersetzt das Original –
  dieselbe Datei, in pCloud eine neue Version, der alte Stand bleibt unter *Versionen*), **Original behalten** (Kopie
  wird gelöscht, in pCloud Papierkorb) oder **Beide behalten** (Kopie bekommt einen neutralen Namen
  `Bericht (PC 2026-09-29 1015).docx`); *Beide öffnen* zum Vergleichen. Erkannt werden auch Konfliktkopien anderer
  Rechner.
- **Freigabe- und Konfliktstatus im Explorer:** Die Spalte *Status* zeigt neben dem Synchronisationssymbol ein Symbol für
  freigegebene Ordner und ihren Inhalt – *Von dir freigegeben*, *Für dich freigegeben* bzw. *… – nur lesen* (Tooltip) –
  und ein Warnsymbol auf Konfliktkopien. Umgesetzt als CustomStateHandler der Shell-DLL; die App trägt die
  freigegebenen Ordner in `HKCU\Software\PCloudSync\Shell` (`Shares`) ein, der Explorer fragt dafür nichts über die
  Befehls-Pipe ab. Nach dem Update fragt Windows einmalig per UAC, um den Handler am Sync-Root einzutragen.
- **Über Windows teilen:** Im Freigeben-Dialog öffnet *Über Windows teilen …* das Teilen-Fenster von Windows (E-Mail,
  Outlook, Teams, Nearby Sharing, …) mit dem pCloud-Link – Ablauf, Download-Limit und Passwort gelten wie gewählt.

## 1.2.2 – 2026-09-29

### Geändert

- Vorschaubilder erscheinen beim Scrollen sofort: Explorer fragt Vorschaubilder nur für sichtbare Dateien an. pCloud
  Sync füllt deshalb nach dem Vorausladen im Hintergrund den Vorschaubild-Cache von Windows (`IThumbnailCache`,
  dieselben `thumbcache_*.db`, die Explorer liest) für die umliegenden Dateien – beim Scrollen findet Explorer die Bilder
  dort und zeigt sie ohne Nachladen, wie bei lokalen Dateien. Das Fenster ist größer (300 Dateien danach, 100 davor).

## 1.2.1 – 2026-09-29

### Geändert

- Vorschaubilder in großen Ordnern: Statt nur die ersten 400 Dateien eines Ordners vorauszuladen, lädt jede Anfrage die
  Umgebung der gerade angezeigten Datei (150 danach, 50 davor in Namensreihenfolge) mit 10 gleichzeitigen Abrufen. Neue
  Anfragen haben Vorrang, beim Scrollen durch Ordner mit Tausenden Bildern bleibt der sichtbare Bereich vorn. Neuer
  Index auf Ordner und Namen in der Zustandsdatei.

## 1.2.0 – 2026-09-29

### Neu

- **Lizenzierung.** Nach der Installation 30 Tage alle Funktionen (Testphase), danach ohne Lizenz die Basisversion:
  ein pCloud-Konto, Platzhalter und Synchronisation in beide Richtungen, Behalten/Freigeben, Links, Auto-Pause,
  Bandbreite, Ransomware- und Löschschutz, Updates. Zur Vollversion gehören außerdem mehrere Konten, Known Folder Move,
  Ordnerauswahl, blockweiser Upload, Vorschaubilder für Nur-online-Dateien sowie der Freigeben- und Versionen-Dialog.
  Tray-Menü → *Lizenz …*: Status, Schlüssel eingeben, *Diesen PC freigeben*. Weitere Konten laufen nur in der
  Vollversion (das erste Konto immer); bestehende KFM-Umleitungen und Ordnerausschlüsse bleiben in der Basisversion
  wirksam.
- **Lizenzen mit Gerätezahl:** Ein Lizenzschlüssel (signiert, Präfix `PCS1.`) gilt für eine Anzahl PCs. Der
  Lizenzserver (Azure Function + Table Storage) zählt die registrierten PCs (Geräte-Id aus der Windows-MachineGuid,
  gehasht) und stellt einen signierten Beleg aus, den die App alle 30 Tage erneuert; bis zu 30 Tage ohne Internet sind
  unkritisch. Ein freigegebener PC gibt seinen Platz zurück. **Generallizenzen** gelten ohne Gerätezahl und ohne
  Online-Registrierung.
- **Lizenzgenerator** `pcs-license` (Kommandozeile, CI-Artefakt): Schlüsselpaare anlegen (`init`), Lizenzen
  (`create --seats N [--expires]`) und Generallizenzen (`master`) erzeugen, Schlüssel prüfen (`show`), registrierte PCs
  anzeigen/freigeben und Lizenzen sperren (`devices`, `release`, `revoke`). `tools/Deploy-LicenseServer.ps1` richtet den
  Lizenzserver in Azure ein und setzt die Build-Variablen. Die privaten Schlüssel bleiben auf dem PC des Anbieters bzw.
  in den App-Einstellungen der Function App.

### Geändert

- **.NET 10 (LTS):** App, Tests, Lizenzserver (Azure Functions, isolierter Worker) und Lizenzgenerator laufen auf
  .NET 10; .NET 8 erreicht am 10.11.2026 das Supportende. Das Setup bleibt eigenständig (keine Runtime-Installation nötig).
- **Schnelleres Laden großer Dateien:** Fordert Windows einen Bereich ab 32 MB an (Kopieren, „Immer behalten“, Videos),
  lädt pCloud Sync ihn über bis zu vier parallele Range-Verbindungen statt über eine – wie OneDrive. Jedes Teilstück
  prüft die Dateigröße gegen den Platzhalter; scheitert eines, werden die übrigen abgebrochen und nur die fehlenden
  Reste als Fehler gemeldet.

## 1.1.5 – 2026-09-29

### Geändert

- Vorschaubilder schneller: Die erste Anfrage in einem Ordner lädt die Vorschauen aller übrigen Nur-online-Bilder,
  -Videos und PDFs des Ordners parallel voraus (6 gleichzeitig, höchstens 400 je Ordner); Explorers folgende Anfragen
  kommen sofort. Geladene Vorschauen liegen unter `%LOCALAPPDATA%\PCloudSync\thumbs` (Schlüssel: pCloud-Inhaltshash und
  Größe, höchstens 200 MB, die am längsten unbenutzten zuerst entfernt) und überstehen Neustarts und das Leeren des
  Explorer-Caches. Gleichzeitige Anfragen für dasselbe Bild teilen sich einen Abruf; bricht Explorer ab, landet das Bild
  trotzdem in der Ablage. Dateien ohne Vorschau werden 30 Minuten nicht erneut angefragt.

## 1.1.4 – 2026-09-29

### Behoben

- Lokal vorhandene Dateien im Sync-Ordner zeigten kein Vorschaubild: Der Handler lehnte sie ab, damit Explorer seine
  normalen Handler nimmt – das tut Explorer bei Sync-Roots aber nicht. Jetzt reicht die Shell-DLL die Anfrage an den
  für die Dateiendung registrierten Vorschaubild-Handler weiter (Fotos, Videos, PDFs …) und dekodiert Bilder ohne
  registrierten Handler selbst (WIC). pCloud Sync muss dafür nicht laufen.

## 1.1.3 – 2026-09-28

### Behoben

- Vorschaubilder für Nur-online-Dateien: Der Client forderte PNG an (`type=png`); die Vorschau-Server von pCloud
  beantworteten das mit 5002 bzw. HTTP 503, während die Weboberfläche (JPEG) Vorschaubilder zeigte. Jetzt kommt JPEG,
  die Shell-DLL erkennt das Format selbst.

### Geändert

- Vorschaubilder: Antwortet pCloud mit 503 bzw. 5xxx (Vorschau wird erst erzeugt), fragt der Client innerhalb des
  Zeitrahmens von Explorer zweimal nach (nach 0,4 s und 1,2 s); die App wartet 4,5 s statt 4 s.

## 1.1.2 – 2026-09-28

### Behoben

- Vorschaubilder für Nur-online-Dateien blieben leer: Der API-Host von pCloud beantwortet `getthumb` teils mit Fehler
  5002 („no servers available“). Der Client holt das Bild dann über `getthumblink` vom Content-Server – wie bei
  Downloads – und bleibt für die Sitzung bei diesem Weg (Info-Zeile im Protokoll).
- Der Crypto-Tresor `Crypto Folder` im pCloud-Stammordner wird auch ohne eingerichtetes Crypto (pCloud setzt das Flag
  `encrypted` erst dann) nicht mehr synchronisiert und nicht in der Ordnerauswahl angeboten; eine vorhandene lokale
  Kopie wird entfernt (in pCloud bleibt er unberührt).
- Eine `desktop.ini` über 1 MB (in pCloud z. B. durch einen verunglückten Upload eines anderen Programms) lädt Explorer
  nicht mehr beim Öffnen des Ordners komplett herunter: Explorer erhält „Zugriff verweigert“, einmalige Warnung im
  Protokoll. Öffnen oder Kopieren der Datei durch andere Programme lädt sie weiterhin.

## 1.1.1 – 2026-09-28

### Geändert

- Signatur über Azure Artifact Signing ist in der CI eingerichtet, griff für diesen Build aber noch nicht (Zugangsdaten
  unvollständig) – 1.1.1 ist wie 1.1.0 unsigniert.
- Setups und Updates kommen aus dem öffentlichen Download-Repository
  [scorsten/pCloudSync-Releases](https://github.com/scorsten/pCloudSync-Releases). Installationen bis 1.0.1 finden
  dieses Update nicht von selbst (sie fragen das private Quell-Repository ab) – einmal von Hand installieren.

## 1.1.0 – 2026-09-28

### Neu

- **Blockweiser Upload geänderter großer Dateien** (Einstellungen → Synchronisation, standardmäßig an ab 8 MB): Statt die
  ganze Datei erneut zu übertragen, werden nur geänderte Blöcke über die Datei-API von pCloud (`file_open`,
  `file_checksum`, `file_pwrite`, `file_truncate`, `file_close`) in die bestehende Datei geschrieben – als neue Revision,
  der Versionsverlauf bleibt. Blockgröße 4 MB (bei sehr großen Dateien bis 64 MB, höchstens 1024 Blöcke); die Blockhashes
  des letzten Uploads liegen in `state.db` (Tabelle `blocks`), sonst fragt der Client die Prüfsummen je Block beim Server
  ab. Nach dem Schreiben wird der SHA-1 der ganzen Datei gegen pCloud geprüft; bei jeder Abweichung folgt der vollständige
  Upload. Eingefügte oder entfernte Bytes verschieben alles dahinter, das wird dann neu geschrieben (Protokoll:
  „Änderung blockweise hochgeladen: … (n von m Blöcken, x von y MB)“).
- **Pause auf Zeit:** *Pausieren* bietet 2 Stunden, 8 Stunden, 24 Stunden oder „Bis ich fortsetze“; *Fortsetzen* zeigt
  den Zeitpunkt der automatischen Fortsetzung („Fortsetzen (sonst automatisch um 17:30)“).
- **Speicherplatz** des Kontos in der Kopfzeile des Tray-Menüs und im Tooltip („3,2 von 5,4 TB belegt“), stündlich
  aktualisiert.
- **Papierkorb (Browser)** und **Rewind – Zeitpunkt wiederherstellen (Browser)** im Tray-Menü je Konto (pCloud-Papierkorb
  bzw. Rewind auf my.pcloud.com, passend zum Rechenzentrum des Kontos).
- **Freigabe-Anfragen:** Lädt jemand ein pCloud-Konto zu einem Ordner ein, erscheint eine Benachrichtigung („… möchte
  „Projekt“ mit dir teilen“); ein Klick öffnet die Freigaben in pCloud zum Annehmen.
- **Nur-Lese-Freigaben:** In Ordnern, die andere nur zum Lesen freigegeben haben, werden Dateien mit Schreibschutz
  angelegt; lokale Änderungen darin werden nicht hochgeladen (einmalige Warnung je Datei im Protokoll statt endloser
  Fehlversuche).
- **pCloud Crypto** wird übersprungen: Clientseitig verschlüsselte Ordner erscheinen weder lokal noch in der
  Ordnerauswahl (Namen und Inhalte wären nur Chiffrat).
- **Windows-Suche:** Der Sync-Ordner wird beim Verbinden in den Suchindex aufgenommen und beim Trennen wieder entfernt
  (Suche im Explorer und im Startmenü findet auch Nur-online-Dateien nach Namen).
- **Statusflyout des Explorers** (Windows 11 ab Build 23504): Ein Klick auf das Wolkensymbol in der Adressleiste zeigt
  Zustand, Speicherplatz und Aktionen (pCloud im Browser öffnen, Pausieren/Fortsetzen, Aktivität und Protokoll,
  Einstellungen). Setzt das signierte Windows-11-Paket voraus; ohne laufende App meldet das Flyout „pCloud Sync läuft
  nicht“.
- **Paket-Handler wie bei Dropbox:** Das Windows-11-Paket deklariert die Cloud-Files-Handler (Vorschaubilder, Zustände,
  Eigenschaften, Statusflyout) im Manifest, und der Sync-Root erhält den `AUMID` des Pakets – Windows findet die
  Handler so über das Paket und nicht nur über die Registry-Einträge.
- **Download-Repository:** Releases erscheinen zusätzlich im öffentlichen Repository `scorsten/pCloudSync-Releases`
  (Setup, `SHA256SUMS.txt`, Changelog – keine Quellen); die Update-Prüfung der App fragt dieses Repository ab, damit
  sie auch bei privatem Quell-Repository funktioniert. Das Setup zeigt einen Endnutzer-Lizenzvertrag (`EULA.txt`).
- **Öffentlich vertrauenswürdige Signatur** (CI): Mit den Repository-Secrets/-Variablen für *Azure Artifact Signing*
  werden Programmdateien, Windows-11-Paket und Setup mit einem öffentlich vertrauenswürdigen Zertifikat signiert (der
  Publisher des Pakets wird aus dem Zertifikat übernommen). Das Setup braucht dann keine UAC-Abfrage mehr für das
  Zertifikat, SmartScreen kennt den Herausgeber, und ein früher registriertes Paket mit dem selbst ausgestellten
  Zertifikat wird beim Update ersetzt. Ohne diese Einstellungen bleibt es bei den bisherigen PFX-Secrets.

### Geändert

- Die lokale Änderungszeit einer synchronen Datei wird auf die Millisekunde genau gespeichert (`state.db`, Spalte
  `lmod`); der schnelle „unverändert“-Test vergleicht exakt statt mit ±2 s Toleranz. Bestehende Zustandsdateien werden
  beim ersten Abgleich nachgezogen; bis dahin gilt für alte Einträge ein Fenster von einer Sekunde nach der
  pCloud-Änderungszeit.
- Die Shell-DLL wird als C++20 gebaut (C++/WinRT für das Statusflyout).

## 1.0.1 – 2026-09-28

### Behoben

- Kontextmenü „pCloud ▸“ erschien im klassischen Menü doppelt, wenn das Windows-11-Paket installiert ist: Der
  Paket-Befehl gilt für das neue und das klassische Menü, der zusätzliche Registry-Eintrag entfällt jetzt, sobald das
  Paket registriert ist.
- Vorschaubilder für Nur-online-Dateien: Der Handler wird beim Start als COM-Klasse des Benutzers registriert
  (`HKCU\Software\Classes\CLSID`, unabhängig vom Paket) und der Eintrag `ThumbnailProvider` am Sync-Root wird
  zuverlässig geschrieben – direkt oder, falls Windows den Schreibzugriff verweigert, einmalig nach einer UAC-Abfrage
  (eine Ablehnung wird gemerkt). Das Ergebnis steht auf Info-/Warn-Stufe im Protokoll statt nur im ausführlichen
  Protokoll; ein unwirksamer HKCU-Spiegel des Eintrags wird entfernt.
- Protokolldateien beginnen mit UTF-8-BOM, damit Windows PowerShell und Editor Umlaute richtig lesen.

### Geändert

- Ohne laufende App kein Kontextmenü „pCloud ▸“ mehr (wie bei Dropbox): *Beenden* entfernt den klassischen Eintrag und
  die Registrierung des Vorschaubild-Handlers, der nächste Start legt sie wieder an; das Paket-Menü blendet sich
  außerdem aus, sobald die Anwendung nicht läuft (auch nach einem Absturz). Die Sync-Root-Registrierung – und damit
  die Cloud-Statussymbole – bleibt bewusst bestehen: Ein abgemeldeter Sync-Root macht Nur-online-Dateien unzugänglich
  (Windows entfernt die Platzhalter), und der nächste Start müsste sie für lokale Löschungen halten.

## 1.0.0 – 2026-09-28

### Neu

- Mehrere pCloud-Konten gleichzeitig („Konto hinzufügen …“ im Tray-Menü): Jedes Konto hat einen eigenen Sync-Ordner,
  pCloud-Ordner, eigene Speicher-Optionen und Ausschlüsse, eine eigene Zustandsdatei (`state-<Kennung>.db`) und einen
  eigenen Zugang im Anmeldeinformationsspeicher (`PCloudSync:account:<Kennung>`). Das Tray-Menü zeigt je Konto einen
  Abschnitt, das Kontextmenü gilt in allen Sync-Ordnern, KFM fragt nach dem Zielkonto, das Aktivitätsfenster fasst
  alle Konten zusammen. Ein Konto kann nur einmal verbunden werden. Bestehende Installationen werden beim ersten Start
  automatisch übernommen (bisheriges Konto = Profil `default`; Zugang und Zustandsdatei behalten ihre Namen).
- Selektive Synchronisation („Ordner auswählen …“ je Konto): pCloud-Ordner im Baum abwählen, Unterordner werden beim
  Aufklappen geladen. Abgewählte Ordner werden lokal entfernt (in pCloud bleibt alles erhalten), wieder angehakte aus
  pCloud nachgezogen. Der Ausschluss hängt an der Ordner-Id und überlebt Umbenennungen; er wird abgelehnt, solange
  darunter noch etwas auf den Upload wartet. Ein lokal angelegter Ordner mit dem Namen eines abgewählten pCloud-Ordners
  wird nicht hochgeladen (Warnung im Protokoll).
- Auto-Pause bei getakteter Verbindung (Mobilfunk, Hotspot, „getaktet“ in den Windows-Einstellungen, Datenlimit,
  Roaming) und im Energiesparmodus – beide Optionen unter Einstellungen → Synchronisation, standardmäßig an. Die
  Synchronisation läuft automatisch weiter, sobald die Bedingung endet; Dateien öffnen funktioniert währenddessen.
  „Trotzdem fortsetzen“ übersteuert bis zum Ende der Bedingung, eine manuelle Pause hat Vorrang.
- Ransomware-Schutz (standardmäßig an): Heuristik über die lokal geänderten Dateien der letzten 10 Minuten –
  Umbenennungen in unbekannte Dateiendungen, angehängte Endungen (`bericht.docx.locked`), verschlüsselt wirkender
  Inhalt in normalerweise unkomprimierten Dateitypen. Ab 30 Änderungen mit 60 % Auffälligen, einer unbekannten Endung
  an 30 Dateien oder 25 verschiedenen unbekannten Endungen hält pCloud Sync die Uploads an und fragt: „Pausieren und
  PC prüfen“ oder „Weiter synchronisieren“. Als bekannt gelten eine eingebaute Liste, alle Endungen des Kontos und
  gelernte Endungen; Laden bei Bedarf und Änderungen aus pCloud laufen bei Verdacht weiter.
- Vorschaubilder für Nur-online-Dateien im Explorer, ohne die Datei zu laden: Thumbnail-Handler in der Shell-DLL
  (COM-Klasse im Windows-11-Paket, als `ThumbnailProvider` am Sync-Root eingetragen), Bild von pCloud (`getthumb`)
  über die laufende App. Nur mit dem signierten Windows-11-Paket; der Handler-Eintrag am Sync-Root ist Best Effort.
- Auto-Update über GitHub Releases: tägliche Prüfung (Einstellungen → Allgemein, abschaltbar) und „Nach Updates
  suchen …“ im Tray-Menü. Das Setup wird nach `%LOCALAPPDATA%\PCloudSync\updates` geladen, gegen `SHA256SUMS.txt`
  des Releases geprüft und still installiert
  (`/SILENT /SUPPRESSMSGBOXES /NORESTART /CLOSEAPPLICATIONS /RESTARTAPPLICATIONS`); nach der Aktualisierung startet
  das Programm automatisch neu. „Später“ überspringt die angebotene Version, sie bleibt als „Update auf …
  installieren …“ im Menü.
- Signierte Builds: Programmdateien und Setup werden in der CI mit Zeitstempel signiert, sobald ein Zertifikat
  (Secrets `CODESIGN_PFX` und `CODESIGN_PFX_PASSWORD`) hinterlegt ist; ohne Zertifikat bleibt der Build unsigniert.
- Neue Tests: zwei Konten in einem Prozess, selektive Synchronisation, Auto-Pause und Ransomware-Muster (End-to-End);
  Kontoprofile, Update-Prüfung und -Download, Systembedingungen, Ransomware-Heuristik und Vorschaubilder (Unit);
  Vorschaubild-Handler der Shell-DLL (DLL-Test).

### Geändert

- Versionsschema: Releases heißen ab jetzt `X.Y.Z` (Git-Tag `vX.Y.Z`), CI-Builds `1.0.<Laufnummer>`. Ein Tag `vX.Y.Z`
  erzeugt ein GitHub Release mit Setup, `SHA256SUMS.txt` und dem Abschnitt dieser Version aus `CHANGELOG.md`.
- Jedes Release und jedes CI-Artefakt liefert neben dem Setup eine `SHA256SUMS.txt` zur Prüfung des Downloads.
- Einstellungen: neuer Bereich „Konten“ (je Konto Lokaler Ordner, pCloud-Ordner, Neue Dateien, Max. Cache, Rückfrage
  ab Löschungen, Nicht hochladen); „Anmeldung“, „Synchronisation“ und „Allgemein“ gelten für alle Konten. Der bisherige
  Bereich „Speicherplatz“ ist im Bereich „Konten“ aufgegangen.
- Kontextmenü: die `AppliesTo`-Bedingung und die Konfiguration der Shell-DLL (`SyncRoots`, REG_MULTI_SZ) umfassen alle
  Sync-Ordner; Befehle gehen an das Konto, zu dessen Ordner das Element gehört.
- Rückfragen (viele Löschungen, Ransomware-Verdacht) nennen das betroffene Konto im Titel.
- Ein nie angemeldetes Konto lässt sich über „Konto entfernen …“ aus der Liste nehmen, ohne zu trennen.
- `--reset-state` löscht die Zustandsdateien aller Konten (`state*.db*`).
- CI: End-to-End-Tests laufen mit `--blame`, das Engine-Protokoll der Tests landet als Datei im Artefakt
  `test-results`.
- Dokumentation (README, Benutzerhandbuch, technische Referenz) auf Stand 1.0.0 mit Abschnitten zu allen neuen
  Funktionen.

### Behoben

- Dokumentation: ein versehentliches Blockzitat in der technischen Referenz entfernt; senkrechte Striche in
  Tabellenzellen maskiert.

## 0.1.48

Stand vor 1.0: nativer Windows-Sync-Client für pCloud auf Basis der Cloud Files API mit Platzhaltern
(Dateien nur online, bei Bedarf lokal, immer behalten), Known Folder Move für Desktop, Dokumente, Bilder, Musik,
Videos und Downloads, Kontextmenü im Explorer (klassisch und Windows 11), Freigaben (öffentliche Links, Ordner
teilen, Upload-Links), Dateiversionen mit Wiederherstellung, Speicherverwaltung (Cache-Obergrenze, Platz freigeben,
Bandbreitenbegrenzung) sowie Benutzerhandbuch und technische Referenz.
