# Erweiterung: Spring Security

## Zweck und Geltungsbereich

ID: `spring-security`. Gilt ausschließlich bei ausdrücklicher Aktivierung für Authentifizierung und Autorisierung mit Spring Security. Diese Fassung beschreibt Servlet-basierte HTTP-Anwendungen und bei Bedarf Method Security in Services.

Sie schreibt weder einen Identity Provider noch JWT, lokale Benutzerverwaltung oder eine Datenbank vor. Reaktive Anwendungen mit WebFlux benötigen ergänzende, ausdrücklich dokumentierte Projektregeln für den reaktiven Security-Stack; die Servlet-Konfiguration darf dort nicht unverändert übernommen werden. Ein eigener Authorization Server ist nicht Bestandteil dieser Erweiterung.

## Voraussetzungen

- Basis-Specs.
- Bei einer eingehenden HTTP-API zusätzlich `http-api` ausdrücklich aktivieren und übernehmen. Bei ausschließlicher Method Security besteht diese Voraussetzung nicht.
- `persistence-jpa` wird nur benötigt, wenn das Projekt tatsächlich eigene relationale Benutzer- oder Berechtigungsdaten mit JPA speichert; dann ebenfalls ausdrücklich aktivieren.
- Keine weiteren Erweiterungen. Eine bestehende Vorgabe „ohne Authentifizierung“ für zu schützende Bereiche muss vor der Implementierung in der Projekt-Spec aufgelöst werden.

## Offene Projektentscheidungen

In `specs/project.md` vor der betroffenen Implementierung festlegen:

| Entscheidung | Erforderliche Angaben |
| --- | --- |
| Geltungsbereich | Geschützte APIs beziehungsweise Service-Operationen, Servlet-Stack oder ausdrücklich ergänzter reaktiver Stack |
| Authentifizierung | Zum Beispiel OIDC-Login, OAuth2 Resource Server mit JWT oder opaque Tokens, oder lokale Anmeldung; keine automatische Auswahl |
| Identitätsquelle | Vertrauenswürdiger Provider, erforderliche Issuer/Audiences beziehungsweise Benutzerquelle und extern zugeführte Konfiguration |
| Berechtigungen | Rollen/Authorities/Scopes, deren Mapping sowie eine Zugriffsmatrix pro Operation; gegebenenfalls Eigentümer- und Mandantengrenzen |
| Öffentliche Zugriffe | Ausdrücklich freigegebene Routen und Methoden, einschließlich gegebenenfalls Login, Health und API-Dokumentation |
| Sitzungen | Zustandslos oder Session-basiert; bei Sessions Laufzeit, Cookie-Einstellungen und Logout-Verhalten |
| Browserzugriffe | Benötigte CORS-Ursprünge, Methoden und Header; CSRF-Konzept und begründete Ausnahmen |
| Fehlerverhalten | API-Antworten für fehlende/ungültige Anmeldung und fehlende Berechtigung; bei Browser-Login zulässige Redirects |
| Betrieb | TLS-Terminierung, vertrauenswürdige Proxy-Konfiguration und Konfigurationsquellen für Schlüssel oder Secrets |

Nicht benötigte Optionen ausdrücklich als nicht zutreffend kennzeichnen. Keine Demo-Benutzer, öffentlichen Endpunkte oder Standardrollen zur Überbrückung offener Entscheidungen erfinden.

## Technische Regeln

### Abhängigkeiten und Authentifizierung

- Verwende `spring-boot-starter-security` und `spring-security-test` im Test-Scope. Ergänze abhängig vom gewählten Verfahren den OAuth2-Client- oder Resource-Server-Starter. Versionsverwaltung erfolgt gemäß Basis; keine unabhängige Aktualisierung einzelner Security-Komponenten ohne dokumentierte Kompatibilitätsprüfung.
- Verwende die eingebauten Authentifizierungsmechanismen und geeignete Erweiterungspunkte. Implementiere keine eigene Tokenprüfung oder Passwortkryptografie, wenn Spring Security das gewählte Verfahren unterstützt.
- Bei JWTs prüfe Signatur, Gültigkeitszeitraum, vertrauenswürdigen Issuer und die für dieses Projekt erlaubte Audience. Konfiguriere Audience-Prüfung ausdrücklich; bloßes Decodieren genügt nicht. Übernimm keine beliebigen Issuer oder Schlüsselquellen aus ungeprüften Token-Inhalten.
- Bei opaque Tokens erfolgt die Prüfung über die konfigurierte vertrauenswürdige Introspection-Anbindung. Ein Provider-Ausfall darf keine erfolgreiche Authentifizierung oder zusätzliche Berechtigungen erzeugen.
- Falls lokale Passwörter benötigt werden, speichere ausschließlich adaptive Passwort-Hashes über einen geeigneten `PasswordEncoder`. Keine Klartextpasswörter, `NoOpPasswordEncoder` oder produktiven Demo-Zugangsdaten.
- Schlüssel, Client-Secrets und Passwörter werden extern zugeführt. Tokens, Cookies und Zugangsdaten erscheinen weder in Logs noch in API-Fehlerantworten. Authentifizierte Zugriffe außerhalb lokaler Tests erfolgen über HTTPS.

