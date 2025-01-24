### **Step-by-Step Guide: Raumschiffsteuerung (Weltraumbewegung)**

Dieser Guide beschreibt detailliert, wie du ein steuerbares Raumschiff in Unreal Engine 5 erstellst. Am Ende hast du ein funktionierendes Raumschiff, das auf Eingaben reagiert und sich im Raum bewegt.

---

### **1. Blueprint erstellen**

1. **Blueprint erstellen**
    
    - Gehe zu **Content Browser**.
    - Rechtsklicke auf einen leeren Bereich und wähle **Blueprint Class**.
    - Wähle **Pawn** als Basisklasse (da Pawns steuerbare Objekte sind).
    - Benenne die Blueprint z. B. `BP_Spaceship`.
2. **Static Mesh Component hinzufügen**
    
    - Öffne `BP_Spaceship`.
    - Klicke oben links im **Components Panel** auf **Add Component** und füge ein `Static Mesh` hinzu.
    - Benenne die Komponente z. B. `SpaceshipMesh`.
    - Wähle im **Details Panel** ein Mesh aus (z. B. ein Raumschiff-Asset oder eine primitive Form wie eine Kugel oder Box).
3. **Camera Component hinzufügen**
    
    - Klicke erneut auf **Add Component** und füge eine **Camera** hinzu.
    - Benenne sie z. B. `SpaceshipCamera`.
    - Positioniere die Kamera hinter dem Raumschiff für eine Third-Person-Ansicht:
        - Verschiebe die Kamera entlang der X- und Z-Achse im **Viewport** (z. B. auf `X = -500`, `Z = 200`).
4. **Scene Root anpassen (falls nötig)**
    
    - Stelle sicher, dass die **Scene Root** korrekt als Haupt-Komponente fungiert und alle anderen Komponenten untergeordnet sind.

---

### **2. Bewegung implementieren**

Jetzt erstellen wir Eingaben und fügen die Bewegungslogik für das Raumschiff hinzu.

#### **2.1 Eingaben konfigurieren**

1. Gehe zu **Edit > Project Settings**.
    
2. Navigiere zu **Input** im linken Menü.
    
3. Scrolle herunter zu **Action Mappings** und **Axis Mappings**:
    
    - Unter **Action Mappings** kannst du Tasten für bestimmte Aktionen definieren.
    - Unter **Axis Mappings** kannst du Eingaben mit einem kontinuierlichen Wert definieren (z. B. für Steuerungen wie Beschleunigung und Drehungen).
4. Füge folgende Mappings hinzu:
    
    - **Axis Mappings:**
        - `Thrust` → **Taste:** W (Scale = 1) und S (Scale = -1).
        - `Pitch` → **Mausachse:** Mouse Y (Scale = -1).
        - `Yaw` → **Mausachse:** Mouse X (Scale = 1).
        - `Roll` → **Taste:** Q (Scale = -1) und E (Scale = 1).

---

#### **2.2 Bewegungslogik implementieren**

1. Öffne `BP_Spaceship` und gehe in die **Event Graph**.
    
2. **Thrust (Vorwärts-/Rückwärtsbewegung):**
    
    - Ziehe aus der **Event Graph** eine neue Node namens `InputAxis Thrust`.
    - Verbinde den Ausgang von `Axis Value` mit einer `Add Force` Node:
        - Klicke mit Rechts auf das Node-Feld und suche nach `Add Force`.
    - Verbinde die **Target**-Pin von `Add Force` mit deinem `SpaceshipMesh`.
    - Berechne die Richtung des Schubs:
        - Ziehe den **Get Forward Vector** der `SpaceshipMesh` Node und multipliziere ihn mit der `Axis Value`.
        - Verbinde das Ergebnis mit dem **Force**-Pin der `Add Force` Node.
    - Beispielgrafik:
    
``InputAxis Thrust → Multiply (Forward Vector * Axis Value) → Add Force  

- **Pitch (Rotation nach oben/unten):**
    
    - Ziehe eine neue Node `InputAxis Pitch`.
    - Füge eine `Add Torque` Node hinzu.
    - Berechne das Drehmoment:
        - Multipliziere den **Right Vector** des `SpaceshipMesh` mit der `Axis Value`.
        - Skaliere den Wert, um die Rotation zu kontrollieren (z. B. mit einem Multiplikator wie `5000`).
    - Verbinde das Ergebnis mit dem **Torque**-Pin.
- **Yaw (Rotation nach links/rechts):**
    
    - Ziehe eine neue Node `InputAxis Yaw`.
    - Wiederhole die Schritte wie bei Pitch, aber nutze den **Up Vector** des `SpaceshipMesh`.
- **Roll (Rollen nach links/rechts):**
    
    - Ziehe eine neue Node `InputAxis Roll`.
    - Wiederhole die Schritte wie bei Pitch, aber nutze den **Forward Vector** des `SpaceshipMesh`.

---

### **3. Raumschiffphysik aktivieren**

Damit sich das Raumschiff realistisch verhält, aktivieren wir die Physik.

1. Wähle im **Viewport** die Komponente `SpaceshipMesh` aus.
    
2. Im **Details Panel** aktiviere die Option **Simulate Physics**.
    
3. Passe folgende Werte im **Details Panel** an:
    
    - **Mass:** Stelle eine passende Masse ein (z. B. 500 kg).
    - **Linear Damping:** 0.2 (verhindert endloses Gleiten).
    - **Angular Damping:** 0.4 (verhindert unkontrolliertes Rotieren).
4. Optional:
    
    - Füge bei Bedarf Constraints hinzu, z. B. für Achsen, auf denen keine Bewegung erlaubt sein soll.

---

### **4. Gameplay testen**

1. Gehe zurück zur Hauptkarte und ziehe `BP_Spaceship` in die Szene.
2. Wähle das Raumschiff aus und gehe ins **Details Panel**.
    - Setze **Auto Possess Player** auf `Player 0`, damit du das Raumschiff sofort steuern kannst.
3. Drücke **Play** (Taste: `P` oder im oberen Menü).
4. Teste, ob das Raumschiff:
    - Auf die Eingaben reagiert (z. B. Vorwärtsbewegung mit W).
    - Sich korrekt dreht (Mausbewegung steuert Pitch/Yaw).
    - Rollbewegungen korrekt umsetzt (Q/E).

---

### **5. Optimierungen und Debugging**

1. **Kamera anpassen:**
    
    - Justiere die Position und den Winkel der Kamera, um eine optimale Ansicht zu erhalten.
    - Optional: Füge eine **Spring Arm Component** hinzu, damit die Kamera geschmeidig folgt.
2. **Feedback für Spieler hinzufügen:**
    
    - Füge Partikeleffekte (z. B. Triebwerksflammen) oder Soundeffekte hinzu, die auf den Schub reagieren.
    - Beispiel: Verändere die Skalierung eines Partikelsystems basierend auf der `Thrust`-Achse.
3. **Debugging:**
    
    - Nutze **Print Strings**, um Eingaben und Bewegungen zu überprüfen.
    - Füge ein `Physics Debugger`-Tool hinzu, um Kräfte und Drehmomente sichtbar zu machen.

---