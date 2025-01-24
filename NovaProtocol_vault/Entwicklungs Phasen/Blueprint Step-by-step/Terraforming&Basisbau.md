### **Step-by-Step Guide: Terraforming und Basisbau**

Terraforming und Basisbau sind komplexe, aber spannende Gameplay-Mechaniken. Hier erstellen wir eine einfache Grundlage, die später erweitert werden kann. Terraforming erlaubt es, Landschaften zu verändern, während der Basisbau Spielern die Möglichkeit gibt, Gebäude zu platzieren und zu interagieren.

---

## **4.1 Terraforming**

Terraforming ermöglicht Spielern, Landschaften dynamisch zu verändern, z. B. Berge zu formen, Täler zu schaffen oder den Boden abzusenken.

---

### **1. Landschaft bearbeiten**

#### **1.1 Landscape Tool nutzen**

1. **Landscape erstellen:**
    
    - Öffne deine Karte.
    - Gehe zu **Modes > Landscape** (oben links, im Standardlayout).
    - Wähle **Create New** und konfiguriere die Größe:
        - **Section Size**: z. B. 63x63 (für große Areale).
        - **Overall Resolution**: Wähle eine Auflösung, die der Größe deines Planeten entspricht.
    - Klicke auf **Create**, um die Landschaft zu generieren.
2. **Manuelle Bearbeitung der Landschaft (für Testzwecke):**
    
    - Wähle unter **Sculpt** Werkzeuge wie:
        - **Sculpt**: Hebt den Boden an.
        - **Smooth**: Glättet unebene Flächen.
        - **Noise**: Fügt zufällige Unebenheiten hinzu.
    - Teste das Werkzeug, um die Landschaft händisch zu bearbeiten.

---

#### **1.2 Dynamische Veränderung per Blueprint**

Wir erstellen ein Blueprint-basiertes Terraforming-System, mit dem Spieler die Landschaft anheben oder absenken können.

1. **Blueprint-Setup:**
    
    - Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
    - Benenne das Blueprint z. B. `BP_TerraformTool`.
2. **Static Mesh hinzufügen (optional):**
    
    - Füge ein `Static Mesh Component` hinzu, z. B. für eine Terraforming-Maschine oder ein sichtbares Werkzeug.
3. **Trace zur Landschaft (zum Zielpunkt):**
    
    - Ziehe im Event Graph eine **Line Trace By Channel**-Node ein.
    - Richte den Trace so ein, dass er vom Spieler aus nach vorne geht:
        - Nutze `Get Player Camera Location` und `Get Player Camera Rotation`, um die Start- und Endpunkte des Traces zu berechnen.
    - Nutze die Ausgabe **Hit Result**, um den Punkt zu erhalten, wo der Trace auf die Landschaft trifft.
4. **Landschaftsveränderung implementieren:**
    
    - Ziehe von der `Hit Result`-Ausgabe eine `Landscape`-Komponente ab.
    - Füge eine Node **Edit Landscape Component** hinzu.
        - Hier kannst du die Höhe des Terrains dynamisch ändern.
        - Beispiel: Verwende `Add` oder `Subtract`, um die Höhe anzuheben oder abzusenken.
5. **Interaktivität hinzufügen:**
    
    - Binde die Terraforming-Logik an Eingaben, z. B. `Left Mouse Button` zum Anheben und `Right Mouse Button` zum Absenken.
    - Nutze Variablen wie `Brush Size`, um den Radius des Terraforming-Effekts zu steuern.

---

### **2. Atmosphäre anpassen**

Die Atmosphäre eines Planeten kann stark zur Immersion beitragen. Wir erstellen ein dynamisches System zur Anpassung der Himmelsfarbe, Nebeldichte und Lichtverhältnisse.

#### **2.1 Sky Atmosphere und Fog hinzufügen**

1. **Sky Atmosphere einfügen:**
    - Gehe zu **Place Actors Panel** und suche nach **Sky Atmosphere**. Ziehe es in die Szene.
    - Passe Parameter wie `Rayleigh Scattering` an, um die Farbe der Atmosphäre zu verändern (z. B. für einen blauen Himmel).
