# Ghost-Net-Fish
Programmierung von industriellen Informationssysteme mit Java EE

01.02.2026
Die Aufgabenstellung besteht aus der Entwicklung eines Prototypes zur Meldung von ungenutzen Fischernetzen. 
Dazu konnte ich folgende Stakeholder finden:
- Meldende
- Bergende
- interdisziplinären Teams aus vielen Entwickler:innen

Daten, die gespeichert werden müssen:
Mit * gekennzeichnet sind Pflichtfelder

Meldende: 
- *Name
- *Telefonnummer für Verschollene Netze
- Telefonnummer für Meldung
  
Bergende
- *Name
- Telefonnummer
- Anzahl der geborgenen Netze
  
Eigenschaften eines Netzes
- Standort (GPS-Koordinaten),
− geschätzte Größe und
− Status:
      − Gemeldet
       (Eine meldende Person hat das Geisternetz im System erfasst.)
      − Bergung bevorstehend
       (Eine bergende Person hat die Bergung angekündigt.)
      − Geborgen
       (Eine bergende Person hat das Geisternetz erfolgreich geborgen. KANNN NUR VON EINER PERSON ALS GEBORGEN GEMELDET WERDEN)
      − Verschollen
       (Eine beliebige Person hat festgestellt, dass das Geisternetz am gemeldeten Standort nicht auffindbar ist.)
 
Desweiteren muss darüber entschieden werden welche 5 Anforderungen für die Fallstudie umgesetzt werden sollen.
Nach dem Prinzip der Logik habe ich mich für folgenden Anforderungen entschieden:
- MUST 1: Geisternetze erfassen (anonym)
  --> Das ist der Einstieg in das Formular, ohne meldende Person keine Netze
- MUST 2: Für Bergung eintragen
  --> Damit stelle ich eine Beziehung zwischen Netz und Bergenden/ Meldenden her
- MUST 3: Offene Geisternetze anzeigen
  --> Das ist eine Statusanzeige
- MUST 4: Als geborgen melden
  --> Ein Statuswechsel
- COULD 7: Als verschollen melden
  --> Ein Statuswechsel,  um die FUnktion des Statuswechsels zu testen.

Außerdem sind die ersten 4 Punkte Musts. Ist vielleicht eine Fangfrage. Ilusion der Kontrolle. 

Insgesamt ergibt sich daraus: Erfassen → Eintragen → Anzeigen → Status ändern → alternative Status-Option 

Wie hängt das jetzt alles zusammen? 
Datenmodell: 
Person
- id (Primärschlüssel)
- name
- telefonnummer*/ telefonnummer   (bei anonymen Meldern leer)

 (melder_id)  0..1
Geisternetz --------------------> Person
- id (Primärschlüssel)                           1
- gpsLat*
- gpsLon*
- groesseGeschaetzt*
- status : Status*
- melder_id (Fremsdschlüssel)          // darf NULL (anonym)
- berger_id (Fremdschlüssel)          // max 1 Person
- verschollenMelder_id(Fremndschlüssel)// Pflicht wenn VERSCHOLLEN

(berger_id)   0..1
Geisternetz--------------------> Person
                                   1
// Eine Person kann viele Netze bergen (0..*)

(verschollenMelder_id)  0..1
Geisternetz --------------------> Person
                                   1

Regeln
- MUST 1 (anonym melden): melder_id darf NULL, dann auch keine Telefonnummer nötig
- MUST 2 (Bergung eintragen): setze berger_id (darf nur gesetzt werden, wenn noch leer) + status = BERGUNG_BEVORSTEHEND
- MUST 4 (geborgen): status = GEBORGEN
- COULD 7 (verschollen): status = VERSCHOLLEN und verschollenMelder_id muss gesetzt sein (nicht anonym)

Status
- Datenmodellierung nochmal checken
- Primär- und Sekundärschlüssel auch nochmal prüfen
- Orientierung abegschlossen
- Anfoderungen festgelegt --> Planung abgeschlossen
- ATTRUBUTNAMEN VORAB FESTLEGEN. Du hast sonst wieder 10 Namen für ein Attribut!!!

02.02.2026
Heute geht es darum, dem Programm eine visuelle Struktur zu geben. Das wird zum einen mittels eines ausgearbeiteten Datenmodells auf Basis des ersten Tages gemacht und anschließend durch ein STrukturdiagramm. 
Zunächst das Datenmodell: 

Person
id
role
name
phonenumber
                                      Person 1 --- reported_by --- 0..n GhostNet   
Person                                Person 1 --- removed_by --- 0..n GhostNet   
GHOSTNET
id                           
latitude
longitude
size
status
reported_by
removed_by

Obwohl meldende und bergende Personen dieselbe Entität darstellen, werden zwei getrennte Beziehungen modelliert, 
da sie unterschiedliche fachliche Bedeutungen und Geschäftsregeln besitzen. 
Dadurch bleibt das Datenmodell eindeutig, nachvollziehbar und erweiterbar.
Eine genaue Abbildung wurde mit Excel erstellt.

Als nächstes das Strukturdiagramm
Es beinhaltet die Beans, den Service und die Entitäten. Eine genaue Darstellung wurde mithilfe von Excel erstellt.

Status: 
- fachliches Modell ausgearbeitet
- Datenbankstruktur fertig
- UML-Strukturdiagramm erstellt
- Attributnamen festgelegt. HALT DICH DRAN!
- Kurz: Grundlagen abgeschlossen

03.02.2026
Heute starte ich mit dem technischen Grundgerüst. 
Wenn ich den Aufbau richtig verstanden habe, dann habe ich 3 Schichten: 
1. JSF/XHTML
   ein leere Formular
3. Backing Bean mit den Java Klassen
   das enthält die Logik
5. Datenbank mit MySQL
   und hier wird alles gespeichtert

Ich nutze Eclpsie, um das Webprojekt zu starten. Warum Eclipse? Nach der Webentwicklung habe ich schon in VS reingeschnuppert, allerdings fühle ich mich aktuell in Eclipse sicherer. Außerdem habe ich Eclipse bisher nur für Java genutzt. Es wäre interessant noch andere Möglichkeiten von Eclispe auszutesten. 

Status:
- nach 4 Stunden nichts funktioniert, Ich kann die Seite aufrufe bekomme aber einen 404
- gefrustet, Hund auch, morgen von vorne.

04.02.2026
Ziel heute ist es, ein lauffendes technisches Grundgerüst für eine Java-Webanwenung zu Verwaltug der Geisternetze zu bauen. 
Folgendes wird heute passieren: 
1.  Projekt in Eclipse anlegen
   In Eclipse wird ein Dynamic Web Project erstellt, das auf Apache Tomcat läuft.
   Die Weboberfläche wird mit JSF umgesetzt, wobei XHTML-Seiten als Views dienen.
   Konfigurationsdateien werden in XML gepflegt (z.B. web.xml, pom.xml).
   Maven wird zur Verwaltung der benötigten Abhängigkeiten eingesetzt.

Status:
- technisches Grundgerüst steht

Nächste Schritte
3. Ich werde eine klase  Geisternetztabelle anlegen, die erstmal leer sein wird
4. Ich werde 2 Seiten erbauen, einmal eine Liste und einmal ein Formular
5. Ich mache einen hübschen Button

