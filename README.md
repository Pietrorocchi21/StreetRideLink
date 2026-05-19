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
}
