### **Step-by-Step Guide: Terraforming und Basisbau – Basisbau (4.2)**

In diesem Guide bauen wir ein System, das es dem Spieler ermöglicht, Gebäude auf einem Bauplatz zu platzieren. Es beinhaltet ein einfaches Grid-System, eine Vorschau des Gebäudes und Mechaniken für Upgrades oder Reparaturen. Dieser Guide konzentriert sich auf die Grundmechaniken, die du später erweitern kannst.

---

## **1. Blueprint für Bauplatz**

Zuerst erstellen wir ein Blueprint, das als Bauplatz dient, auf dem der Spieler Gebäude platzieren kann.

#### **1.1 Neues Blueprint für Bauplatz erstellen**

1. Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Actor**.
2. Benenne das Blueprint z. B. `BP_BuildingSlot`.
3. Öffne das Blueprint und füge folgende Komponenten hinzu:
    - **Static Mesh Component**: Dient als visuelle Repräsentation des Bauplatzes (z. B. ein Quadrat oder eine plane Fläche).
        - Wähle ein **Grid-ähnliches Mesh** (falls du keins hast, nutze ein einfaches Plane-Mesh).
        - Skaliere das Mesh auf eine geeignete Größe (z. B. `200 x 200` Unreal-Einheiten).
    - **Box Collision Component**: Diese Komponente wird später für Interaktionen verwendet, z. B. zur Erkennung, ob ein Gebäude auf dem Bauplatz platziert werden kann.

#### **1.2 Variablen hinzufügen**

1. Öffne im **Blueprint Editor** den **My Blueprint**-Bereich und erstelle folgende Variablen:
    - **IsOccupied** (Boolean): Zeigt an, ob der Bauplatz bereits von einem Gebäude belegt ist.
        - Standardwert: `false`.
    - **CurrentBuilding** (Actor Reference): Referenz auf das aktuell platzierte Gebäude.
        - Standardwert: `None`.
2. Markiere `IsOccupied` und `CurrentBuilding` als **Instance Editable**, damit sie später leicht im Editor angepasst werden können.

#### **1.3 Platzierungslogik implementieren**

1. Gehe zum **Event Graph** und füge folgende Logik hinzu:
    - **On Begin Overlap (Box Collision)**:
        - Überprüfe mit einer **Branch**-Node, ob `IsOccupied` auf `false` steht.
        - Wenn `IsOccupied` `false` ist, ermögliche das Platzieren eines Gebäudes.
    - **Platzierung:** Wenn ein Gebäude platziert wird, setze `IsOccupied` auf `true` und speichere das Gebäude in `CurrentBuilding`.

---

## **2. Platzieren von Gebäuden**

Nun erstellen wir ein System, das es dem Spieler ermöglicht, Gebäude auszuwählen und auf dem Bauplatz zu platzieren.

#### **2.1 Neues UI-Widget für Gebäudeauswahl**

1. Gehe zu **Content Browser > Rechtsklick > User Interface > Widget Blueprint**.
2. Benenne das Widget z. B. `BP_BuildingMenu`.
3. Öffne das Widget im **Designer**:
    - Füge **Buttons** hinzu, die verschiedene Gebäude repräsentieren (z. B. einen Button für eine Basis, einen für einen Turm, etc.).
    - Optional: Füge Bilder oder Text hinzu, um die Gebäude zu beschreiben.

#### **2.2 Drag-and-Drop-System einrichten**

1. **Drag-and-Drop aktivieren:**
    
    - Füge im Event Graph des Widgets eine Logik für Drag-and-Drop hinzu:
        - Ziehe von einem Button aus eine **On Clicked**-Node.
        - Verwende eine **Create Widget**-Node, um eine Vorschau des Gebäudes zu erzeugen.
        - Nutze die **Begin Drag Detection**-Node, um das Widget beim Ziehen sichtbar zu machen.
2. **Platzierung vorbereiten:**
    
    - Wenn der Spieler das Gebäude über einem Bauplatz loslässt, überprüfe:
        - Ist der Bauplatz **frei** (`IsOccupied == false`)?
        - Wenn ja, erzeuge das Gebäude (siehe Abschnitt 2.3).

#### **2.3 Gebäude-Vorschau**

