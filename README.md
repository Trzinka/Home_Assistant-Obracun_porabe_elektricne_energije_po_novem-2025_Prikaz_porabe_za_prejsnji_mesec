# Ta git je nadgradnja https://github.com/Trzinka/Home_Assistant-Obracun_porabe_elektricne_energije_po_novem-2025, ki ga morate predhodno prilagoditi svojim potrebam!

V `configuration.yaml` dodajte `sql: !include sql.yaml` in v korenskem imeniku naredite datoteko `sql.yaml` kot prikazuje slika.

![configuration yaml](https://github.com/user-attachments/assets/edb66835-7dfb-4df3-af4f-6d358a8500bc)

V datoteko `sql.yaml` dodajte:

🔴 **Popravljeno 02.04.2025**
```yaml
- name: "Prejšnji mesec P1 meter faza3 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.p1_meter_phase_3_mesecno_kwh'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"
#===========================================================================
- name: "Prejšnji mesec P1 meter faza3 blok1 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.tarife_p1_meter_faza3_mesecno_1'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"

- name: "Prejšnji mesec P1 meter faza3 blok2 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.tarife_p1_meter_faza3_mesecno_2'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"

- name: "Prejšnji mesec P1 meter faza3 blok3 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.tarife_p1_meter_faza3_mesecno_3'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"

- name: "Prejšnji mesec P1 meter faza3 blok4 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.tarife_p1_meter_faza3_mesecno_4'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"

- name: "Prejšnji mesec P1 meter faza3 blok5 mesečno"
  query: >
    SELECT 
    CAST(Round(MAX(s.state), 0) AS INTEGER) AS state
    FROM statistics s
    JOIN statistics_meta sm ON s.metadata_id = sm.id
    WHERE sm.statistic_id = 'sensor.tarife_p1_meter_faza3_mesecno_5'
      AND s.state IS NOT NULL
      AND s.start_ts <= strftime('%s', 'now', 'start of month', '-1 second')
      AND s.start_ts > strftime('%s', 'now', 'start of month', '-2 day')
      AND s.state > 0
    GROUP BY strftime('%Y-%m-%d', datetime(s.start_ts, 'unixepoch'))
    ORDER BY s.start_ts DESC
    LIMIT 1;
  column: 'state'
  unit_of_measurement: "kWh"
```

Ta koda dela poizvedbo v podatkovni datoteki home assistant `(home-assistant_v2.db)`
Priporočam da dodate v `configuration.yaml` sledeč:

🔴 **Popravljeno 02.04.2025**
```yaml
recorder:
  purge_keep_days: 1095
  purge_interval: 7
  db_url: sqlite:///home-assistant_v2.db
```

kar bo omogočilo hranjenje podatkov za v mojem primeru 1.095 dni (cca 3 leta) in da bo vsakih 7 dni brisalo starejše zapise od 1.095 dni.

🟢 Privzeta vrednost je 10, kar pomeni, da se podatki hranijo do 10 dni.

🔵 **Nastavite vsaj 62.**

*****************************************************************************************

![Sensors](https://github.com/user-attachments/assets/9f5979c0-de7b-487e-9a92-6b2c80028b0c)


V mapi `share` in podmapi `sensors` ustvarite datoteke `dogovorjena_moc_polovica.yaml`, `elektrika_cenik_prejsnji.yaml`, `elektrika_obracun_prejsnji.yaml` in še `dogovorjena_moc_polovica.yaml` in `elektrika_obracun_prejsnji_grm.yaml`, če živite v dvo družinski hiši in delite izračun.

V datoteko `dogovorjena_moc_polovica.yaml` vpišite:

```yaml
- platform: template
  sensors:
    moj_elektro_casovni_blok_1_polovica:
      friendly_name: "Moj Elektro Casovni Blok 1 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_1') | float }}"
      unique_id: 4525ad63-7144-4ba8-95f6-7aa649b3879c

- platform: template
  sensors:
    moj_elektro_casovni_blok_2_polovica:
      friendly_name: "Moj Elektro Casovni Blok 2 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_2') | float }}"
      unique_id: 4301a898-a54c-411c-ba1a-1759e8f0676d

- platform: template
  sensors:
    moj_elektro_casovni_blok_3_polovica:
      friendly_name: "Moj Elektro Casovni Blok 3 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_3') | float }}"
      unique_id: 38be6300-1de6-4a0c-a24e-b78abbcb6dfb

- platform: template
  sensors:
    moj_elektro_casovni_blok_4_polovica:
      friendly_name: "Moj Elektro Casovni Blok 4 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_4') | float }}"
      unique_id: 9b0c24d9-e872-479b-81a1-d2df008e2162

- platform: template
  sensors:
    moj_elektro_casovni_blok_5_polovica:
      friendly_name: "Moj Elektro Casovni Blok 5 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_5') | float }}"
      unique_id: c17ef3b7-63bd-499a-a57d-08b870b11414
```


V datoteko `elektrika_cenik_prejsnji.yaml` vpišite:

```yaml
#============================================
# Cene
#============================================
  - platform: template
    sensors:
      prejsnja_cena_elektricne_energije_et:
        friendly_name: "Perjšnja cena električne energije ET"
        value_template: "0.113900"
        unit_of_measurement: "EUR/kWh"
        unique_id: 210325e3-acc3-46cd-b3c5-80b3487b5511
#============================================
      prejsnja_cena_dogovorjena_moc_casovni_blok_1:
        friendly_name_template: "Perjšnja cena za dogovorjeno moč za časovni blok 1"
        value_template: "0.0"
        unit_of_measurement: EUR/kW
        unique_id: 44fd5814-ffa7-491f-9129-7f8c8ab18c89

      prejsnja_cena_dogovorjena_moc_casovni_blok_2:
        friendly_name_template: "Perjšnja cena za dogovorjeno moč za časovni blok 2"
        value_template: "0.912240"
        unit_of_measurement: EUR/kW
        unique_id: 66346327-dc44-4745-b9d1-1809c9a18c5d

      prejsnja_cena_dogovorjena_moc_casovni_blok_3:
        friendly_name_template: "Perjšnja cena za dogovorjeno moč za časovni blok 3"
        value_template: "0.162970"
        unit_of_measurement: EUR/kW
        unique_id: 94198ef6-a0ed-47e4-8c6c-cf5276877daf

      prejsnja_cena_dogovorjena_moc_casovni_blok_4:
        friendly_name_template: "Perjšnja cena za dogovorjeno moč za časovni blok 4"
        value_template: "0.004070"
        unit_of_measurement: EUR/kW
        unique_id: ef40e5b7-e028-40b3-923f-9eb23f8c6bed
      
      prejsnja_cena_dogovorjena_moc_casovni_blok_5:
        friendly_name_template: "Perjšnja cena za dogovorjeno moč za časovni blok 5"
        value_template: "0.000000"
        unit_of_measurement: EUR/kW
        unique_id: d6e59adb-eb69-4f18-a2c5-ac75f7f8e291
 #=================    
      prejsnja_cena_prevzeta_ee_casovni_blok_1:
        friendly_name_template: "Perjšnja cena za prevzeto EE za časovni blok 1"
        value_template: "0.0"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 65a16c54-c10d-42b5-a1c9-6db24d3fa815

      prejsnja_cena_prevzeta_ee_casovni_blok_2:
        friendly_name_template: "Perjšnja cena za prevzeto EE za časovni blok 2"
        value_template: "0.018330"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 87a4382e-b31c-4fad-a6b6-e8fe759b5a50

      prejsnja_cena_prevzeta_ee_casovni_blok_3:
        friendly_name_template: "Perjšnja cena za prevzeto EE za časovni blok 3"
        value_template: "0.018090"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 8274cad6-4fe5-4c20-b009-a3983edfd7e5

      prejsnja_cena_prevzeta_ee_casovni_blok_4:
        friendly_name_template: "Perjšnja cena za prevzeto EE za časovni blok 4"
        value_template: "0.018550"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 7122a5a4-8cf5-4aba-b6e0-e42cee5d401e

      prejsnja_cena_prevzeta_ee_casovni_blok_5:
        friendly_name_template: "Perjšnja cena za prevzeto EE za časovni blok 5"
        value_template: "0.018730"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: b7aacd93-5780-41b7-b637-4b857b55d52d
#============================================
      prejsnja_cena_prispevka_za_delovanje_operaterja_trga:
        friendly_name_template: "Perjšnja cena prispevka za delovanje operaterja trga"
        value_template: "0.000130"
        unit_of_measurement: EUR/kWh
        unique_id: 9364a03e-080e-4eee-a6b6-b381c0e8d28a

      prejsnja_cena_prispevka_za_energetsko_ucinkovitost:
        friendly_name_template: "Perjšnja cena prispevka za energetsko učinkovitost"
        value_template: "0.000800"
        unit_of_measurement: EUR/kWh
        unique_id: beb87190-d6c2-4821-8d4b-393e4e21f8a7

      prejsnja_cena_prispevka_za_spte_in_ove:
        friendly_name_template: "Perjšnja cena prispevka za SPTE in OVE"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: 79199c64-92a4-476f-bac5-985ad9552ad5
#============================================
      prejsnja_cena_trosarine:
        friendly_name_template: "Perjšnja cena trošarine"
        value_template: "0.001530"
        unit_of_measurement: EUR/kWh
        unique_id: c1b5686b-122f-4281-a9d9-36ac032fb16e
#============================================
      prejsnja_cena_jederske_energije:
        friendly_name_template: "Perjšnja cena jederske energije"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: 2018b622-abf1-40fb-a87f-82612fbb213b

      prejsnja_cena_pavsalnih_stroskov:
        friendly_name_template: "Perjšnja cena pavšalnih stroškov"
        value_template: "1.990000"
        unit_of_measurement: EUR
        unique_id: 14fd8b69-d4c5-43fa-a1c1-2ca48287cd40

      prejsnja_cena_e_popusta:
        friendly_name_template: "Perjšnja cena E-popusta"
        value_template: "0.810000"
        unit_of_measurement: EUR
        unique_id: fecc8994-760a-478d-964e-2eeb9327ce44
```

V datoteko `elektrika_obracun_prejsnji.yaml` vpišite:

```yaml
#=========================================================================================================================================================
#                          Obračun elektrike za prejšnji mesec
#=========================================================================================================================================================
#============================================
# Izračun stroška električne energije
#============================================ 
  - platform: template
    sensors:
      prejsnji_skupaj_izracun_stroska_elektricne_energije:
        friendly_name: "Prejšnji skupaj izračun stroška električne energije"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_elektricne_energije_et') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 7f6160d0-d102-4d57-acbe-f01d2c286dc3
#============================================
# Izračun stroška omrežnine za elektroenergetski sistem
#============================================
  - platform: template
    sensors:
      prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok1:
        friendly_name: "Prejšnji izračun stroška dogovorjene moči za časovni blok 1"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_1') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_1') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR/kW"
        unique_id: 880189b1-1523-4819-98d9-ffa9236f6fd2

  - platform: template
    sensors:
      prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok2:
        friendly_name: "Prejšnji izračun stroška dogovorjene moči za časovni blok 2"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_2') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_2') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR/kW"
        unique_id: 5da1b87c-34c2-4038-bc3d-cec309a55291

  - platform: template
    sensors:
      prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok3:
        friendly_name: "Prejšnji izračun stroška dogovorjene moči za časovni blok 3"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_3') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_3') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR/kW"
        unique_id: 997194bc-e858-4ed5-93c0-556067ea5059

  - platform: template
    sensors:
      prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok4:
        friendly_name: "Prejšnji izračun stroška dogovorjene moči za časovni blok 4"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_4') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_4') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR/kW"
        unique_id: 9269c795-17ff-4ad9-99b1-9940d52d7e29

  - platform: template
    sensors:
      prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok5:
        friendly_name: "Prejšnji izračun stroška dogovorjene moči za časovni blok 5"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_5') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_5') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR/kW"
        unique_id: 1c181d01-81fb-4676-9331-304f324ebe4e
#===
# SKUPAJ
#===
  - platform: template
    sensors:
      prejsnji_skupaj_izracun_stroska_dogovorjene_moci:
        friendly_name: "Prejšnji skupaj izračun stroška dogovorjene moči"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok1') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok2') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok3') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok4') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok5') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: 69e7e7f9-4ae3-4113-a72a-742057aa7831        
 #=================  
  - platform: template
    sensors:
      prejsnji_izracun_stroska_prevzete_ee_casovni_blok1:
        friendly_name: "Prejšnji izračun stroška prevzete EE za časovni blok 1"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_1') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_blok1_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: e84a39d5-bb95-4312-852d-0f3e6cb18bf5

  - platform: template
    sensors:
      prejsnji_izracun_stroska_prevzete_ee_casovni_blok2:
        friendly_name: "Prejšnji izračun stroška prevzete EE za časovni blok 2"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_2') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_blok2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 6381050a-c267-4aa8-b4b5-425fc09929b6

  - platform: template
    sensors:
      prejsnji_izracun_stroska_prevzete_ee_casovni_blok3:
        friendly_name: "Prejšnji izračun stroška prevzete EE za časovni blok 3"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_3') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_blok3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 9823c296-92e3-45e2-a350-abe44ece10bf

  - platform: template
    sensors:
      prejsnji_izracun_stroska_prevzete_ee_casovni_blok4:
        friendly_name: "Prejšnji izračun stroška prevzete EE za časovni blok 4"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_4') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_blok4_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 889ebb92-5c35-4238-ae67-95b6b87e8d16

  - platform: template
    sensors:
      prejsnji_izracun_stroska_prevzete_ee_casovni_blok5:
        friendly_name: "Prejšnji izračun stroška prevzete EE za časovni blok 5"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_5') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_blok5_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 5d33b72a-3543-4fbf-aaa3-d227c516e3ca
#===
# SKUPAJ
#===
  - platform: template
    sensors:
      prejsnji_skupaj_izracun_stroska_prevzete_ee:
        friendly_name: "Prejšnji skupaj izračun stroška prevzete EE"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok1') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok2') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok3') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok4') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok5') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: e1de71fa-2b04-4a46-b2cf-448184050c1b 
#============================================
# Izračun stroška prispevkov in ostalih dajatev
#============================================
      prejsnji_izracun_stroska_prispevka_za_delovanje_operaterja_trga:
        friendly_name: "Prejšnji izračun stroška prispevka za delovanje operaterja trga"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prispevka_za_delovanje_operaterja_trga') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: d8f476d7-2321-4711-8b8b-f6e993872022

      prejsnji_izracun_stroska_prispevka_za_energetsko_ucinkovitost:
        friendly_name: "Prejšnji izračun stroška prispevka za energetsko učinkovitost"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prispevka_za_energetsko_ucinkovitost') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: badd97ba-a781-497e-9836-260a45ea37dd

      prejsnji_izracun_stroska_prispevka_za_spte_in_ove:
        friendly_name: "Prejšnji izračun stroška prispevka za SPTE in OVE"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prispevka_za_spte_in_ove') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_2') | float(default=0) / 2)) }}
        unit_of_measurement: "EUR"
        unique_id: 8b728e0a-e332-494b-9f4c-ccb141e5c5be
#===
# SKUPAJ
#===
      prejsnji_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev:
        friendly_name: "Prejšnji skupaj izračun stroška prispevkov in ostalih dajatev"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_izracun_stroska_prispevka_za_delovanje_operaterja_trga') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prispevka_za_energetsko_ucinkovitost') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prispevka_za_spte_in_ove') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: 96db7ad5-b8b9-4d73-a15d-d553b5f7af59
#============================================
# Izračun stroška trošarine
#============================================
      prejsnji_skupaj_izracun_stroska_trosarine:
        friendly_name: "Prejšnji skupaj izračun stroška trošarine"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_trosarine') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: d4effd80-7314-4429-a060-94054afb1e8f
#============================================
# Izračun stroška storitve pogodbenega računa
#============================================
      prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna:
        friendly_name: "Prejšnji skupaj izračun stroška storitev pogodbenega računa"
        value_template: >
          {{ "%.5f"|format(((states('sensor.prejsnja_cena_jederske_energije') | float(default=0)) + 
              (states('sensor.prejsnja_cena_pavsalnih_stroskov') | float(default=0)) - 
              (states('sensor.prejsnja_cena_e_popusta') | float(default=0))) / 2 ) }}
        unit_of_measurement: "EUR"
        unique_id: c8939c42-b97a-43e2-9e00-2edd8531de1d  

#============================================
# SKUPAJ IZRAČUN
#============================================
      prejsnji_skupaj_izracun_stroskov:
        friendly_name: "Prejšnji skupaj izračun stroškov"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_skupaj_izracun_stroska_elektricne_energije') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_dogovorjene_moci') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_prevzete_ee') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_trosarine') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 40627115-cd4c-4fd2-b54f-31ce2e8751a3
#============================================
# SKUPAJ IZRAČUN z DDV
#============================================
      prejsnji_skupaj_izracun_stroskov_ddv:
        friendly_name: "Prejšnji skupaj izračun stroškov z DDV"
        value_template: >
          {{ "%.5f"|format((
              (states('sensor.prejsnji_skupaj_izracun_stroska_elektricne_energije') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_dogovorjene_moci') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_prevzete_ee') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_trosarine') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna') | float(default=0))
              ) * 1.22) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 62e27f75-3ae1-4650-9c51-a3e4cbe03b24

```

In še če potrebujete `dogovorjena_moc_polovica.yaml`

```yaml
- platform: template
  sensors:
    moj_elektro_casovni_blok_1_polovica:
      friendly_name: "Moj Elektro Casovni Blok 1 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_1') | float }}"
      unique_id: 4525ad63-7144-4ba8-95f6-7aa649b3879c

- platform: template
  sensors:
    moj_elektro_casovni_blok_2_polovica:
      friendly_name: "Moj Elektro Casovni Blok 2 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_2') | float }}"
      unique_id: 4301a898-a54c-411c-ba1a-1759e8f0676d

- platform: template
  sensors:
    moj_elektro_casovni_blok_3_polovica:
      friendly_name: "Moj Elektro Casovni Blok 3 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_3') | float }}"
      unique_id: 38be6300-1de6-4a0c-a24e-b78abbcb6dfb

- platform: template
  sensors:
    moj_elektro_casovni_blok_4_polovica:
      friendly_name: "Moj Elektro Casovni Blok 4 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_4') | float }}"
      unique_id: 9b0c24d9-e872-479b-81a1-d2df008e2162

- platform: template
  sensors:
    moj_elektro_casovni_blok_5_polovica:
      friendly_name: "Moj Elektro Casovni Blok 5 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.prejsnja_dogovorjena_moc_casovni_blok_5') | float }}"
      unique_id: c17ef3b7-63bd-499a-a57d-08b870b11414
```

In še če potrebujete `elektrika_obracun_prejsnji_grm.yaml`

```yaml
#=========================================================================================================================================================
#                          Obračun elektrike za prejšnji mesec
#=========================================================================================================================================================
#============================================
# Izračun stroška električne energije
#============================================ 
  - platform: template
    sensors:
      prejsnji_grm_skupaj_izracun_stroska_elektricne_energije:
        friendly_name: "Prejšnji GRM skupaj izračun stroška električne energije"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_elektricne_energije_et') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: b946156c-785f-49ac-8c25-45d089a13dd6
    
 #=================  
  - platform: template
    sensors:
      prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok1:
        friendly_name: "Prejšnji GRM izračun stroška prevzete EE za časovni blok 1"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_1') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_blok1_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 5685005e-30ee-45b6-bf47-5e32a5c7f1a9

  - platform: template
    sensors:
      prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok2:
        friendly_name: "Prejšnji GRM izračun stroška prevzete EE za časovni blok 2"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_2') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_blok2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: db2fb892-3062-4775-9309-39b437edde22

  - platform: template
    sensors:
      prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok3:
        friendly_name: "Prejšnji GRM izračun stroška prevzete EE za časovni blok 3"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_3') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_blok3_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 1667bbaf-2589-488e-8159-242d484314d9

  - platform: template
    sensors:
      prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok4:
        friendly_name: "Prejšnji GRM izračun stroška prevzete EE za časovni blok 4"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_4') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_blok4_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: c3dcb931-34b7-48ad-807f-05d97f25127f

  - platform: template
    sensors:
      prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok5:
        friendly_name: "Prejšnji GRM izračun stroška prevzete EE za časovni blok 5"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prevzeta_ee_casovni_blok_5') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_blok5_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 8c3e7b3a-5380-4f7f-8659-b2134393e1cc
#===
# SKUPAJ
#===
  - platform: template
    sensors:   
      prejsnji_grm_skupaj_izracun_stroska_prevzete_ee:
        friendly_name: "Prejšnji GRM skupaj izračun stroška prevzete EE"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok1') | float(default=0)) + 
              (states('sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok2') | float(default=0)) + 
              (states('sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok3') | float(default=0)) + 
              (states('sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok4') | float(default=0)) + 
              (states('sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok5') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: f10a96bd-fe49-465a-b2ee-62f4f0709856
#============================================
# Izračun stroška prispevkov in ostalih dajatev
#============================================
      prejsnji_grm_izracun_stroska_prispevka_za_delovanje_operaterja_trga:
        friendly_name: "Prejšnji GRM izračun stroška prispevka za delovanje operaterja trga"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prispevka_za_delovanje_operaterja_trga') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: ebee4fdf-6f89-4461-b420-6f04796ac914

      prejsnji_grm_izracun_stroska_prispevka_za_energetsko_ucinkovitost:
        friendly_name: "Prejšnji GRM izračun stroška prispevka za energetsko učinkovitost"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_prispevka_za_energetsko_ucinkovitost') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 5226d28b-4e77-4d6e-b8ce-91f84e64d810
#===
# SKUPAJ
#===
      prejsnji_grm_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev:
        friendly_name: "Prejšnji GRM skupaj izračun stroška prispevkov in ostalih dajatev"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_grm_izracun_stroska_prispevka_za_delovanje_operaterja_trga') | float(default=0)) + 
              (states('sensor.prejsnji_grm_izracun_stroska_prispevka_za_energetsko_ucinkovitost') | float(default=0)) + 
              (states('sensor.prejsnji_izracun_stroska_prispevka_za_spte_in_ove') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: 16139429-8750-4629-92e7-db9320438145
#============================================
# Izračun stroška trošarine
#============================================
      prejsnji_grm_skupaj_izracun_stroska_trosarine:
        friendly_name: "Prejšnji GRM skupaj izračun stroška trošarine"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnja_cena_trosarine') | float(default=0)) * 
              (states('sensor.prejsnji_mesec_p1_meter_faza2_mesecno') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 0ab8f5d8-2dc5-4246-9ddd-1a3776b68e50
#============================================
# SKUPAJ IZRAČUN
#============================================
      prejsnji_grm_skupaj_izracun_stroskov:
        friendly_name: "Prejšnji GRM skupaj izračun stroškov"
        value_template: >
          {{ "%.5f"|format((states('sensor.prejsnji_grm_skupaj_izracun_stroska_elektricne_energije') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_dogovorjene_moci') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_prevzete_ee') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_trosarine') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 82f79bee-53dd-402b-961f-d90455651db8

#============================================
# SKUPAJ IZRAČUN Z DDV
#============================================
      prejsnji_grm_skupaj_izracun_stroskov_ddv:
        friendly_name: "Prejšnji GRM skupaj izračun stroškov z DDV"
        value_template: >
          {{ "%.5f"|format((
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_elektricne_energije') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_dogovorjene_moci') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_prevzete_ee') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev') | float(default=0)) + 
              (states('sensor.prejsnji_grm_skupaj_izracun_stroska_trosarine') | float(default=0)) + 
              (states('sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna') | float(default=0))
            ) * 1.22) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: 16c2e6f2-a39b-4c88-877a-8191276fa40d
```

Izgled kartice:
![20250404-Izračun porabe elektrike-PERČ](https://github.com/user-attachments/assets/f35d3438-9338-43bc-871a-63d77a8bc99d)



Še koda kartice:

```yaml
type: horizontal-stack
cards:
  - type: entities
    entities:
      - entity: sensor.prejsnja_cena_elektricne_energije_et
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_delovanje_operaterja_trga
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_energetsko_ucinkovitost
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_spte_in_ove
        icon: none
        name: .
      - entity: sensor.cena_trosarine
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: sensor.date
        card_mod:
          style: ":host {\n  color: rgb(78, 192, 245);\n  font-weight: bolder;\n  }\nhui-generic-entity-row {\n  border-bottom: 5px solid rgb(78, 192, 245);\n  width: 100%;\n  }\t\n"
    state_color: false
  - type: entities
    entities:
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_blok2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_blok3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_blok4_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_blok5_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_1
        icon: none
        name: .
      - entity: sensor.prejsnji_mesec_p1_meter_faza3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: sensor.time
        card_mod:
          style: |
            :host {
              color: rgb(78, 192, 245);
              font-weight: bolder;
              }
            hui-generic-entity-row {
              border-bottom: 5px solid rgb(78, 192, 245);
              width: 100%;
              }
    state_color: false
  - type: entities
    entities:
      - entity: sensor.prejsnji_skupaj_izracun_stroska_elektricne_energije
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok2
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok3
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok4
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok5
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok2
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok3
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok4
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_izracun_stroska_prevzete_ee_casovni_blok5
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_izracun_stroska_prispevka_za_delovanje_operaterja_trga
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_izracun_stroska_prispevka_za_energetsko_ucinkovitost
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_izracun_stroska_prispevka_za_spte_in_ove
      - entity: sensor.prejsnji_skupaj_izracun_stroska_trosarine
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: >-
          sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna
        card_mod:
          style: |
            :host {
              color: rgb(78, 192, 245);
              }
            hui-generic-entity-row {
              border-bottom: 5px solid rgb(78, 192, 245);
              width: 100%;
              }
      - entity: sensor.prejsnji_skupaj_izracun_stroskov
        icon: mdi:currency-eur
        card_mod:
          style: |
            :host {
              color: red;
              }
      - entity: sensor.prejsnji_skupaj_izracun_stroskov_ddv
        icon: mdi:currency-eur
        card_mod:
          style: |
            :host {
              color: red;
              font-weight: bold;
              }              
    state_color: false
title: Prejšnji mesec PERIČ
```

In še izpis za solastnike hiše:

![20250404-Izračun porabe elektrike-GRM](https://github.com/user-attachments/assets/129210af-556a-4426-af64-952bc5a1cba2)

Še koda kartice:

```yaml
type: horizontal-stack
cards:
  - type: entities
    entities:
      - entity: sensor.prejsnja_cena_elektricne_energije_et
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_dogovorjena_moc_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prevzeta_ee_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_delovanje_operaterja_trga
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_energetsko_ucinkovitost
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_cena_prispevka_za_spte_in_ove
        icon: none
        name: .
      - entity: sensor.cena_trosarine
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: sensor.date
        card_mod:
          style: ":host {\n  color: rgb(78, 192, 245);\n  font-weight: bolder;\n  }\nhui-generic-entity-row {\n  border-bottom: 5px solid rgb(78, 192, 245);\n  width: 100%;\n  }\t\n"
    state_color: false
  - type: entities
    entities:
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_blok2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_blok3_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_blok4_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_blok5_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnja_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
      - entity: sensor.prejsnji_mesec_p1_meter_faza2_mesecno
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: sensor.time
        card_mod:
          style: |
            :host {
              color: rgb(78, 192, 245);
              font-weight: bolder;
              }
            hui-generic-entity-row {
              border-bottom: 5px solid rgb(78, 192, 245);
              width: 100%;
              }
    state_color: false
  - type: entities
    entities:
      - entity: sensor.prejsnji_grm_skupaj_izracun_stroska_elektricne_energije
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok2
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok3
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok4
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_izracun_stroska_dogovorjene_moci_casovni_blok5
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok2
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok3
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok4
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: sensor.prejsnji_grm_izracun_stroska_prevzete_ee_casovni_blok5
        card_mod:
          style: |
            :host {
              color: purple;
              }
      - entity: >-
          sensor.prejsnji_grm_izracun_stroska_prispevka_za_delovanje_operaterja_trga
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: >-
          sensor.prejsnji_grm_izracun_stroska_prispevka_za_energetsko_ucinkovitost
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.prejsnji_izracun_stroska_prispevka_za_spte_in_ove
      - entity: sensor.prejsnji_grm_skupaj_izracun_stroska_trosarine
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: >-
          sensor.prejsnji_skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna
        card_mod:
          style: |
            :host {
              color: rgb(78, 192, 245);
              }
            hui-generic-entity-row {
              border-bottom: 5px solid rgb(78, 192, 245);
              width: 100%;
              }
      - entity: sensor.prejsnji_grm_skupaj_izracun_stroskov
        icon: mdi:currency-eur
        card_mod:
          style: |
            :host {
              color: red;
              }
      - entity: sensor.prejsnji_grm_skupaj_izracun_stroskov_ddv
        icon: mdi:currency-eur
        card_mod:
          style: |
            :host {
              color: red;
              font-weight: bold;
              }              
    state_color: false
title: Prejšnji mesec GRM
```


Za vse ki želite preveriti pravilnost izračuna je tu še delni izpis računa Elektro energije:

![202503_Stran_2](https://github.com/user-attachments/assets/04141fdb-af3d-421e-a373-01c8afff8f64)


# Malo pojasnitve in zmedenosti!

Ob prehodu iz meseca `februarja` v `marec` smo prešli iz `višje sezone` v `nižjo sezono` istočasno je prenehal veljati `blok 3` in je začel veljati `blok 5`, ker je začetek meseca obenem bil `dela proti dan` (sobota)!
![Časovni bloki](https://github.com/user-attachments/assets/3bfdcaac-2eb1-446b-bfa9-c1fb50fbfbcc)

Torej, če uporabim malo prirejeno SQL poizvedbo lahko ugotovim, da se podatki shranjujejo v podatkovno bazo `vsako uro`:

![20250316-SQL sensor tarife_p1_meter_faza3_mesecno_5-Na uro](https://github.com/user-attachments/assets/e39aca20-caa1-4fac-bff8-2a6901ca54bb)


![20250316-SQL sensor tarife_p1_meter_faza3_mesecno_3-Na uro](https://github.com/user-attachments/assets/d40f55be-0021-4ed8-aef3-bd4cb3199e45)

Kot se da opaziti je logično, da je zadnji zapis za prejšnji mesec `2025-03-01 00:00:00` ker so entitete `sensor.tarife_p1_meter_faza3_mesecno_3` in podobne mesečne komulative.

Iz meni neznanega razloga je za entiteto `sensor.tarife_p1_meter_faza3_mesecno_5` ravno tako prvi zapis `2025-03-01 00:00:00` kar ni logično, ker bi pričakoval, da bo prvi zapis zapisan `2025-03-01 01:00:00`.

Zaradi tega lahko pri izračuni pride do male napake:

![20250313-SQL query](https://github.com/user-attachments/assets/57a4bb7b-3457-4ed4-88b3-58a34fef19fe).

Da bi se izognil napakam pri izračunu sem v datoteki `elektrika_cenik_prejsnji.yaml` za `blok 5` postavil ceno 0 EUR pri izpisu pa ga ne prikažem.

Po raznih forumih sem iskal pomoč oziroma pojasnitev zakaj se to dogaja, a zaman.

Morda toliko, da preverite logiko podatkov:
![20250313-SQL query prejšnji mesec več entitet](https://github.com/user-attachments/assets/cc25b780-852b-45cf-b940-99985d16b916)



