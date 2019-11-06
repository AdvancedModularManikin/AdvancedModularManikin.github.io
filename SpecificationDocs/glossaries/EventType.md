


##### Auscultate
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="Auscultate">
   <Instrument>Usually "Stethoscope" (maybe "Ear"?)</Instrument>
</EventRecord>
```

##### IV Access
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="IV Access">
   <Cannula size="gauge"/>
</EventRecord>
```

##### Palpate
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="Palpate">
   <Pressure value="applied pressure" units="pressure units"/>
</EventRecord>
```

##### Substance Bolus
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="Substance Bolus">
   <!-- Tags to match BioGears Actions -->
   <Substance>Substance Name</Substance>
   <Concentration value="a value" unit="mass per volume unit"/>
   <Dose value="bolus amount" unit="volume unit"/>
   <!-- Additional tag to match Attribute of Action tag -->
   <AdminRoute>Intravenous</AdminRoute>
</EventRecord>
```

##### Substance Compound Infusion
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="Substance Compound Infusion">
   <!-- Tags to match BioGears Actions -->
   <SubstanceCompound>Compound Name</SubstanceCompound>
   <BagVolume value="size of bag" unit="volume unit"/>
   <Rate value="rate of administration" unit="volume per time unit"/>
</EventRecord>
```

##### Substance Infusion
```
<?xml version="1.0" encoding="UTF-8"?>
<EventRecord type="Substance Infusion">
   <!-- Tags to match BioGears Actions -->
   <Substance>Substance Name</Substance>
   <Concentration value="a value" unit="mass per volume unit"/>
   <Rate value="rate of administration" unit="volume per time unit"/>
</EventRecord>
```
