# WTC - Word Template Corrector

## Deutsch
### Was ist WTC?
Erzeugt man Dokumente mit Microsoft Word auf Basis von Vorlagen, so wird im
Word-Dokument auch der Pfad die dieser Vorlage gespeichert. In Unternehmen
wird oft eine gemeinsame Sammlung von Vorlagen, die auf einem Server liegt,
genutzt. So weit so gut.

### Das Problem
�ndert sich nun der Pfad zum Vorlagenverzeichnis wird die Sache unangenehm.
Insbesondere die �nderung der Serverangabe ist problematisch. Zum besseren 
Verst�ndnis hier mal ein Beispiel:

* Alter Pfad zu den Vorlagen: \\\\alter-server\\share\\templates\\
* Neuer Pfad zu den Vorlagen: \\\\neuer-server\\share\\templates\\
* Der alte Server existiert nicht mehr.

�ffnet man nun ein Word-Dokument, dass mit einer Vorlage vom alten Server
erzeugt wurde, versucht Word die Vorlage von dort zu laden. Da der Server
aber gar nicht mehr existiert, versucht Word das so lange, bis es in einen
Timeout l�uft. Und das kann leider gef�hlt, sehr lange dauern.

### Die L�sung
Die L�sung ist naheliegend. In den betroffenen Dokumenten muss **nur** der
Pfad zur Dokumentenvorlage ge�ndert werden. Dazu findet man im Netz viele
L�sungen, die aber fast alle eines gemein haben:
* basieren auf VBA und laufen innerhalb von Word oder Excel,
* sind langsam,
* verarbeiten keine Verzeichnisb�ume
* geben keine vern�nftigen Fehlermeldungen aus,
* k�nnen nicht ad�quat von der Kommandozeile genutzt werden.

WTC macht das anders, was allerdings zu einer Einschr�nkung f�hrt. **WTC
funktioniert nur f�r Dokumente im Format Office Open XML, das mit Office 2003
eingef�hrt wurde. Die gebr�uchlichen Dateiendungen sind .docx, dotx, docm und
dotm.** Diese Dateien sind im Grunde eine Sammlung von
XML-Dateien, die in ein ZIP-Archiv verpackt sind. Ja tats�chlich. Man kann die
Dokumente ganz einfach mit einem Programm wie 7Zip �ffnen oder entpacken.

### So arbeitet WTC
Das Prinzip ist simpel. Datei f�r Datei wird entpackt und dann wird in der
Einstellungsdatei `word\_rels\settings.xml.rels` wird die alte und unerw�nschte
Pfadangabe zur Vorlage durch die neue, korrekte ersetzt. Danach wird alles
wieder eingepackt und die Originaldatei ersetzt. Standardm��ig wird dabei eine
Sicherungskopie des Originals mit der Endung .bak erzeugt.

### Die Optionen
* `--help`  
Ausgabe der m�glichen Optionen
* `-d, --directory` (required)  
Alle Dokumente in diesem Verzeichnis werden bearbeitet.
* `-o, --old` (required)  
Pfad oder Teil des Pfades zur Vorlage bzw. dem Vorlagenverzeichnis der ersetzt werden soll.
* `-n, --new` (required)  
Durch diesen Pfad oder Teilpfad wird ersetzt.
* `-r, recursive` (optional)  
Bearbeite auch die Dokumente in den Unterverzeichnissen.
* `-b, --nobackup` (optional)  
Es werden **keine** Sicherungsdateien f�r korrigierte Dokumente angelegt.
* `-t, --dry-run` (optional)  
Testmodus benutzen. Es wird nach zu �ndernden Dokumenten gesucht, sie werden aber nicht ver�ndert.
* `-v, --verbose` (optional)  
Es werden ausf�hrlichere Fehlermeldung ausgegeben.

#### Beispiel
`wtc -d \\server\share\documents -o \\alter-server\share\templates\ -n \\server\share\templates\ -r`


## English

Translation following soon.