### HTTP-Schutz und Browserzugriffe

- Definiere die Servlet-Konfiguration als `SecurityFilterChain`-Beans mit der zur verwalteten Version passenden Lambda-Konfiguration. Verwende keinen `WebSecurityConfigurerAdapter`.
- Setze die Zugriffsmatrix ausdrücklich um. Nicht zugeordnete Requests werden standardmäßig abgewiesen. Bei mehreren Filterketten definiere Reihenfolge und Matcher sowie eine abschließende Kette, damit keine ungeschützten Lücken entstehen.
- Öffentliche Endpunkte werden gezielt freigegeben und bleiben möglichst innerhalb der Security-Filterkette. Deaktiviere nicht pauschal den Schutz für ganze Anwendungen oder alle Management-Endpunkte.
- Aktiviere nur das gewählte Anmeldeverfahren. Eine reine Bearer-API erhält keinen unbeabsichtigten Formular-Login und keine Login-Redirects. Verwende dort zustandslose Sitzungsverwaltung, sofern dies der Projektentscheidung entspricht.
- Behalte CSRF-Schutz bei Browserzugriffen mit automatisch übertragenen Zugangsdaten, insbesondere Session-Cookies, bei. „REST“ oder „stateless“ allein rechtfertigt keine Abschaltung. Für ausschließlich per Authorization-Header authentifizierte Bearer-Endpunkte ohne Cookie- oder andere automatisch übertragene Authentifizierung kann CSRF nach dokumentierter Prüfung gezielt ausgenommen werden.
- CORS wird nur bei Bedarf eingerichtet. Erlaube ausschließlich die benötigten Ursprünge, Methoden und Header; keine pauschale Freigabe mit Credentials. Prüfe Preflight-Verarbeitung getrennt von Authentifizierung und Autorisierung.
- Bei Sessions definiere sichere Cookie-Einstellungen, Ablauf und Logout. Behalte den Schutz gegen Session Fixation und die Security-Header bei; projektspezifische Änderungen werden begründet.

## Architektur

- Gemeinsame technische Security-Konfiguration liegt unter `<basispaket>.configuration.security`. Dieser Bereich verdrahtet Komponenten; er enthält keine fachlichen Anwendungsfälle.
- HTTP-spezifische Fehlerhandler liegen in der API-Schicht, beispielsweise `<fachmodul>.api.security`. Konfiguriere `AuthenticationEntryPoint` und `AccessDeniedHandler` für Fehler aus der Filterkette; verlasse dich dafür nicht allein auf Controller-Exception-Handler. Verwende das in der HTTP-API festgelegte Fehlerformat, soweit das Authentifizierungsprotokoll dies zulässt, und erhalte erforderliche Protokoll-Header.
- Unterscheide bei APIs fehlende oder ungültige Authentifizierung (401) von fehlender Berechtigung eines authentifizierten Aufrufers (403). Browser-Login darf die dokumentierten Redirects verwenden. Andere Schutzfehler wie CSRF werden nach ihrem eigenen Vertrag behandelt.
- Fachliche Services prüfen bei Bedarf Eigentümer-, Mandanten- und objektbezogene Berechtigungen. Eine passende Rolle allein erlaubt nicht den Zugriff auf beliebige fremde Objekte.
- Benötigt die Fachlichkeit Angaben zum Aufrufer, übergib fachliche, unveränderliche DTOs beziehungsweise einfache Werttypen aus einer vertrauenswürdigen Grenze. Spring-Security-Typen wie `Authentication`, `Jwt` oder `UserDetails` sind keine fachlichen Port-, Contract- oder Service-Parameter. Übernimm Identität und Rechte nicht ungeprüft aus Request-Feldern.
- Bei benötigter Method Security aktiviere sie ausdrücklich mit `@EnableMethodSecurity` und sichere die tatsächlichen Service-Einstiegspunkte. Security-Annotationen wie `@PreAuthorize` sind dort zulässig; vermeide sonstige direkte Zugriffe des Business-Codes auf den `SecurityContextHolder`. Beachte beim Entwurf, dass interne Selbstaufrufe die Proxy-Prüfung nicht auslösen.
- Eigene fachliche Anbindungen an Identitäts- oder Berechtigungssysteme folgen den Port-/Adapter-Regeln der Basis. Framework-interne OIDC-, JWKS- oder Introspection-Aufrufe werden über Spring Security konfiguriert; erzeuge dafür keinen künstlichen Business-Port.
- Converter einer externen Identitätsanbindung liegen im jeweiligen Adapter. Selbst definierte DTOs behalten die Ablageorte und Zugriffsgrenzen der Basis. Security-Konfiguration greift nicht direkt auf Persistenzentitäten zu.

## Tests

Die Unit-Test- und Maven-Regeln der Basis gelten. Zusätzliche Tests richten sich nach den tatsächlich ausgewählten Verfahren; ungenutzte Anmeldeverfahren werden nicht implementiert.

