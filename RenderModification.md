

## Breath Sounds
```
<?xml version="1.0" encoding="UTF-8"?>
<RenderModification type="Breath Sounds">
   <BreathQuality>
      <!-- Exactly one of the following lines (Enum) -->
      Normal
      Vesicular
      Bronchial
      Diminished
      Crackles
      Rhonchi
      Pleural rub
      Wheeze
      Cough
      Egophany
      Pectoriloquy
      Dullness to percussion
      Hyperresonance to percussion
      Fremitus
   </BreathQuality>
   <Severity value="0 to 1"/>
</RenderModification>
```

## Hypoxia
```
<?xml version="1.0" encoding="UTF-8"?>
<RenderModification type="Hypoxia">
   <!-- Normally Location is redundant because it’s included in the
   Event Record metadata, but this Location may be different. -->
   <Location value="FMAID number for affected region of body"/>
   <Severity value="0 to 1"/>
</RenderModification>
```

## IV Access
```
<?xml version="1.0" encoding="UTF-8"?>
<RenderModification type="IV Access">
   <Cannula size="gauge"/>
</RenderModification>
```
