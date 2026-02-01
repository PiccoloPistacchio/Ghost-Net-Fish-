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
[Person]
- id (Primärschlüssel)
- name
- telefonnummer*/ telefonnummer   (bei anonymen Meldern leer)

 (melder_id)  0..1
[Geisternetz] --------------------> [Person]
- id (Primärschlüssel)                           1
- gpsLat*
- gpsLon*
- groesseGeschaetzt*
- status : Status*
- melder_id (Fremsdschlüssel)          // darf NULL (anonym)
- berger_id (Fremdschlüssel)          // max 1 Person
- verschollenMelder_id(Fremndschlüssel)// Pflicht wenn VERSCHOLLEN

(berger_id)   0..1
[Geisternetz] --------------------> [Person]
                                   1
// Eine Person kann viele Netze bergen (0..*)

(verschollenMelder_id)  0..1
[Geisternetz] --------------------> [Person]
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
