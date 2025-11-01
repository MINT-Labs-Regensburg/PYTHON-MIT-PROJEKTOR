
# Pygame Spiele mit dem EVIYOS projizieren.  

<p align="center">
    <img src="assets/Projektor.jpg" alt="Projektor"/>
    <br/>
    <em>Der <a href="https://ams-osram.com/de/products/product-families/eviyos" target="_blank">EVIYOS Projektor</a> mit 25.600 einzeln ansteuerbaren LEDs. Damit können zum Beispiel Autoscheinwerfer so angesteuert werden, dass entgegenkommende Fahrzeuge nicht geblendet werden. Im Bild is ein Demonstrator mit Raspberry, wie wir ihn im Kurs verwenden</em>
</p>


### Herzlichen Dank
| Ein herzliches Dankeschön an Dr. Norwin von Malm und Stefan Grötsch – die Preisträger des [Deutschen Zukunftspreises 2024](https://www.deutscher-zukunftspreis.de/de/team-1-2024).<br>Mit ihrer Spende und ihrer großzügigen Unterstützung haben Sie die Entwicklung und Durchführung dieses Kurses ermöglicht. | <img src="assets/DZP_Logo_2.svg" alt="DZP Logo" width="120"/> |
|:---|:---:|


### Auf dem Raspberry Pi des EVIYOS Projectors ist bereits ein Projekt-Gerüst vorbereitet, dem du dein eigenes Pygame hinzufügen kannst. 

## Visual Studio Code öffnen mit Application Menu → Programming → Visual Studio Code

## Öffne Projektfolder in Visual Studio Code

Öffne Visual Studio Code und wähle im Menü **Datei → Ordner öffnen...** den Ordner `MintLabs/python-mit-projector` aus.  

### Im Ordner neue Datei `mein_pygame.py` erstellen

Erstelle eine neue Python-Datei namens `mein_pygame.py` im Verzeichnis `MintLabs/python-mit-projector`. Füge folgenden Beispielcode ein.



```python
import pygame

pygame.init()

# Bildschirmauflösung ENVIOS projector 320*80
screen = pygame.display.set_mode((320, 80))
pygame.display.set_caption('Mein Pygame')

# Farben (EVIYOS projector kann nur Schwarz Weiss Grau)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GRAY = (128, 128, 128)

running = True

while running:
    # Handle events, damit das Programm mit `x` rechts oben im Fenster beendet werden kann
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    screen.fill(GRAY)
    pygame.display.flip()

pygame.quit()
```

Wenn du das Porgramm startest wird, wie gewohnt der Pygame Bildschirm auf dem Monitor ausgegeben - Aber kleiner, da er nur 320x80 Pixel hat.

---
## Jetzt kommt der Projektor ins Spiel
## Das Programm so erweitern, dass dein pygame screen gleichzeitig auch auf dem Projektor zu sehen ist

**Wichtig:** Der Projector verlangt eine Auflösung **320×80 Pixel** und verwendet nur **schwarz weiss** Töne. 

Um den Projektor anzusteuern sind folgende Schritte nötig:
1. Die Projectorklasse importieren: `from helpers.eviyos_projector import EVIYOSProjector`
2. Den projector starten: `projector = EVIYOSProjector()`
2. Den pygame screen auf den projector spiegeln: `projector.display()`
4. Den projector am Ende des Programms schließen: `projector.close()`



Zunächst ganz oben im code nach `import pygame` den `EVIOSProjector`  importieren und die Instanz `projector` starten

```python
import pygame
from helpers.eviyos_projector import EVIYOSProjector
projector = EVIYOSProjector()
```


Spiegle den pygame screen auf dem projector mit `.display(screen)` . Jedes mal, nachdem der pygame screen upgedated wurde
```python
pygame.display.flip()
projector.display(screen)
```

Schließe den projector am Ende des Programms mit `.close()`
```python
pygame.quit()
projector.close()
```

Ein Minimal-Programm, das einen einfachen Pygame screen auf dem Projektor auswirft sieht dann so aus:
```python
import pygame

from helpers.eviyos_projector import EVIYOSProjector
projector = EVIYOSProjector()

pygame.init()

# Bildschirmauflösung ENVIOS projector 320*80
screen = pygame.display.set_mode((320, 80))
pygame.display.set_caption('Mein Pygame')

# Farben (EVIYOS projector kann nur Schwarz Weiss Grau)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GRAY = (128, 128, 128)

running = True

while running:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    screen.fill(GRAY)
    pygame.display.flip()
    projector.display(screen)

pygame.quit()
projector.close()
```

