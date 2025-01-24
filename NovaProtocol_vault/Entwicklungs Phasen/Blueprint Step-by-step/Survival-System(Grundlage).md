### **Step-by-Step Guide: Survival-System (Grundlage)**

In diesem Guide erstellen wir ein grundlegendes Survival-System in Unreal Engine 5. Der Spieler hat Variablen wie `Health`, `Oxygen` und `Energy`, die mit der Zeit abnehmen oder durch bestimmte Aktionen wiederhergestellt werden können. Ein HUD wird hinzugefügt, um den aktuellen Status der Ressourcen anzuzeigen.

---

### **1. Blueprint für Spieler erstellen**

#### **1.1 Spieler-Blueprint erstellen**

1. Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Character**.
2. Benenne den Blueprint z. B. `BP_PlayerCharacter`.
3. Öffne den Blueprint und überprüfe, ob er eine `Capsule Component`, ein `Skeletal Mesh`, eine `Camera` und eine `Character Movement Component` enthält (das sind Standardkomponenten).

#### **1.2 Variablen hinzufügen**

1. Öffne den **Blueprint Editor** für `BP_PlayerCharacter`.
    
2. Gehe in den **My Blueprint**-Bereich (links unten) und erstelle drei neue Variablen:
    
    - `Health` (Float) – Standardwert: `100` (z. B. maximale Gesundheit).
    - `Oxygen` (Float) – Standardwert: `100` (z. B. maximale Sauerstoffmenge).
    - `Energy` (Float) – Standardwert: `100` (z. B. maximale Energie).
3. Stelle für jede Variable sicher:
    
    - Aktiviere **Instance Editable** und **Expose on Spawn** (damit sie später angepasst werden können).
    - Gehe zu **Details Panel > Category** und gruppiere sie unter "Survival".

---

### **2. HUD für Ressourcenanzeige**

#### **2.1 Neues Widget Blueprint erstellen**

1. Gehe zu **Content Browser > Rechtsklick > User Interface > Widget Blueprint**.
2. Benenne das Widget z. B. `BP_HUD`.
3. Öffne den Widget Blueprint im **UMG Designer**.

#### **2.2 Fortschrittsleisten hinzufügen**

1. Ziehe aus der Toolbox **Progress Bar** (Fortschrittsleiste) in den **Designer**.
    
2. Füge drei Fortschrittsleisten hinzu und positioniere sie:
    
    - Eine für `Health`, eine für `Oxygen` und eine für `Energy`.
    - Optional: Füge Labels oder Icons daneben hinzu (z. B. für bessere Lesbarkeit).
3. Passe die Fortschrittsleisten im **Details Panel** an:
    
    - Ändere die Farben, um die Ressourcen visuell zu unterscheiden (z. B. Grün für `Health`, Blau für `Oxygen`, Gelb für `Energy`).
    - Setze die Werte im Bereich `Percent` auf `1.0` (entspricht 100 %).

#### **2.3 Variablen im HUD binden**

1. Klicke auf die Fortschrittsleiste für `Health`.
    
2. Gehe zum **Details Panel > Bind > Percent** und erstelle eine Binding-Funktion:
    
    - Diese Funktion ruft den aktuellen `Health`-Wert des Spielers ab.
    - Beispiel: Im Binding-Fenster nutze `Get Player Character > Cast to BP_PlayerCharacter > Get Health`.
3. Wiederhole diesen Schritt für `Oxygen` und `Energy`, indem du die jeweiligen Variablen bindest.
    

---

### **3. Tick-Events für Ressourcenverbrauch**

#### **3.1 Sauerstoffverbrauch programmieren**

