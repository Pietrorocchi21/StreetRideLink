# 🏍️ StreetRideLink

> **The ultimate open-source co-pilot for motorcyclists.**
> Sistema integrato di telemetria, sicurezza e social riding.

![Status](https://img.shields.io/badge/Status-In%20Development-orange)
![License](https://img.shields.io/badge/License-MIT-blue)
![Platform](https://img.shields.io/badge/Platform-ESP32%20%7C%20RPi-red)

---

## 📑 Indice
- [Descrizione](#descrizione)
- [Funzionalità Principali](#-funzionalità-principali)
- [Analisi Tecnica](#-analisi-tecnica)
- [Roadmap di Sviluppo](#-roadmap-di-sviluppo)
- [Hardware Requisito](#-hardware-requisito)

---

## 📝 Descrizione
**MotoSmart Hub** è un dispositivo progettato per essere montato sul manubrio di qualsiasi motocicletta. Trasforma i dati grezzi dei sensori inerziali in informazioni vitali per la sicurezza e il divertimento del pilota, offrendo un'interfaccia pulita e non distratta.

---

## 🚀 Funzionalità Principali

### 🛡️ Sicurezza & Protezione
- [x] **e-Call:** Chiamata d'emergenza automatica in caso di caduta.
- [x] **Antifurto GPS:** Notifiche push e tracciamento live se la moto viene spostata a quadro spento.
- [x] **Black Box:** Registrazione degli ultimi 10 minuti di telemetria in caso di incidente.

### 📊 Telemetria & Performance
| Funzione | Descrizione | Sensore Usato |
| :--- | :--- | :--- |
| **Lean Angle** | Visualizzazione angolo di piega in tempo reale | IMU (Giroscopio) |
| **Braking Analysis** | Analisi forza G e rilevamento vibrazioni ABS | IMU (Accelerometro) |
| **Ghost Riding** | Posizione dei compagni sulla mappa in tempo reale | GPS + Cloud |

### 🌐 Smart Connectivity
- **Meteo Dinamico:** Avvisi pioggia imminente basati sulla tua posizione GPS.
- **Modalità Notturna:** UI adattiva a basso contrasto per non abbagliare il pilota.
- **Overlay Messaggi:** Lettura TTS (Text-to-Speech) dei messaggi in arrivo.

---

## 🔬 Analisi Tecnica

Il sistema utilizza un algoritmo di **Sensor Fusion** per calcolare l'angolo di piega ed evitare i falsi positivi nelle frenate.

```cpp
// Esempio logica calcolo inclinazione (Filtro Complementare)
float calculateLeanAngle(float accAngle, float gyroRate, float dt) {
    float angle = 0.98 * (angle + gyroRate * dt) + 0.02 * accAngle;
    return angle;
}```

# 🏍️ MotoSmart Hub: The Intelligent Motorcycle Co-Pilot

> **Un ecosistema open-source per trasformare ogni moto in una smart-bike.**
> Integra navigazione, telemetria avanzata, sicurezza attiva e connettività social.

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Version](https://img.shields.io/badge/version-1.0.0--alpha-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## 🗺️ Roadmap di Sviluppo

Segui l'avanzamento del progetto attraverso le fasi di rilascio:

- [x] **Fase 1: Fondamenta**
  - [x] Selezione Microcontrollore (ESP32-S3).
  - [x] Algoritmo Sensor Fusion per calcolo angolo di piega.
- [ ] **Fase 2: Sicurezza & Telemetria**
  - [ ] Sviluppo analisi FFT per riconoscimento vibrazioni ABS.
  - [ ] Implementazione Buffer Circolare per Scatola Nera.
  - [ ] Logica di rilevamento caduta per e-Call.
- [ ] **Fase 3: Connettività & UI**
  - [ ] Integrazione API meteo dinamiche.
  - [ ] Modalità Notturna adattiva automatica.
  - [ ] Protocollo MQTT per il Ghost Riding di gruppo.

---

## 🛠️ Hardware

Il sistema è progettato per essere modulare. Di seguito i componenti necessari per il build standard:

| Componente | Modello Consigliato | Funzione |
| :--- | :--- | :--- |
| **Cervello (MCU)** | `ESP32-S3` | Elaborazione dati e gestione UI. |
| **Sensore Inerziale** | `Bosch BNO055` | IMU a 9 assi per piega e decelerazione. |
| **Modulo GPS** | `u-blox NEO-M8N` | Posizionamento e navigazione. |
| **Modulo Cellulare** | `SIM7600G-H (4G)` | Connettività cloud e chiamate d'emergenza. |
| **Display** | `4.3" IPS 1000 nits` | Output visivo IP67 antiriflesso. |
| **Storage** | `MicroSD 32GB` | Logging dati telemetrici (Black Box). |

---

## 🔬 Dettagli Tecnici

### Analisi della Frenata (Senza Centralina)
Il dispositivo monitora la decelerazione longitudinale ($G$) e analizza le vibrazioni ad alta frequenza per rilevare l'attivazione meccanica dell'ABS:

```cpp
// Esempio logica trigger ABS basata su soglia vibrazioni
if (longitudinalG < -0.8 && vibrationFrequency > ABS_THRESHOLD) {
    status.absTriggered = true;
    display.showAlert("ABS ACTIVE");
}
