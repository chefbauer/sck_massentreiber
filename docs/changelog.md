# Changelog – SCK Massentreiber

## 2026-07-29

### 1. Ventil-Automatik bei System AUS (hardware.yaml)
- **Problem:** Ventil-Automatik (RV-Intervall) lief auch wenn System AUS war
- **Fix:** Bedingung `id(system_ein)` vor RV-Intervall-Logik eingefügt
- **Effekt:** System AUS → Automatik deaktiviert, nur manuelle Ventil-Steuerung funktioniert

### 2. ESPHome Template Warning (schwenker.yaml, Zeile ~1630)
- **Problem:** Warning bei `"Motor Start/Stop"` – `/` in template string
- **Fix:** `"Motor Start/Stop"` → `"Motor Start-Stop"`
- **Effekt:** Warning verschwunden

### 3. RV-Intervall nicht aktiv (global_rv_aktiv)
- **Problem:** RV-Intervall lief nicht obwohl Pumpe auf AUTO
- **Ursache:** RV-Schalter (`sw_rv_aktiv`) in Einstellungen → Kühler Tab war AUS
- **Lösung:** Schalter AN schalten → Intervall funktioniert
- **Hinweis:** `global_rv_aktiv` wird bereits mit `restore_value: yes` in NVS gespeichert

### 4. Slot Colors Mix – Fade-Richtung korrigiert (lights.yaml, Zeile 221)
- **Problem:** Am Ende jedes Farbsegments fadet die Farbe in die falsche Richtung
  - Beispiel Blau→Rot-Grenze: Fade war Rot→Blau statt Blau→Rot
- **Fix:** Blend-Richtung in 2. Fade-Zone umgekehrt:
  ```cpp
  // Vorher:
  float blend = (t - (1.0f - fade_t)) / fade_t;
  
  // Nachher:
  float blend = 1.0f - (t - (1.0f - fade_t)) / fade_t;
  ```
- **Effekt:** Farben faden korrekt ineinander am Segmentende