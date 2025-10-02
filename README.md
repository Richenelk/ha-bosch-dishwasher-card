# Bosch Dishwasher Card voor Home Assistant

Een uitgebreide Lovelace kaart voor het monitoren en bedienen van je Bosch Home Connect vaatwasser.

![Vaatwasser Actief](images/dishwasher-running-on.png)

## ✨ Functies

- 📊 **Voortgangsbalk** - Visuele weergave van de cyclus voortgang
- ⏱️ **Geschatte eindtijd** - Toont wanneer de vaatwasser klaar is (alleen tijdens actieve cyclus)
- 🚪 **Deur status** - Indicator voor open/dichte deur
- ▶️ **Bedieningsstatus** - Toont de huidige operatiestatus (Run, Ready, Finished, etc.)
- 🔌 **Aan/uit schakelaar** - Schakel de vaatwasser aan of uit
- 🏃 **Slimme start/stop knop** - Start het Auto 45-65° programma of stop de lopende cyclus
- 📈 **Percentage weergave** - Toont de exacte voortgang in procenten
- 📱 **Mobiel vriendelijk** - Responsive ontwerp dat goed werkt op alle apparaten

## 📋 Vereisten

### HACS Custom Cards
Installeer de volgende custom cards via HACS:

1. **stack-in-card**
   - HACS → Frontend → Zoek naar "stack-in-card"
   - [Repository](https://github.com/custom-cards/stack-in-card)

2. **lovelace-entity-progress-card**
   - HACS → Frontend → Zoek naar "entity-progress-card"
   - [Repository](https://github.com/berrywhite96/lovelace-entity-progress-card)

3. **mushroom-cards**
   - HACS → Frontend → Zoek naar "Mushroom"
   - [Repository](https://github.com/piitaya/lovelace-mushroom)

4. **card-mod**
   - HACS → Frontend → Zoek naar "card-mod"
   - [Repository](https://github.com/thomasloven/lovelace-card-mod)

### Integraties

- **Home Connect Alt** - Voor vaatwasser integratie
  - Installeer via HACS → Integraties → Zoek naar "Home Connect Alt"
  - [Repository](https://github.com/ekutner/home-connect-hass)
  - Deze alternatieve integratie biedt betere ondersteuning en meer functionaliteit dan de standaard Home Connect integratie

## 🚀 Installatie

### Stap 1: Template Sensor Configureren

1. Open je Home Assistant configuratie map
2. Als je nog geen `template.yaml` bestand hebt, maak er dan een aan
3. Kopieer de inhoud van [`template.yaml`](template.yaml) naar je bestand
4. Voeg het volgende toe aan je `configuration.yaml` (als je dat nog niet hebt):

```yaml
template: !include template.yaml
```

5. Herstart Home Assistant

### Stap 2: Script Toevoegen

1. Ga naar **Instellingen** → **Automatiseringen & Scènes** → **Scripts**
2. Klik op **+ Script Toevoegen**
3. Klik rechtsboven op de drie puntjes en kies **Bewerken als YAML**
4. Kopieer de volledige inhoud van [`dishwasher-smart-toggle-script.yaml`](dishwasher-smart-toggle-script.yaml)
5. Plak het in de editor en sla op

### Stap 3: Dashboard Card Toevoegen

1. Open je Home Assistant dashboard
2. Klik rechtsboven op de drie puntjes → **Dashboard bewerken**
3. Klik op **+ Kaart toevoegen**
4. Scroll helemaal naar beneden en klik op **Handmatig**
5. Kopieer de volledige inhoud van [`dishwasher-card.yaml`](dishwasher-card.yaml)
6. Plak het in de editor
7. Klik op **Opslaan**

## 🔧 Aanpassen

### Entity ID's Wijzigen

Als je vaatwasser andere entity ID's heeft, pas dan de volgende regels aan in `dishwasher-card.yaml`:

```yaml
sensor.dishwasher_program_progress          # Voortgang percentage
sensor.dishwasher_minutes_remaining         # Resterende minuten
sensor.dishwasher_active_program            # Actief programma
sensor.dishwasher_operation_state           # Bedieningsstatus
sensor.dishwasher_bsh_common_setting_powerstate  # Aan/uit status
binary_sensor.dishwasher_bsh_common_status_doorstate  # Deur status
switch.dishwasher_power                     # Aan/uit schakelaar
select.dishwasher_programs                  # Programma selectie
button.dishwasher_start_pause               # Start/pauze knop
button.dishwasher_stop                      # Stop knop
```

### Programma's Aanpassen

Om andere programma's toe te voegen of te wijzigen, bewerk de `program_map` in `dishwasher-card.yaml`:

```yaml
{% set program_map = {
  'Dishcare.Dishwasher.Program.Auto2': 'Auto 45-65°',
  'Dishcare.Dishwasher.Program.Eco50': 'Eco 50°',
  'Dishcare.Dishwasher.Program.Intensive70': 'Intensive 70°',
  'Dishcare.Dishwasher.Program.Quick45': 'Quick 45°',
  'Dishcare.Dishwasher.Program.PreRinse': 'Pre-Rinse',
  'Dishcare.Dishwasher.Program.Silent': 'Silent',
  'Dishcare.Dishwasher.Program.Normal65': 'Normal 65°'
} %}
```

### Standaard Start Programma Wijzigen

Het script start standaard het **Auto 45-65°** (Auto2) programma. Om dit te wijzigen, pas de volgende regel aan in `dishwasher-smart-toggle-script.yaml`:

```yaml
data:
  option: Dishcare.Dishwasher.Program.Auto2  # Wijzig naar gewenst programma
```

### Kleuren Aanpassen

Pas de kleuren aan in `dishwasher-card.yaml`:

```yaml
bar_color: '#5E8FCC'        # Voortgangsbalk kleur
icon_color: blue            # Hoofdicoon kleur
```

Status kleuren:
- **Groen** - Actief/Aan
- **Oranje** - Pauze/Deur open
- **Blauw** - Ready
- **Paars** - Afgerond
- **Rood** - Fout/Actie vereist
- **Grijs** - Inactief/Uit

## 📸 Screenshots

### Vaatwasser Actief met Voortgang
![Actieve Cyclus](images/dishwasher-running-on.png)
*Vaatwasser tijdens een actieve cyclus met voortgangsbalk en geschatte eindtijd*

### Vaatwasser Klaar - Deur Open
![Deur Open](images/dishwasher-open-on.png)
*Vaatwasser aan met deur open - oranje indicator toont deur status*

### Vaatwasser Uit - Deur Open
![Uitgeschakeld met Open Deur](images/dishwasher-open-off.png)
*Vaatwasser uitgeschakeld, deur status wordt nog steeds weergegeven*

### Vaatwasser Klaar - Deur Dicht
![Klaar voor Gebruik](images/dishwasher-closed-off.png)
*Vaatwasser in standby modus, deur gesloten en klaar om te starten*

## 🎯 Gebruik

### Start Knop (Operation State Chip)
- **Enkele tik**: Start Auto 45-65° programma of stop de lopende cyclus
- **Dubbele tik**: Pauzeert/hervat de huidige cyclus

### Aan/Uit Knop
- **Tik**: Schakel de vaatwasser aan of uit

### Deur Status
- **Grijs**: Deur is dicht
- **Oranje**: Deur is open

### Bedieningsstatus
- **Run** (Groen): Vaatwasser is actief bezig
- **Ready** (Blauw): Klaar om te starten
- **Finished** (Paars): Cyclus is voltooid
- **Pause** (Oranje): Cyclus is gepauzeerd
- **Delayed** (Cyaan): Vertraagde start
- **Error** (Rood): Fout opgetreden

## 🐛 Probleemoplossing

### De kaart toont "unavailable"
- Controleer of de Bosch Home Connect integratie correct is geconfigureerd
- Zorg dat je vaatwasser is ingeschakeld en verbonden met WiFi
- Verifieer de entity ID's in de kaart configuratie

### Het script werkt niet
- Controleer of het script correct is opgeslagen in Home Assistant
- Verifieer dat `script.dishwasher_smart_toggle` bestaat
- Check de entity ID's van de knoppen in het script

### De voortgangsbalk wordt niet weergegeven
- De voortgangsbalk wordt automatisch verborgen wanneer de vaatwasser uit staat of geen cyclus actief is
- Controleer of `sensor.dishwasher_program_progress` beschikbaar is

### "Program Progress" tekst loopt over de kaart
- Dit zou al opgelost moeten zijn met de korte naam
- Controleer je browser cache en ververs de pagina

## 🤝 Bijdragen

Voel je vrij om issues te openen of pull requests in te dienen voor verbeteringen!

## 📄 Licentie

Dit project is vrij te gebruiken en aan te passen voor persoonlijk gebruik.

## 👏 Credits

Gemaakt met ❤️ voor de Home Assistant community

---

**Vragen of problemen?** Open een issue op GitHub!
