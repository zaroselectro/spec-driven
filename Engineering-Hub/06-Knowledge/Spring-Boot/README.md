---
type: knowledge
topic: spring-boot
tags: [wissen, spring-boot]
---

# Wissen: Spring Boot

Hintergrund zu Spring Boot 3.x. **Verbindlich** ist ausschließlich [[02-Standards/Java/spring-boot-standards]].

## Die Idee dahinter

Spring kapselt Konfigurations- und Laufzeitarbeit: Das Framework löst Abhängigkeiten auf, bindet Konfiguration und stellt einen Laufzeitkontext bereit. Der Nutzen entsteht, wenn die Anwendung die Standardkonventionen nutzt – und die eigene Logik außerhalb der Framework-Typen hält.

## Konzepte in Kurzform

- **Dependency Injection über Konstruktor:** Jede Klasse bekommt, was sie braucht, sichtbar in der Signatur. Das macht Lesen und Testen einfach; Feldinjektion versteckt Abhängigkeiten.
- **`@ConfigurationProperties`:** Konfiguration bündeln, typsicher binden und validieren – statt verstreuter `@Value`-Stellen ohne Übersicht.
- **Profile:** Unterschiede zwischen Umgebungen in einer Datei abbilden (`local`, `test`, `prod`). Profile ergänzen Konfiguration, sie ersetzen keine Dokumentation.
- **Controller-Advice:** Ein zentraler Ort für Fehler der Schnittstelle – identische Fehlerformate über alle Endpunkte (siehe [[02-Standards/API/rest-standards]]).
- **Test-Slices:** `@WebMvcTest`, `@DataJpaTest` u. ä. laden nur den Teil des Kontexts, den der Test braucht – schneller und weniger fragil als immer den vollen Kontext.

## Typische Missverständnisse

| Missverständnis | Folge |
| --- | --- |
| „Das Framework prüft die Eingaben schon.“ | Nur was deklariert wird; unbekannte Felder und Grenzen bleiben unprüft |
| „Eine Transaktion über den ganzen Vorgang ist sicherer.“ | Lange Sperren, schlechtes Verhalten über Netzgrenzen hinweg |
| „Ein leerer `catch` macht den Dienst stabiler.“ | Fehler werden unsichtbar; Stabilität täuschend |
| „Profil `prod` enthält die geheimen Werte.“ | Geheimnisse gehören in die Umgebung, nicht in die Repository-Datei |

## Iteration und Beobachtbarkeit

Actuator liefert Gesundheit und Metriken; ohne sie ist der Betriebszustand ratebasiert. Strukturierte Logs plus Kontext-ID machen Vorgänge über Services hinweg nachvollziehbar (siehe [[02-Standards/General/clean-code]]).

## Einstieg in dieses Vault

Standard: [[02-Standards/Java/spring-boot-standards]] · Testen: [[02-Standards/Testing/testing-standards]] · Betrieb: [[02-Standards/Infrastructure/kubernetes-standards]]

**Siehe auch:** [[06-Knowledge/README]] · [[06-Knowledge/Java/README]]
