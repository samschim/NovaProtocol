### **Step-by-Step Guide: Offene Welten mit World Partition**

In diesem Guide erstellen wir eine offene Welt in Unreal Engine 5 mit der **World Partition**-Technologie, prozedural generierten Planetenlayouts und optimierter Performance durch Level Streaming. Die offenen Welten dienen als Grundlage für die Planeten in deinem Spiel.

---

### **1. World Partition aktivieren**

#### **1.1 Projekt für offene Welten vorbereiten**

1. Öffne dein Unreal Engine 5 Projekt.
2. Gehe zu **Edit > Project Settings**.
3. Suche in der linken Seitenleiste nach **World Settings** (unter "Engine").
4. Aktiviere die Checkbox **Enable World Partition**.

#### **1.2 Neue Karte erstellen**

1. Gehe zu **File > New Level** und wähle die Vorlage **Open World**.
2. Speichere die neue Karte im Content Browser, z. B. unter `Maps/OpenWorldLevel`.

#### **1.3 Grid-basierte Sektoren überprüfen**

1. Öffne die World Settings (im **Details Panel** bei geöffneter Karte):
    - Klicke auf das Zahnradsymbol oben rechts und aktiviere **World Partition > World Composition**.
2. Du wirst jetzt ein **Grid System** für deine offene Welt sehen, das die Karte in Sektoren unterteilt.
3. Speichere die Karte.

---

### **2. Prozedurale Planeten-Generierung**

#### **2.1 Procedural Content Generation Framework aktivieren**

1. Gehe zu **Edit > Plugins**.
2. Suche nach **Procedural Content Generation** und aktiviere das Plugin.
3. Starte die Engine neu, damit das Plugin aktiviert wird.

#### **2.2 Procedural Volume erstellen**

1. Gehe in den **Place Actors Panel** (linke Seite, Standardlayout).
2. Suche nach **Procedural Volume** und ziehe es in die Szene.
3. Skaliere das Volume so, dass es deinen gewünschten Planetenbereich abdeckt (z. B. durch Ziehen an den Ecken oder durch Eingabe der Maße im **Details Panel**).

#### **2.3 Biome und Landschaft generieren**

1. **Landschafts-Blueprint erstellen:**
    
    - Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
    - Benenne die Blueprint z. B. `BP_ProceduralLandscape`.
    - Öffne die Blueprint und füge folgende Komponenten hinzu:
        - **Landscape**: Die Landschaftskomponente, die für den Planeten sichtbar ist.
        - **Procedural Components**: Nutze Nodes wie `Spawn Actors at Runtime`, um Biome und Elemente zu generieren.
2. **Biome generieren:**
    
    - Im `BP_ProceduralLandscape` kannst du mit folgenden Logiken arbeiten:
        - Füge Bereiche hinzu, die unterschiedliche Texturen und Assets haben (z. B. Wüste, Eis, Wälder).
        - Nutze Material-Blend-Techniken, um Übergänge zwischen Biomen glatt erscheinen zu lassen.
3. **Objekte dynamisch platzieren:**
    
    - Füge in deinem Blueprint Nodes wie `SpawnActor` oder `Add Hierarchical Instanced Static Mesh` hinzu, um Felsen, Bäume und andere Objekte zu generieren.
    - Nutze Schleifen und Zufallswerte (`Random Float in Range`), um die Platzierung zufällig zu gestalten.
    - Beispiel: Felsen können in einem bestimmten Bereich mit einer zufälligen Größe und Drehung gespawnt werden.
4. **Höhen und Berge generieren:**
    
    - Gehe zu deiner Landschaft und aktiviere **Sculpt Tools**.
    - Verwende Werkzeuge wie **Noise** oder **Erosion**, um realistische Berge und Täler zu erzeugen.

#### **2.4 Anpassungen vornehmen**

1. Teste die prozedurale Generierung, indem du das Spiel startest.
2. Optimiere die Platzierung, Größe und Variationen der Biome oder Objekte.
3. Speichere das Blueprint.

---

### **3. Performance optimieren**

#### **3.1 Level Streaming mit World Partition**

1. **HLOD (Hierarchical Level of Detail) aktivieren:**
    
    - Gehe zu **World Settings > World Partition**.
    - Aktiviere die Option **Enable HLOD** (Hierarchical Level of Detail).
    - Dies reduziert die Renderkosten, indem entfernte Objekte durch vereinfachte Versionen dargestellt werden.
2. **Sektoren für Streaming aktivieren:**
    
    - Gehe zu **World Partition Panel** (über das Menü **Window > World Partition** öffnen).
    - Hier kannst du Sektoren aktivieren/deaktivieren, um nur bestimmte Teile der Karte zu laden.
    - Unreal Engine lädt automatisch nur die Sektoren, die sich im Sichtbereich des Spielers befinden.
3. **Landscape Streaming verwenden:**
    
    - Öffne das `BP_ProceduralLandscape`.
    - Unterteile die Landschaft in kleinere Abschnitte, die individuell gestreamt werden können.
    - Füge eine **Level Streaming Volume** hinzu, um Sektoren basierend auf der Position des Spielers zu laden.

#### **3.2 Optimierung von Assets**

1. Nutze **Instanced Static Meshes**, um Objekte (z. B. Bäume, Steine) effizienter darzustellen.
    
    - Erstelle ein **Hierarchical Instanced Static Mesh Component** für wiederholte Assets.
    - Reduziere die Anzahl individueller Draw Calls, was die Performance steigert.
2. Reduziere die Polyanzahl von Modellen und aktiviere LOD-Stufen (Level of Detail):
    
    - Stelle sicher, dass entfernte Objekte niedrigere Detailstufen haben.

#### **3.3 Dynamisches Laden und Entladen testen**

1. Laufe durch deine Welt und beobachte, ob Sektoren korrekt laden und entladen.
2. Nutze den **Stat Streaming** Befehl in der Konsole, um zu sehen, wie viele Assets gestreamt werden.
3. Passe die Größe der Sektoren und die Streaming-Entfernung an, falls es zu Einbrüchen kommt.

---

### **Zusammenfassung**

Nach diesen Schritten hast du eine offene Welt, die:

- Mit World Partition optimiert ist und nur sichtbare Sektoren lädt.
- Biome und Objekte dynamisch mit dem Procedural Content Generation Framework generiert.
- Eine gute Performance durch Level Streaming und optimierte Assets sicherstellt.

Dies ist die Grundlage, um deine Planeten- und Universumsmechanik weiter auszubauen! 