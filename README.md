
# Taktischer Einsatzmittel Code (TEM-Code)
Der Taktische Einsatzmittel Code ist eine technische Spezifikation zur Datenübertragung von Informationen über Einsatzmittel im Bereich der Behörden und Organisationen mit Sicherheitsaufgaben als auch aller weiteren Einheiten im Katastrophenschutz. Der TEM-Code soll es ermöglichen auch in großen Lagen schnell Lagerelvante Einsatzmitteldaten bereitzustellen.

## Anwendungsbereich
Der TEM-Code kommt immer dann zum Einsatz, wenn Einsatzleitungen taktische Informationen zu Einsatzmitteln, also u.a. Fahrzeuge benötigen. Dies ist insbesondere bei der Registrierung von überörtlichen Einsatzmitteln z.B. in Bereitstellungsräumen oder Einsatzabschnitten relevant.

## Funktionsweise
Der TEM-Code wird von den Organisationen, die Einsatzmittel - z.B. Einsatzfahrzeuge - besitzen, in Form eines QR Codes erstellt und im Einsatzfall mitgeführt. 

Kommt das Einsatzmittel nun zu einem Einsatz, so wird es in der Regel im Bereitstellungsraum oder spätestens im Einsatzabschnitt registriert. Dabei werden alle - für den Einsatz notwendigen - Daten erfasst. Bisher erfolgte dies händisch mit Listen. In Zukunft kann die Einsatzleitung die Erfassung durch Scannen des TEM-Codes deutlich beschleunigen und vereinfachen.

## Statischer und dynamischer TEM-Code
Der TEM-Code enthält grundsätzlich statische Informationen, wie z.B. KFZ Kennzeichen, Fahrzeugart, Rufname, TETRA-Funkinformationen, Treibstoff, etc.
Diese Informationen sind für die Einsatzleitung von zentraler Bedeutung, da dadurch eine schnelle Erfassung und Integration in die Lageführung erfolgen kann. Diese statischen Informationen können auch ohne weitere Hilfsmittel bereits im Vorhinein durch die Behörden und Organisationen als QR Code ausgedruckt und in den Einsatzmitteln mitgeführt werden.

Als Erweiterung ist es auch möglich, mit technischen Hilfsmitteln, wie Smartphones oder Tablets einen dynamischen TEM-Code zu erzeugen. Dieser enthält sämtliche statischen Daten als auch eine Erweiterung um dynamische Daten, wie z.B. das Personal oder aktuelle zusätzliche Beladungen.

## Notwendige Ausstattung
Zum Erstellen von statischen TEM-Codes ist keine besondere technische Ausstattung notwendig. Ein Online-TEM-Code Generator kann genutzt werden, um den QR Code zu erzeugen. Dieser kann auf Papier ausgedruckt ganz einfach im Einsatzmittel mitgeführt und bei Bedarf vorgezeigt werden.

Für die Erstellung von dynamischen TEM-Codes sind Apps für Smartphone oder Tablet notwendig, in denen alle relevanten Daten erfasst werden und die den dynamischen Code generieren und anzeigen können. Dieser dynamische TEM-Code kann dann im Einsatzfall erzeugt und dargestellt werden.

Zum Lesen und Verarbeiten der TEM-Codes benötigt die Einsatzleitung eine App oder ein Lageführungssystem, dass das Auslesen und Verarbeiten von TEM Codes unterstützt. Eine Liste der aktuell verfügbaren Produkte ist im Anhang zu finden.

# Datenstruktur

Die Datenstruktur kann dem [JSON Schema](tem-code.schema.json) entnommen werden. Alle dort enthaltenen Felder sind - bis auf den Rufnamen - optional. Je mehr Felder gefüllt werden, um so mehr Daten stehen der Eisnatzleitung im Einsatzfall für den taktisch sinnvollen Einsatz des Einsatzmittels zur Verfügung.

## Technischer Ablauf für die Kodierung

Um einen TEM-Code zu erzeugen, sind die folgenden Schritte notwendig.

1. Erstellen des JSON Objektes gemäß der [JSON Schema Definition](tem-code.schema.json)
2. Umwandeln des JSON Objektes in seine Binärform mit Hilfe der ["Concise Binary Object Representation"](https://cbor.io/) nach [RFC 8949](https://www.rfc-editor.org/rfc/rfc8949)
3. Komprimieren der binären Daten mit Hilfe des Deflate Algorithmus nach [RFC 1951](https://datatracker.ietf.org/doc/html/rfc1951). Dazu kann z.B. die Zlib Bibliothek verwendet werden, die in vielen Programmiersprachen verfügbar ist.
4. Die komprimierten Daten werden schließlich Base45 kodiert, um die Kapazität des QR Codes optimal nutzen zu können. [RFC 9285](https://datatracker.ietf.org/doc/rfc9285/)
5. Erstellung des QR Codes mit dem Base45 Text mit einer beliebigen Bibliothek zur QR Code Generierung

```mermaid
%%{init: {"flowchart": {"htmlLabels": false}} }%%
flowchart LR
    json["Create JSON"]
    cbor["CBOR Encoding"]
    zlib["ZLIB Compression"]
    base45["Base45 Encoding"]
    qr["Generate QR Code"]
    json --> cbor --> zlib --> base45 --> qr
```

## Technischer Ablauf für die Dekodierung

Für die Dekodierung des QR Codes, wird der gleiche Ablauf, wie unter [Kodierung](#technischer-ablauf-für-die-kodierung) beschrieben, allerdings in umgekehrter Reihenfolge mit den jeweiligen Umkehrfunktionen durchgeführt.

```mermaid
%%{init: {"flowchart": {"htmlLabels": false}} }%%
flowchart LR
    json["JSON Object"]
    cbor["CBOR Decoding"]
    zlib["ZLIB Decompression"]
    base45["Base45 Decoding"]
    qr["Read QR Code"]
    qr --> base45 --> zlib --> cbor --> json 
```
## Referenzimplementierung

Eien Referenzimplementierung mittels JavaScript/TypeScript wird in Kürze hier verfügbar sein.


# Produkte mit TEM-Code Unterstützung

| Name | Systemart | URL |
| - | - | - |
| REV+ | Einsatzführungssystem | https://www.einsatzverwaltung.de |
