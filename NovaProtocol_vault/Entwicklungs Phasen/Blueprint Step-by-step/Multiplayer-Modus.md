### **Step-by-Step Guide: Multiplayer-Modus hinzufügen**

Dieser Guide zeigt dir, wie du deinem Projekt in Unreal Engine 5 einen Multiplayer-Modus hinzufügst. Wir konfigurieren das **Online Subsystem**, erstellen grundlegende Netzwerklogik und testen den Multiplayer-Modus mit dedizierten Servern.

---

## **1. Online Subsystem aktivieren**

Das **Online Subsystem** ist ein Plugin, das dir die Integration von Multiplayer-Diensten wie **Steam**, **Epic Online Services** oder **LAN** ermöglicht. Für diesen Guide aktivieren wir das Subsystem und konfigurieren es.

---

### **1.1 Plugins aktivieren**

1. Gehe zu **Edit > Plugins**.
2. Suche nach **Online Subsystem** und aktiviere es.
3. Falls du **Steam** verwenden möchtest, aktiviere zusätzlich:
    - **Online Subsystem Steam**
4. Starte die Unreal Engine neu, damit die Plugins korrekt geladen werden.

---

### **1.2 Projekt konfigurieren**

1. Gehe zu **Edit > Project Settings**.
    
2. Suche links nach **Maps & Modes**:
    
    - Setze die **Game Default Map** und die **Editor Startup Map** (z. B. deine Hauptkarte).
3. Suche nach **Packaging**:
    
    - Setze `Use Pak File` auf **true** (für fertige Builds).
4. Konfiguriere die **DefaultEngine.ini** für das Online Subsystem: 	    
	- Öffne die Datei `DefaultEngine.ini` in deinem Projektordner:
	```csharp 
	[OnlineSubsystem]
	DefaultPlatformService=Null 
	```
	- Falls du **Steam** nutzen möchtest, ändere DefaultPlatformService=Steam.
5. Falls du Steam nutzt, füge die Steam-App-ID hinzu:
    
    - Ergänze in `DefaultEngine.ini`:
	```csharp 
	[OnlineSubsystem]
	DefaultPlatformService=Null 
	```
_(App ID 480 ist die Standard-ID für Tests mit Steam. Für veröffentlichte Spiele benötigst du eine eigene App-ID.)_

---

## **2. Netzwerkfähigkeit für Blueprints**

Unreal Engine bietet leistungsstarke Werkzeuge für Multiplayer-Logik. Du kannst Server-Client-Kommunikation mit **RPCs (Remote Procedure Calls)** und replizierten Variablen implementieren.

---

### **2.1 Blueprint-Logik für einen Multiplayer-Charakter**

#### **Schritt 1: Character-Blueprint für Netzwerk vorbereiten**

1. Öffne deinen **BP_PlayerCharacter**.
2. Aktiviere die Replikation:
    - Gehe zu **Details Panel** (klicke auf die **Root-Komponente** des Blueprints).
    - Aktiviere **Replicates** und **Always Relevant**.

#### **Schritt 2: Replizierte Variablen hinzufügen**

1. Öffne den **My Blueprint**-Bereich.
2. Erstelle eine neue Variable, z. B. `Health` (Float).
3. Aktiviere die Replikation:
    - Wähle die Variable aus und aktiviere im **Details Panel** die Checkbox **Replicate**.
    - Dies synchronisiert den Variablenwert zwischen Server und Client.
4. Wiederhole diesen Schritt für alle wichtigen Variablen (z. B. Sauerstoff, Energie, etc.).

#### **Schritt 3: Bewegungslogik testen**

1. Unreal Engine synchronisiert Bewegungen automatisch für Charaktere, die von der **Character-Klasse** abgeleitet sind.
2. Teste, ob dein Charakter korrekt über das Netzwerk gesteuert wird:
    - Starte ein Spiel mit mehreren Clients (siehe Abschnitt **3. Testen**).

---

### **2.2 Funktionen mit RPCs (Remote Procedure Calls)**

RPCs ermöglichen es dir, Funktionen entweder auf dem Server oder auf den Clients auszuführen.

#### **Schritt 1: Funktion erstellen**

1. Öffne den **BP_PlayerCharacter**.
2. Erstelle eine neue Funktion, z. B. `DealDamage`:
    - Parameter: `DamageAmount` (Float).
    - Logik: Ziehe die `Health`-Variable in den Graph und reduziere ihren Wert basierend auf `DamageAmount`.