2. **Exponential Height Fog hinzufügen:**
    - Suche nach **Exponential Height Fog** und füge es hinzu.
    - Setze den **Fog Density**-Wert, um die Dichte des Nebels anzupassen.

#### **2.2 Dynamische Anpassung per Blueprint**

1. Öffne das `BP_TerraformTool` oder erstelle ein neues Blueprint.
2. Füge Funktionen hinzu, um die Atmosphäre zu verändern:
    - Verwende **Set Sky Atmosphere Properties** und **Set Exponential Height Fog Properties**.
    - Binde diese Funktionen an Eingaben (z. B. durch Drücken von Tasten wie `1` für rote Atmosphäre, `2` für grüne Atmosphäre).

---

## **4.2 Basisbau**

Der Basisbau erlaubt es Spielern, Gebäude zu platzieren und mit ihnen zu interagieren. Wir beginnen mit einem einfachen Grid-System und einem Platzierungs-Blueprint.

---

### **1. Blueprint für Bauplatz**

#### **1.1 Building Slot erstellen**

1. Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
2. Benenne das Blueprint z. B. `BP_BuildingSlot`.
3. Öffne den Blueprint und füge folgende Komponenten hinzu:
    - **Box Collision**: Definiert den Bereich, in dem ein Gebäude platziert werden kann.
    - **Static Mesh (optional)**: Kann eine visuelle Vorschau oder Markierung darstellen.

#### **1.2 Platzierungslogik**

1. **Grid-System:**
    - Lege ein virtuelles Grid fest, indem du die Weltkoordinaten abrundest.
    - Beispiel: Nutze eine `Floor` Node, um Koordinaten auf ein Raster (z. B. 100x100) zu begrenzen.
2. **Blueprint-Logik:**
    - Im Event Graph überprüfst du, ob der Spieler ein Gebäude platzieren möchte (z. B. durch Drücken einer Taste wie `Left Mouse Button`).
    - Nutze eine Node wie `SpawnActor`, um ein Gebäude-Blueprint zu erzeugen.

---

### **2. Platzieren von Gebäuden**

#### **2.1 Widget-UI für Platzierung**

1. Gehe zu **Content Browser > Rechtsklick > User Interface > Widget Blueprint**.
2. Benenne das Widget z. B. `BP_BuildingMenu`.
3. Füge Buttons für verschiedene Gebäudetypen hinzu (z. B. "Wohngebäude", "Forschungsstation").
4. Verknüpfe die Buttons mit Events, um das ausgewählte Gebäude zu speichern.

#### **2.2 Vorschau anzeigen**

1. Öffne das `BP_BuildingSlot` und füge eine `Static Mesh Component` hinzu.
2. Nutze eine transparente Materialinstanz, um eine Vorschau des Gebäudes zu zeigen.
3. Bewege das Gebäude mithilfe von **Set World Location**, während der Spieler die Maus bewegt.

---

### **3. Interaktion und Upgrades**

#### **3.1 Interaktionslogik**

1. Öffne das Blueprint eines platzierten Gebäudes (z. B. `BP_BaseBuilding`).
2. Füge eine Funktion hinzu, die Ressourcen benötigt, um das Gebäude zu reparieren oder zu verbessern:
    - Beispiel: `If Player Has Enough Resources → Upgrade`.

#### **3.2 Upgrade-Funktion**

1. Erstelle eine `Level`-Variable im Gebäude-Blueprint (z. B. 1, 2, 3).
2. Erhöhe die Variable, wenn ein Upgrade durchgeführt wird, und ändere das Aussehen oder die Funktionen des Gebäudes entsprechend.

---

### **Zusammenfassung**

Nach diesen Schritten hast du:

- Ein Terraforming-System, mit dem Spieler die Landschaft dynamisch verändern können.
- Eine erste Version eines Basisbau-Systems, bei dem Spieler Gebäude auf einem Grid platzieren und interagieren können.

Später kannst du das System erweitern, z. B. durch prozedurale Texturen für Terraforming oder eine komplexere Interaktionslogik. Wenn du bei einem der Schritte Unterstützung brauchst, lass es mich wissen! 😊