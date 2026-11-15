# Bitwarden/Vaultwarden-Synchronisierung über WLAN

> **Status:** ✅ Problem gelöst
> **Datum:** 15. November 2026
> **Betroffene Komponenten:** Bitwarden Android-App, Vaultwarden (Docker), FRITZ!Box 6670 Cable, IPv6
> **Offizielles Issue:** [bitwarden/android#7618](https://github.com/bitwarden/android/issues/7618)

---

## 1. Fehlerbeschreibung

Die Bitwarden-App auf dem Samsung-Smartphone konnte seit kurzem nicht mehr mit dem selbst gehosteten Vaultwarden-Docker-Dienst synchronisieren.

### Verhalten

| Netzwerk | Ergebnis |
|----------|----------|
| Mobilfunk | ✅ Synchronisierung funktionierte problemlos |
| WLAN | ❌ Verbindungsfehler bzw. Timeout |
| Tablet im selben WLAN | ✅ Funktionierte problemlos |
| Vaultwarden & FRITZ!Box Erreichbarkeit | ✅ Grundsätzlich erreichbar |

---

## 2. Ursache

Eine **fehlende Android-App-Berechtigung** nach einem Android-Update.

Bitwarden benötigte die Berechtigung **„Geräte in der Nähe“** (Nearby devices) für den Netzwerkzugriff in der betroffenen IPv6-Konstellation.

Ohne diese Berechtigung wurde die Verbindung über WLAN blockiert. Die Verbindung über Mobilfunk war davon nicht betroffen.

---

## 3. Lösung

Auf dem Samsung-Smartphone:

1. **Einstellungen** öffnen
2. **Apps → Bitwarden**
3. **Berechtigungen** auswählen
4. **„Geräte in der Nähe“** erlauben
5. Bitwarden neu starten und Synchronisierung testen

✅ **Synchronisierung wiederhergestellt**

---

## 4. Auswirkungen auf die Serverinfrastruktur

Es waren **keinerlei Änderungen** erforderlich an:

- Vaultwarden-Docker-Container
- Reverse Proxy
- DNS bzw. DDNS
- FRITZ!Box 6670 Cable
- IPv6-Konfiguration
- SSL-Zertifikaten

---

## 5. Erkenntnis für zukünftige Fehlersuche

Bei Problemen mit selbst gehosteten Diensten unter Android, insbesondere wenn:

- Mobilfunk funktioniert, WLAN aber nicht,
- andere Geräte im selben WLAN funktionieren,
- Verbindungsfehler oder Timeouts auftreten,

sollte **zuerst die App-Berechtigung „Geräte in der Nähe“** überprüft werden.

---

## 6. Offizielles Issue

Das Problem wurde als Feature Request im offiziellen Bitwarden-Android-Repo dokumentiert:

📌 **[bitwarden/android#7618](https://github.com/bitwarden/android/issues/7618)** – *Add warning/check for missing 'Nearby devices' permission that silently blocks WiFi connectivity*

---

## Fazit

Kein Server- oder Routerproblem, sondern eine fehlende Android-Berechtigung für Bitwarden.
