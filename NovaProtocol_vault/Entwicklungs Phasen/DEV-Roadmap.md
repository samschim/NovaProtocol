
---
## [**Roadmap NovaProtocol**](NovaProtocol-Spielbeschreibung.md)

---

## **Schritt 1: Planung und Organisation**

Bevor wir in die Entwicklung einsteigen, müssen wir einen soliden Plan erstellen, damit du die Entwicklung strukturierst und nicht den Überblick verlierst.

### **1.1 [Entwicklungsplan](Entwicklungsplan.md) erstellen**

Teile dein Projekt in kleinere Module auf. Beispiel:

- **[Phase 1](Phase1)**
	- **Phase 1.1: Kernfunktionen erstellen**
	    - Charakterbewegung (Raumschiffsteuerung und Spielersteuerung).
	    - Grundlegendes Survival-System (Sauerstoff, Ressourcen, Energie).
	    - Prozedurale Generierung eines Planeten oder Universums.
	- **Phase 2: Gameplay erweitern**
	    - Terraforming-System und Basisbau.
	    - Crafting und Ressourcenabbau.
	    - Einfache Gegner und Fraktionen.
	- **Phase 3: Multiplayer und Polishing**
	    - Kooperativer Multiplayer.
	    - Fortgeschrittene Weltraumkämpfe und dynamische Events.
	    - Optimierung der Performance und UI/UX.
- **[Phase2](Phase2)**
- **[Phase3](Phase3)**

---

## **Schritt 2: Unreal Engine 5 einrichten**

### **2.1 Unreal Engine 5 installieren**

1. Lade die Unreal Engine 5 aus dem **Epic Games Launcher** herunter.
2. Wähle ein leeres Projekt mit **Blueprint + C++** (so kannst du später auf C++ erweitern, wenn nötig).
3. Stelle sicher, dass folgende Plugins aktiviert sind:
    - **Chaos Physics** (für Zerstörung und Physik).
    - **World Partition** (für offene Welten).
    - **Procedural Content Generation Framework** (prozedurale Generierung).
    - **Multiplayer-Features** (z. B. Online Subsystem).

---

## **Schritt 3: Erste Kernfunktionen erstellen**

Wir konzentrieren uns darauf, die grundlegenden Systeme und Mechaniken für dein Spiel zu erstellen.

---

### **3.1 Raumschiffsteuerung (Weltraumbewegung)**

#### Schritte:

1. **Blueprint erstellen:**
    
    - Gehe zu **Content Browser > Rechtsklick > Blueprint Class > Pawn**.
    - Nenne die Blueprint z. B. `BP_Spaceship`.
    - Öffne sie und füge eine `Static Mesh Component` hinzu (z. B. als Raumschiffkörper).
    - Setze eine **Camera Component** für die Ansicht hinzu.
2. **Bewegung implementieren:**
    
    - Öffne die Event Graph.
    - Implementiere Eingaben: Gehe zu **Edit > Project Settings > Input > Action Mappings**:
        - Erstelle Eingaben wie `Thrust`, `Turn`, `Pitch`, `Roll`.
    - Nutze Nodes wie `Add Movement Input` oder `Add Torque` für Steuerung.
3. **Raumschiffphysik aktivieren:**
    
    - Aktiviere bei der `Static Mesh Component` die Physik-Optionen (`Simulate Physics` aktivieren).
    - Passe Masse und Dämpfung im **Details Panel** an, damit die Bewegungen realistisch wirken.
4. **Gameplay testen:**
    
    - Ziehe `BP_Spaceship` in die Welt.
    - Stelle sicher, dass das Raumschiff mit deinen Eingaben reagiert.

---

### **3.2 Offene Welten mit World Partition**

#### Schritte:

1. **World Partition aktivieren:**
    
    - Gehe zu **Edit > Project Settings > World Settings > Enable World Partition**.
    - Erstelle eine offene Weltkarte, die in Sektoren aufgeteilt ist.
2. **Prozedurale Planeten-Generierung:**
    
    - Nutze das **Procedural Content Generation Framework**, um zufällige Planetenlayouts zu erstellen:
        - Erstelle ein **Procedural Volume** (in der Szene platzieren).
        - Nutze **Nodes**, um Biome (z. B. Wüste, Eis) und Ressourcen (z. B. Felsen, Bäume) dynamisch zu generieren.