- Prüfe eigene Logik wie Authority-Mapping, Berechtigungsentscheidungen und Fehlerübersetzung isoliert. Rein deklarative Konfiguration benötigt keine künstlichen Unit-Tests; ihre Wirkung wird durch Integrationstests geprüft.
- Bei HTTP verwende die tatsächliche Security-Filterkette. Prüfe repräsentativ jede unterschiedliche Regel der Zugriffsmatrix: berechtigter Zugriff, anonymer Zugriff, authentifizierter Zugriff ohne Berechtigung und ausdrücklich öffentliche Zugriffe. Prüfe auch die abweisende Auffangregel und bei mehreren Ketten deren Abgrenzung.
- Prüfe Status, Fehlerformat und erforderliche Authentifizierungs-Header. Deaktiviere Security-Filter nicht, um diese Tests erfolgreich zu machen. Bestehende erfolgreiche HTTP-Integrationstests müssen mit einer passend autorisierten Identität laufen.
- Bei JWTs ergänze Tests mit lokal signierten Tokens und tatsächlicher Decoder-/Validator-Konfiguration: gültiges Token, falsche Signatur, abgelaufenes Token sowie falscher Issuer und falsche Audience. Gemockte JWT-Identitäten oder ein gemockter Decoder belegen keine Tokenvalidierung.
- OIDC-, JWKS- und Introspection-Aufrufe werden bei entsprechenden Integrationstests gegen lokale Protokoll-Stubs mit dynamischen Ports geprüft. Teste relevante Ablehnungen und Ausfälle. Ein echter Identity Provider, reale Konten und produktive Secrets sind nicht erforderlich.
- Bei CSRF-Schutz prüfe zustandsändernde Requests mit gültigem sowie fehlendem oder ungültigem CSRF-Token; verwende dabei eine ansonsten berechtigte Identität. Bei CORS prüfe erlaubte und nicht erlaubte Origins einschließlich Preflight.
- Bei Session-Login prüfe Anmeldung, Sitzungsnutzung und Logout. Bei einer als zustandslos spezifizierten API prüfe, dass keine Authentifizierung aus einer HTTP-Session übernommen oder darin gespeichert wird.
- Bei Method Security prüfe erlaubte und abgewiesene Aufrufe über den tatsächlichen Spring-Proxy. Bei Eigentümer- oder Mandantenregeln ergänze den Zugriff auf fremde Objekte als Negativfall.

## Abnahmekriterien

- Alle für das gewählte Verfahren notwendigen Projektentscheidungen sind dokumentiert und implementiert.
- Öffentliche Zugriffe sind ausdrücklich begrenzt; geschützte Operationen setzen die Zugriffsmatrix und gegebenenfalls Objektgrenzen durch.
- Authentifizierungsprüfung, Session-/CSRF-/CORS-Verhalten und Fehlerantworten entsprechen den Projektentscheidungen.
- Die ausgewählten Security-Integrationstests laufen zusammen mit den bestehenden Tests erfolgreich im Maven-Lifecycle der Basis, ohne echte Fremdsysteme.
- Es gibt keine hartcodierten produktiven Zugangsdaten oder pauschal deaktivierten Sicherheitsprüfungen zur Umgehung fehlender Konfiguration.

## Abweichungen von der Basis

Die allgemeine Port-Regel in `specs/architecture.md`, Abschnitt „Externe Systeme und Datenbanken: Ports und Adapter“, wird für rein technische, durch Spring Security ausgeführte OIDC-, JWKS- und Introspection-Aufrufe präzisiert: Diese benötigen keinen fachlichen Port. Die dortigen und die allgemeinen Integrationstestregeln werden für diese Aufrufe durch die oben beschriebenene Framework-Konfiguration und lokale Protokolltests erfüllt. Eigene fachliche Integrationen bleiben unverändert Port-basiert.

## Referenzen

Bei Projekterstellung die zur vom Spring-Boot-Parent verwalteten Security-Version passende Dokumentation verwenden. Die folgenden Referenzen beziehen sich auf die 6.5-Dokumentation, nicht auf eine zusätzlich festzuschreibende Dependency-Version.

- [Servlet-Konfiguration und Filterketten](https://docs.spring.io/spring-security/reference/6.5/servlet/configuration/java.html)
- [CSRF-Schutz](https://docs.spring.io/spring-security/reference/6.5/servlet/exploits/csrf.html)
- [JWT Resource Server und Validierung](https://docs.spring.io/spring-security/reference/6.5/servlet/oauth2/resource-server/jwt.html)
- [OAuth2-Testunterstützung](https://docs.spring.io/spring-security/reference/6.5/servlet/test/mockmvc/oauth2.html)
- [Method Security](https://docs.spring.io/spring-security/reference/6.5/servlet/authorization/method-security.html)
- [PasswordEncoder](https://docs.spring.io/spring-security/reference/6.5/servlet/authentication/passwords/password-encoder.html)