1. Erstelle ein neues Blueprint, z. B. `BP_BuildingPreview` (aus `Actor` ableiten).
    
    - Füge ein **Static Mesh Component** hinzu, das das Gebäude repräsentiert.
    - Stelle sicher, dass das Mesh halbtransparent ist:
        - Wähle ein Material mit aktivierter Transparenz oder erzeuge ein neues Material:
            - **Material Type:** Surface.
            - **Blend Mode:** Translucent.
            - **Opacity:** Setze eine konstante niedrige Opazität (z. B. `0.3`).
2. Logik für Vorschau hinzufügen:
    
    - Im `BP_BuildingSlot` kannst du bei einem validen Bauplatz automatisch eine Vorschau anzeigen:
        - Füge eine Funktion hinzu, die das `BP_BuildingPreview`-Actor über dem Bauplatz spawned.
        - Bewege die Vorschau dynamisch mit der Maus.

#### **2.4 Gebäude erstellen**

1. Erstelle ein neues Blueprint für Gebäude, z. B. `BP_BuildingBase`:
    
    - Leite es von `Actor` ab.
    - Füge ein **Static Mesh Component** hinzu, das das tatsächliche Gebäude repräsentiert.
    - Füge Variablen wie **Health** oder **UpgradeLevel** hinzu (siehe Abschnitt 3).
2. Im `BP_BuildingSlot` implementierst du die Platzierungslogik:
    
    - Wenn der Spieler ein Gebäude platzieren möchte:
        - Prüfe, ob `IsOccupied == false`.
        - Spawne das `BP_BuildingBase`-Actor über dem Bauplatz.
        - Setze die Variable `IsOccupied` auf `true`.
        - Speichere das referenzierte Gebäude in `CurrentBuilding`.

---

## **3. Interaktion und Upgrades**

Zum Schluss fügen wir Interaktionen hinzu, z. B. Reparieren oder Upgrades von Gebäuden.

#### **3.1 Interaktion programmieren**

1. Öffne `BP_BuildingBase`.
2. Erstelle eine Funktion `Interact`:
    - Diese Funktion wird aufgerufen, wenn der Spieler mit einem Gebäude interagiert (z. B. durch Drücken der **E-Taste**).
    - Nutze einen **Switch on Int** für unterschiedliche Interaktionen:
        - **Option 0:** Reparieren.
        - **Option 1:** Upgraden.

#### **3.2 Reparatur-Logik**

1. Füge eine Variable `CurrentHealth` (Float) und `MaxHealth` (Float) hinzu.
    - Standardwerte: `MaxHealth = 100`, `CurrentHealth = 50`.
2. Implementiere die Reparaturlogik:
    - Prüfe, ob der Spieler genügend Ressourcen besitzt.
    - Wenn ja, erhöhe den `CurrentHealth`-Wert bis zur maximalen Gesundheit (`MaxHealth`).

#### **3.3 Upgrade-Logik**

1. Füge eine Variable `UpgradeLevel` (Integer) hinzu.
    - Standardwert: `0`.
2. Implementiere die Upgrade-Logik:
    - Erhöhe den `UpgradeLevel`, wenn der Spieler ein Upgrade kauft.
    - Modifiziere das Gebäude basierend auf dem Level:
        - Beispiel: Ändere das Mesh oder erhöhe die `MaxHealth`.

#### **3.4 Interaktion mit Bauplätzen**

1. Öffne `BP_BuildingSlot`.
2. Implementiere eine Funktion `UpgradeOrRepairBuilding`:
    - Überprüfe, ob `IsOccupied == true`.
    - Rufe die `Interact`-Funktion des `CurrentBuilding` auf und übergebe die gewünschte Interaktion.

---

### **Endergebnis**

Nach diesen Schritten hast du ein funktionierendes Basisbausystem:

- Der Spieler kann Bauplätze verwenden, um Gebäude zu platzieren.
- Gebäude haben eine Vorschau, die vor der Platzierung angezeigt wird.
- Interaktionen wie Reparieren und Upgrades sind möglich.

Dieses System kann leicht erweitert werden, um weitere Funktionen wie Ressourcenmanagement, automatisierte Gebäudeproduktion oder spezifische Gebäudetypen hinzuzufügen. Wenn du mehr Details oder spezifische Anweisungen zu einem der Schritte benötigst, lass es mich wissen! 😊