3. **Performance optimieren:**
    
    - Nutze **Level Streaming**, um nur sichtbare Teile des Planeten zu laden.

---

### **3.3 Survival-System (Grundlage)**

#### Schritte:

1. **Blueprint für Spieler erstellen:**
    
    - Erstelle einen `BP_PlayerCharacter` (aus `Character`-Klasse ableiten).
    - Füge eine **Health**, **Oxygen** und **Energy** Variable hinzu (z. B. `Float`-Typ).
2. **HUD für Ressourcenanzeige:**
    
    - Erstelle ein neues **Widget Blueprint** (z. B. `BP_HUD`).
    - Füge Fortschrittsleisten für `Oxygen`, `Health` und `Energy` hinzu.
    - Binde sie an die Variablen deines Charakters (z. B. über `Bind`).
3. **Tick-Events für Ressourcenverbrauch:**
    
    - Im `BP_PlayerCharacter` nutze die `Event Tick`, um `Oxygen` schrittweise zu reduzieren.
    - Füge Logik hinzu: Wenn `Oxygen` 0 erreicht, reduziere `Health`.
4. **Test-Ressourcen hinzufügen:**
    
    - Platziere Ressourcen in der Szene (z. B. Sauerstoffkapseln) und implementiere eine Logik, dass der Spieler diese aufnehmen kann.

---

## **Schritt 4: Terraforming und Basisbau**

### **4.1 Terraforming**

Terraforming ist ein Kernstück deines Spiels. Für den Anfang kannst du einfache Mechaniken umsetzen, die später ausgebaut werden.

#### Schritte:

1. **Landschaft bearbeiten:**
    
    - Nutze das **Landscape Tool** in Unreal Engine, um Planeten zu erstellen.
    - Mit **Blueprint Scripting** kannst du dynamisch die Form der Landschaft verändern:
        - Beispiel: Verwende `Set World Position` und `Deform Mesh` Nodes, um das Gelände per Knopfdruck anzuheben/abzusenken.
2. **Atmosphäre anpassen:**
    
    - Aktiviere **Sky Atmosphere** und **Exponential Height Fog**, um eine realistische Atmosphäre zu erstellen.
    - Füge eine Funktion hinzu, um die Farbe und Dichte der Atmosphäre zu verändern.

---

### **4.2 Basisbau**

#### Schritte:

1. **Blueprint für Bauplatz:**
    
    - Erstelle ein Blueprint für eine Platzierungsfunktion (`BP_BuildingSlot`).
    - Spieler können hier Gebäude platzieren, indem sie ein einfaches **Grid-System** nutzen.
2. **Platzieren von Gebäuden:**
    
    - Implementiere ein Drag-and-Drop-System für Gebäude in einem **UI-Widget**.
    - Nutze eine Vorschau (transparente Darstellung des Gebäudes), bevor der Spieler es setzt.
3. **Interaktion und Upgrades:**
    
    - Gebäude können aufgerüstet oder repariert werden, wenn Ressourcen verfügbar sind.

---

## **Schritt 5: Multiplayer-Modus**

Sobald die Kernmechaniken stehen, kannst du den Multiplayer-Modus hinzufügen.

#### Schritte:

1. **Online Subsystem aktivieren:**
    
    - In **Project Settings > Plugins > Online Subsystem** (z. B. Steam) aktivieren.
2. **Netzwerkfähigkeit für Blueprints:**
    
    - Füge Multiplayer-Logik hinzu (z. B. `Replicate` Variablen, `RPC` für Server/Client-Kommunikation).
3. **Testen:**
    
    - Teste den Multiplayer-Modus mit dedizierten Servern. Unreal Engine bietet dafür Tools wie den **Session Browser**.

---

## **Zusammenfassung**

Dein Spiel wird durch die Kombination verschiedener Systeme komplex, daher solltest du modular arbeiten. Entwickle die Kernmechaniken zuerst und baue dann darauf auf. Jeder neue Schritt sollte gut getestet und optimiert werden. Ich helfe dir gerne bei jedem Detail, egal ob es um Blueprint-Logik, prozedurale Generierung oder Multiplayer-Funktionen geht!