1. Öffne den Event Graph von `BP_PlayerCharacter`.
2. Suche nach der **Event Tick** Node (wird standardmäßig hinzugefügt).
3. Ziehe die `Oxygen`-Variable in den Graph und erstelle eine **Set Oxygen**-Node.
4. Verbinde die Logik wie folgt:
    - Ziehe eine `Delta Seconds` Node (aus Event Tick) und multipliziere sie mit einem konstanten Wert (z. B. `1.0`).
    - Subtrahiere den resultierenden Wert von der aktuellen `Oxygen`-Variable.
    - Speichere das Ergebnis zurück in `Oxygen`.
5. Füge eine **Branch**-Node hinzu, um zu prüfen, ob `Oxygen <= 0`:
    - Wenn ja, reduziere den `Health`-Wert:
        - Ziehe die `Health`-Variable in den Graph und verwende eine ähnliche Logik wie oben (subtrahiere einen Wert und speichere ihn zurück).
    - Optional: Füge eine **Game Over**-Logik hinzu, falls `Health` ebenfalls 0 erreicht.

#### **3.2 Energieverbrauch (optional)**

- Wiederhole dieselbe Logik für die `Energy`-Variable, falls du möchtest, dass z. B. Aktionen wie Sprinten oder Springen Energie verbrauchen.

---

### **4. Test-Ressourcen hinzufügen**

#### **4.1 Blueprint für Sauerstoffkapseln erstellen**

1. Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
2. Benenne den Blueprint z. B. `BP_OxygenPickup`.
3. Öffne den Blueprint und füge eine `Static Mesh Component` hinzu:
    - Wähle ein Mesh aus (z. B. eine Kapsel oder eine Kugel) und passe die Größe an.
4. Füge eine **Box Collision** hinzu:
    - Skaliere sie so, dass sie den Bereich um das Mesh abdeckt.
    - Stelle sicher, dass die Kollision so eingestellt ist, dass sie Events auslöst.

#### **4.2 Pickup-Logik implementieren**

1. Öffne den Event Graph von `BP_OxygenPickup`.
2. Füge folgende Logik hinzu:
    - Ziehe eine **On Component Begin Overlap** Node von der `Box Collision`.
    - Nutze **Cast to BP_PlayerCharacter**, um sicherzustellen, dass der Spieler das Objekt berührt.
    - Ziehe die `Oxygen`-Variable des Charakters und addiere einen festen Wert (z. B. `20`) hinzu.
    - Verwende eine **Destroy Actor**-Node, um das Objekt nach dem Aufnehmen zu entfernen.
    - Beispielgrafik:

``OnComponentBeginOverlap -> Cast to BP_PlayerCharacter -> Get Oxygen -> Add -> Set Oxygen -> Destroy Actor``

#### **4.3 Sauerstoffkapseln in der Welt platzieren**

1. Ziehe `BP_OxygenPickup` in die Szene.
2. Platziere mehrere Kapseln an verschiedenen Stellen in der Welt.
3. Teste das Spiel:
    - Beobachte, ob der Sauerstoffverbrauch funktioniert und ob Sauerstoffkapseln korrekt aufgesammelt werden.

---

### **Zusätzliche Verbesserungen**

1. **Animation für Ressourcenaufnahme:**
    
    - Füge eine kleine Partikelanimation (z. B. eine Explosion) hinzu, wenn der Spieler eine Kapsel aufsammelt.
2. **Soundeffekte:**
    
    - Spiele einen Ton ab, wenn Sauerstoff verbraucht wird oder eine Kapsel eingesammelt wird.
3. **Warnanzeige:**
    
    - Zeige eine Warnung im HUD, wenn `Oxygen` oder `Health` unter 20 % fällt (z. B. durch Blinken oder eine Textnachricht).

---

### **Endergebnis**

Nach diesen Schritten hast du ein funktionierendes Survival-System mit den Variablen `Health`, `Oxygen` und `Energy`. Der Spieler verliert Ressourcen über die Zeit, kann jedoch durch das Einsammeln von Ressourcen überleben. Das HUD zeigt den aktuellen Status der Ressourcen an, und die Logik kann flexibel erweitert werden (z. B. für Energiesysteme oder Schadensquellen).