#### **Schritt 2: RPC für den Server erstellen**

1. Wähle die Funktion `DealDamage` aus.
2. Gehe zu den **Details Panel** der Funktion.
3. Setze **Replicates** auf:
    - **Run on Server**: Diese Funktion wird nur auf dem Server ausgeführt.
4. Logik hinzufügen:
    - Rufe die Funktion über ein Event (z. B. "On Fire") auf.
    - Der Server aktualisiert den `Health`-Wert und repliziert die Änderung an die Clients.

#### **Schritt 3: RPC für Clients erstellen**

1. Füge eine neue Funktion hinzu, z. B. `UpdateHealthUI`:
    - Diese Funktion aktualisiert das HUD auf den Clients.
2. Setze **Replicates** auf:
    - **Multicast**: Diese Funktion wird auf allen Clients (und dem Server) ausgeführt.
3. Rufe die Funktion `UpdateHealthUI` innerhalb der Serverfunktion `DealDamage` auf.

---

### **2.3 Game Mode für Multiplayer**

1. **Game Mode anpassen:**
    - Öffne deinen Game Mode Blueprint (z. B. `BP_GameMode`).
    - Stelle sicher, dass:
        - **Default Pawn Class** auf deinen `BP_PlayerCharacter` gesetzt ist.
        - **Player Controller Class** auf `BP_PlayerController` gesetzt ist (falls vorhanden).
2. Füge eine Logik hinzu, um Spieler automatisch zu spawnen:
    - Standardmäßig wird der Game Mode dies automatisch übernehmen, aber du kannst dies übersteuern, falls du spezifische Spawnpunkte verwenden möchtest.

---

## **3. Multiplayer-Modus testen**

Unreal Engine bietet mehrere Möglichkeiten, Multiplayer-Umgebungen zu testen. Hier sind die wichtigsten Methoden:

---

### **3.1 Lokales Multiplayer-Testen**

1. Gehe zu **Play** (oben im Editor).
2. Klicke auf den kleinen Pfeil neben dem Play-Button und wähle:
    - **Number of Players:** Stelle die Anzahl der Spieler ein (z. B. 2).
    - **Net Mode:** Wähle **Play as Listen Server** (ein Spieler agiert als Server).
3. Starte das Spiel:
    - Du siehst zwei Fenster: eines für den Server und eines für den Client.
    - Teste, ob Bewegungen, Interaktionen und Variablen synchronisiert sind.

---

### **3.2 Dedizierter Server**

1. Gehe zu **Play > Advanced Settings**.
2. Aktiviere **Run Dedicated Server**.
3. Starte das Spiel:
    - Jetzt agiert der Server unabhängig von den Clients.
    - Teste, ob die Logik weiterhin korrekt funktioniert.

---

### **3.3 Multiplayer-Session (z. B. LAN/Steam)**

1. Implementiere eine Session-Logik:
    - Öffne deinen **Game Instance Blueprint** (z. B. `BP_GameInstance`).
    - Füge Funktionen hinzu:
        - **Create Session:** Startet eine Multiplayer-Session.
        - **Find Session:** Sucht nach verfügbaren Sessions.
        - **Join Session:** Tritt einer existierenden Session bei.
2. Unreal bietet Nodes wie:
    - `Create Session`
    - `Find Sessions`
    - `Join Session`
    - Diese kannst du in deinem Menü oder HUD nutzen, um den Multiplayer-Zugang zu erleichtern.
3. Teste, ob Clients Sessions beitreten können:
    - Erstelle eine Session auf einem Rechner und finde diese Session auf einem anderen.

---

## **Zusammenfassung**

Nach diesen Schritten hast du einen grundlegenden Multiplayer-Modus eingerichtet:

- Das **Online Subsystem** ist aktiviert und konfiguriert (z. B. für LAN oder Steam).
- Dein Charakter und wichtige Variablen sind repliziert.
- RPCs werden verwendet, um Server-Client-Kommunikation zu ermöglichen.
- Du kannst den Multiplayer-Modus lokal oder auf dedizierten Servern testen.

## Weitere, zu ergänzende Multiplayer-Funktionen:
- Lobby-Systeme
- Inventar-Synchronisation
- Welten-Synchronisation (Universen-Synchronisation)