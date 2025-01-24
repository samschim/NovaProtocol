### **Step-by-Step Guide: Terraforming**

In diesem Guide zeige ich dir, wie du grundlegendes Terraforming in Unreal Engine 5 implementierst und erste Mechaniken für dynamische Landschaftsanpassungen einbaust. Du lernst, wie du mit dem **Landscape Tool** eine Landschaft erstellst, deren Form dynamisch durch Blueprint-Logik angepasst werden kann. Außerdem fügen wir atmosphärische Effekte hinzu, die mit der Terraforming-Mechanik verknüpft werden.

---

## **4.1 Terraforming**

---

### **Schritt 1: Landschaft bearbeiten (Erstellen einer Basislandschaft)**

1. **Neue Landschaft erstellen**
    
    - Öffne deine Karte oder erstelle eine neue Karte (z. B. über **File > New Level > Empty Level**).
    - Gehe in das **Modes Panel** (oben links im Standard-Layout) und wähle den Reiter **Landscape** aus.
    - Stelle die folgenden Parameter ein:
        - **Section Size:** 63x63 Quads (Standardgröße für Planeten oder größere Flächen).
        - **Number of Components:** Passe dies basierend auf der Größe deines Planeten an (z. B. 8x8).
        - **Scale:** Setze die Z-Skalierung auf `100.0`, um Höhenunterschiede deutlicher sichtbar zu machen.
    - Klicke auf **Create**, um die Landschaft zu erstellen.
2. **Terraforming manuell testen**
    
    - Verwende die Werkzeuge im **Sculpt Tool** (z. B. "Sculpt", "Smooth", "Noise"), um manuell Berge, Täler oder andere Landschaftsmerkmale zu erstellen.
    - Experimentiere mit verschiedenen Brushes, um interessante Formen zu erzeugen.

---

### **Schritt 2: Terraforming dynamisch umsetzen (Blueprint-basiert)**

#### **2.1 Blueprint für Terraforming erstellen**

1. Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
2. Benenne den Blueprint z. B. `BP_TerraformingTool`.
3. Öffne den Blueprint und füge folgende Komponenten hinzu:
    - **Scene Component:** Wird als Root-Komponente verwendet.
    - **Static Mesh Component:** Optional, z. B. eine sichtbare Darstellung des Werkzeugs (z. B. eine Kapsel oder Kugel).
    - **Sphere Collision Component:** Definiert den Bereich, der terraformiert wird.

#### **2.2 Landschafts-Referenz festlegen**

1. Öffne den Event Graph von `BP_TerraformingTool`.
2. Ziehe eine **Get All Actors of Class** Node in den Graph und wähle **Landscape** als Class.
    - **Hinweis:** Dies erfasst die Landschaft, auf der wir später Änderungen vornehmen.
    - Speichere die referenzierte Landschaft in einer **Variable** (z. B. `Target Landscape`).

---

#### **2.3 Dynamische Höhenanpassung implementieren**

1. Ziehe eine Node für die **Sphere Collision Component** in den Graph.
    
2. Füge die Node **On Component Begin Overlap** hinzu.
    
3. Implementiere die Logik:
    
    - Prüfe, ob die überlappenden Objekte zur Landschaft gehören.
    - Verwende die Node **Edit Landscape Component** (oder ähnliche Custom-Nodes), um die Höhe des Landschaftssegments zu ändern.
    - Beispiel:
        - Hole dir die Position der Kollision (`Get World Location`) und füge einen Wert hinzu, um die Landschaft anzuheben.
        - Nutze ein **Timeline**-Node, um die Änderungen flüssig und animiert wirken zu lassen.
4. **Ergebnisse simulieren:**
    
    - Spiele mit Multiplikatoren oder Konstanten, um zu bestimmen, wie stark die Landschaft angehoben/abgesenkt wird.

---

### **Schritt 3: Atmosphäre anpassen**

#### **3.1 Sky Atmosphere hinzufügen**

1. Gehe zu **Place Actors Panel** (linke Seite im Editor).
2. Suche nach **Sky Atmosphere** und ziehe es in die Szene.
3. Füge zusätzlich die folgenden Effekte hinzu:
    - **Exponential Height Fog**: Für Nebeleffekte und mehr Tiefe in der Atmosphäre.
    - **Directional Light**: Stelle sicher, dass diese das Licht der Sonne simuliert (aktiviere **Atmosphere Sun Light** im **Details Panel**).

#### **3.2 Atmosphäre dynamisch verändern**

1. Öffne erneut den `BP_TerraformingTool`.
    
2. Füge eine Referenz zum **Sky Atmosphere** in den Blueprint ein:
    
    - Nutze **Get All Actors of Class** und wähle `Sky Atmosphere` als Class.
    - Speichere diese in einer Variable (z. B. `Sky Atmosphere Reference`).
3. Implementiere die Logik zur Anpassung:
    
    - Verwende Nodes wie `Set Atmosphere Thickness` oder `Set Light Color`, um die Atmosphäre dynamisch zu verändern.
    - Beispiel:
        - Wenn der Spieler ein Terraforming-Tool benutzt, verringere die Dichte des Nebels oder ändere die Helligkeit des Himmels.

---

### **Schritt 4: Terraforming testen**

1. Ziehe den `BP_TerraformingTool` in die Szene.
    
2. Positioniere es über der Landschaft und spiele das Spiel.
    
3. Bewege das Werkzeug und beobachte:
    
    - Hebt/senkt sich die Landschaft korrekt?
    - Passen sich die atmosphärischen Effekte sichtbar an?
4. Optimiere die Werte (z. B. Geschwindigkeit der Veränderungen, Skalierung der Effekte).
    

---

### **Erweiterungen und Tipps**

- **Partikeleffekte hinzufügen:** Erstelle ein Partikelsystem (z. B. Staub oder Rauch), das beim Terraforming abgespielt wird.
- **Soundeffekte:** Spiele Terraforming-Sounds ab, wenn die Landschaft verändert wird.
- **Player Interaction:** Implementiere, dass der Spieler das Terraforming-Tool aufnimmt und durch Eingaben (z. B. Maustaste) aktiviert.
- **Terraforming-Bereich:** Erlaube dem Spieler, den Bereich des Terraformings dynamisch zu vergrößern/verkleinern.
- **Biome beeinflussen:** Terraforming kann Biome verändern, z. B. durch das Platzieren von Vegetation oder das Schmelzen von Eis.

---

### **Zusammenfassung**

Du hast ein grundlegendes Terraforming-System erstellt, mit dem du die Landschaft dynamisch anpassen kannst. Das Hinzufügen von atmosphärischen Effekten macht das Terraforming visuell eindrucksvoller. Dieses System kann später um weitere Funktionen erweitert werden, wie z. B. prozedurale Biome oder ressourcenabhängiges Terraforming. 😊