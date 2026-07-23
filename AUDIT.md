# Audit — aktionstracker.de (Stand: 2026-07-23, korrigierte Version)

## ⚠️ Korrektur-Hinweis

Die erste Version dieses Audits basierte versehentlich auf einem **22 Commits
veralteten lokalen Checkout** (letzter lokaler Commit: 17. Juni 2026,
`origin/main` aber bereits bei einem Commit vom 19. Juli 2026). In der
Zwischenzeit war die Seite offenbar schon in einer anderen Session/Maschine
umfangreich überarbeitet worden — neue Preise, neue Sektionen, echte
Screenshots, Performance-/A11y-Fixes, CTA-Hierarchie. Das führte zu falschen
Aussagen (u. a. „alte Preise", „keine Screenshots vorhanden", „3-CTA-Hero").

**Root Cause:** Mein lokaler Ordner `aktionstracker-website/` war seit
Juni nicht mehr gepullt worden. Behoben durch `git pull origin main
--ff-only` (sauberer Fast-Forward, keine Konflikte, nichts überschrieben).

Diese Version ist gegen den **aktuellen, korrekten Stand** neu erstellt:
lokal `index.html` nach dem Pull (3300 Zeilen) **und** direkt gegen die
Live-Seite `https://aktionstracker.de` verifiziert (Preise, Lighthouse).

---

## 1. Techstack, Dateistruktur, Build-Prozess

- Statisches HTML/CSS/JS, **eine Datei** `index.html` (3300 Zeilen), kein
  Build-Schritt, kein `package.json`, kein `netlify.toml`.
- Netlify-Hosting bestätigt über `data-netlify` im Kontaktformular —
  Netlify Forms fängt das Formular serverseitig ab.
- Externe Assets: Bootstrap 5.3.2 CSS/JS + Bootstrap Icons 1.11.3 über
  jsDelivr, **neu seit dem letzten Audit:** Google Fonts (Bitter + Public
  Sans) über `fonts.googleapis.com`/`fonts.gstatic.com` mit `preconnect`.
- Repo enthält jetzt zusätzlich einen `img/`-Ordner mit 10 Dateien: 9 echte
  App-Screenshots (`ma-*.webp`, `vkl-*.webp`) + `og-image.jpg`, sowie
  `llms.txt` (neu, für KI-Suchmaschinen-Crawler).
- Sitemap enthält weiterhin nur die eine URL.
- Zwei `<script type="application/ld+json">`-Blöcke im `<head>`: ein
  vollständiges `FAQPage`-Schema (29 Fragen, identisch zum sichtbaren FAQ)
  und ein `SoftwareApplication`-Schema mit den drei Preis-Offers.

⚠️ Weiterhin nicht von hier aus verifizierbar: die tatsächliche
Netlify-Projektkonfiguration (liegt im Netlify-Dashboard).

---

## 2. Vollständige Sektionsliste (in Reihenfolge, aktueller Stand)

| # | Sektion | Zeilen | Kurzbeschreibung |
|---|---|---|---|
| 1 | Navbar | 1158–1188 | Sticky, 5 Anker-Links + „Demo testen" (extern) + „Kontakt"-Button |
| 2 | Hero | 1190–1414 | H1, **ein** primärer CTA („Demo öffnen", teal), zwei klar untergeordnete Textlinks, Zwei-Handy-Animation (Mitarbeiter trägt ein → VKL sieht live) |
| 3 | Trust-Bar | 1416–1426 | 4 Vertrauens-Badges |
| 4 | Demo-Teaser | 1428–1494 | Eigene Sektion, führt zur Live-Demo |
| 5 | Problem + Karussell | 1495–1774 | 3 „Chaos"-Karten + Screenshot-Karussell (2 Tabs: Mitarbeiter/VKL, je 4–5 echte Screenshots mit Lightbox-Zoom) |
| 6 | Früh-Kontakt | 1775–1837 | Zweite CTA-Gelegenheit vor den Features |
| 7 | Features | 1839–1966 | 9 Feature-Karten, auf Mobile aufklappbar |
| 8 | Zielgruppe | 1968–2013 | 4 Zielgruppen-Items |
| 9 | Wie es funktioniert | 2016–2070 | 3 Schritte |
| 10 | Stats | 2072–2102 | 4 Kennzahlen-Kacheln |
| 11 | Preise | 2105–2436 | 3 Pakete + „Individuell" + Billing-Toggle + Preisvergleich + einklappbare Add-ons |
| 12 | Über mich | 2437–2466 | Neu: Herkunfts-/Motivations-Story von Jan Anschütz |
| 13 | FAQ | 2468–3021 | 5 Kategorien, 29 Fragen |
| 14 | CTA + Kontakt | 3022–3092 | Checkliste + Netlify-Formular |
| 15 | Lightbox | 3093–3123 | Screenshot-Vollbildansicht mit Pfeil-Navigation |
| 16 | Footer | 3124–3146 | Logo, Kontakt-Link, Datenschutz-/Impressum-Modal-Trigger |
| 17 | Modal: Impressum | 3148–3174 | § 5 TMG, jetzt inkl. OS-Streitbeilegungs-Hinweis (§ 36 VSBG) |
| 18 | Modal: Datenschutz | 3176–3204 | Art. 13 DSGVO |

---

## 3. Widerspruchstabelle (korrigiert)

### Real und aktuell bestätigt

| Thema | Fundstellen | Wortlaut |
|---|---|---|
| **Time-to-live (4 verschiedene Aussagen)** | Z. 1226 (Hero) | „1 Woche bis live" |
| | Z. 2021 (Wie-Sektion) | „Ihr Team kann **innerhalb eines Tages** loslegen" |
| | Z. 2090 (Stats) | „**< 1 Tag** bis zur Produktivität" |
| | Z. 247/2942 (FAQ, JSON-LD + sichtbar identisch) | „In der Regel **5–7 Werktage** nach Vertragsabschluss" |
| **Zielgruppen-Untergrenze** | Z. 1999 (Zielgruppe-Sektion) | „Teams **ab 3** Außendienst-Mitarbeitern" |
| | Z. 79 (FAQ, JSON-LD + sichtbar identisch) | „**Auch mit 1–2** Außendienstlern lohnt sich der Aktions Tracker bereits" |

### Bereits gelöst — in der ersten Audit-Version fälschlich als offen markiert

- **Preise:** Website zeigt jetzt durchgängig **129 € / 219 € / 319 €** —
  identisch zu deiner Preiserhöhungs-Entscheidung. Konsistent in Hero,
  Preiskarten, Preisvergleichs-Hinweis, FAQ-Text und beiden JSON-LD-Schemas.
  ✅ Kein Widerspruch mehr.
- **Laufzeit/Kündigung:** Jetzt sauber nach Zahlweise differenziert — die
  Kündigungs-Badges (Z. 2131/2134) wechseln passend zum Billing-Toggle
  zwischen „Keine Mindestlaufzeit" (monatlich) und „12 Monate
  Mindestlaufzeit" (jährlich), FAQ sagt exakt dasselbe. ✅ Kein Widerspruch
  mehr.
- **Hero-CTA-Hierarchie:** Jetzt ein einziger primärer CTA („Demo öffnen",
  teal, groß) + zwei klar untergeordnete Textlinks („Gespräch anfragen",
  „Preise ansehen"). Kein drittes gleichrangiges CTA mehr. ✅ Bereits
  umgesetzt, entspricht genau der in Schritt 2.2 vorgeschlagenen Lösung.
- **Screenshots:** Es gibt jetzt echte App-Screenshots (`img/ma-*.webp`,
  `img/vkl-*.webp`, 9 Stück, alle mit beschreibenden Alt-Texten) in einem
  Tab-Karussell mit Lightbox-Zoom. ✅ Meine vorherige Aussage „keine
  Screenshots vorhanden" war falsch (stale Daten).

### Weiterhin offen (aus dem ursprünglichen Auftrag)

- **§ 19 UStG** steht weiterhin nur in der Preise-Fußnote (Z. 2430) und im
  Impressum-Modal (Z. 3165) — nicht prominent. Falls das für dich schon
  „erledigt" zählt (weil nicht mehr blickfangend), sag Bescheid, sonst siehe
  Rückfragen unten.
- **Vertrauen in den Einzelunternehmer (Ausfallrisiko):** Die neue
  „Über mich"-Sektion (Z. 2437–2466) erzählt Jans Hintergrundgeschichte
  (Motivation, Erfahrung), beantwortet aber **nicht** die Frage „Was
  passiert, wenn Sie ausfallen oder aufhören?" — ich habe im ganzen
  Dokument keine Textstelle dazu gefunden. Bleibt ein offener Punkt aus
  Schritt 2.3.
- **Consent-Banner für GA4:** weiterhin keiner vorhanden — GA4 lädt
  unbedingt im `<head>` (Z. 7–13), kein Opt-in-Gate.
- **Impressum/Datenschutz:** weiterhin nur Bootstrap-Modals mit `href="#"`,
  keine echten crawlbaren Unterseiten.

---

## 4. Asset-Realitätscheck (korrigiert)

✅ **9 echte Screenshots vorhanden** unter `img/`, alle mit sinnvollem
Alt-Text, eingebunden in ein Tab-Karussell (Mitarbeiter-Ansicht / VKL-Ansicht)
mit Klick-zum-Zoom (Lightbox). Kein kaputtes `<img>` gefunden — alle 9
`<img>`-Tags plus das dynamische Lightbox-`<img id="lb-img">` haben eine
gültige `src` bzw. werden per JS befüllt.

---

## 5. Lighthouse-Baseline (Live-Messung, `https://aktionstracker.de`, Mobile)

Diese Messung war bereits korrekt in der ersten Audit-Version — sie lief
gegen die echte Live-URL, nicht gegen mein lokales (stale) Dateisystem.

| Kategorie | Score |
|---|---|
| Performance | **55** |
| Accessibility | **82** |
| Best Practices | **96** |
| SEO | **100** |

**Core Web Vitals (Mobile, simuliert):**

| Metrik | Wert |
|---|---|
| First Contentful Paint | 4,4 s |
| Largest Contentful Paint | 5,5 s |
| Total Blocking Time | 470 ms |
| Cumulative Layout Shift | 0 |
| Speed Index | 5,5 s |

**Top-Accessibility-Fehler (0-Score-Audits):**
- `button-name` — Buttons ohne zugänglichen Namen
- `color-contrast` — siehe Tabelle unten
- `heading-order` — Überschriften-Hierarchie nicht durchgängig absteigend
- `landmark-one-main` — kein `<main>`-Element
- `target-size` — mehrere Touch-Targets unter Mindestgröße

Performance-Ziel aus Schritt 4 (≥ 95) ist mit 55 weit entfernt. Mit den
jetzt zusätzlichen Google-Fonts-Requests (2× `preconnect` + 1× CSS) kommt
noch eine dritte externe Render-Abhängigkeit neben den zwei jsDelivr-CSS/JS
dazu — das dürfte die Lücke eher vergrößert als verkleinert haben.

---

## 6. Farbkontrast — gemessen gegen den aktuellen Stand (WCAG 2.1)

AA-Schwellen: 4,5:1 normaler Text, 3:1 großer Text (≥ 18,66 px fett /
≥ 24 px normal) und UI-Komponenten.

| Ratio | Bewertung | Kombination |
|---|---|---|
| 3,06:1 | Nur Large-Text ✅ (Button ist groß+fett) | `.btn-amber`: weiß auf `#c8860a` |
| 5,17:1 | ✅ | **Neu:** `.btn-cta` (Haupt-CTA „Demo öffnen"): weiß auf `--teal #1c7a6e` |
| 15,96:1 | ✅ | Body-Fließtext: `--ink #16232f` auf `#fff` |
| 8,42:1 | ✅ | Navbar-Link auf `#1a3a5c` |
| 7,46:1 | ✅ | `.section-sub`: `#555` auf `#fff` |
| 5,20:1 | ✅ | `.stat-label` (Problem-Sektion) |
| 5,28:1 | ✅ | **Verbessert:** `.trust-item` jetzt `rgba(255,255,255,.5)` (vorher .42) — besteht jetzt auch für Normaltext |
| 5,85:1 | ✅ | Footer-Text |
| **2,85:1** | **❌ Fehler** | `.price-note`: `#999` auf `#fff` |
| 3,54:1 | Nur Large-Text ⚠️ | `.price-name`: `#888` — Schrift ist klein (.82rem) → **verfehlt AA** |
| 3,06:1 | Nur Large-Text ⚠️ | `.section-label` (Amber) auf `#fff` — klein (.78rem) → **verfehlt AA** |
| **2,84:1** | **❌ Fehler** | `.section-label` (Amber) auf `--hell` |
| **2,44:1** | **❌ Fehler** | `.stat-shock.stat-muted .stat-num` |
| 3,92:1 | Nur Large-Text ⚠️ | `.step-col p` — klein (.82rem) → **verfehlt AA** |
| 4,98:1 | ✅ | **Verbessert:** `.email-meta`/`.email-thread` jetzt `rgba(255,255,255,.5)` (vorher .4/.28) |

**Zusammenfassung:** Von den 9 Verstößen aus der ersten Prüfung sind **3
bereits behoben** (`.trust-item`, `.email-meta`, `.email-thread` — jemand
hat offenbar zwischenzeitlich genau daran gearbeitet, siehe Commit
„Web-Quality-Audit: Performance, A11y, SEO Fixes"). **5 bestehen weiter:**
`.price-note`, `.price-name`, `.section-label` (2 Varianten),
`.stat-shock.stat-muted .stat-num`. Das neue `.btn-cta` (Haupt-CTA) ist mit
5,17:1 unproblematisch.

---

## 7. Kaputte Assets

Keine gefunden. Alle 9 Screenshot-`<img>`-Tags plus das Lightbox-Bild sind
funktional korrekt eingebunden.

---

## 8. Sonstige Beobachtungen

- Aus der Git-Historie ist ersichtlich, dass zwischen dem alten und dem
  aktuellen Stand bereits gearbeitet wurde an: eigenständiger Typografie
  (Bitter/Public Sans statt Segoe UI — **das in Schritt 3 „Verboten"-Ziel
  ist also schon erreicht**), CTA-Hierarchie, SEO/KI-Suche (`llms.txt`,
  JSON-LD-Schemas), Scroll-Tracking, Haptik-Fixes (iOS-Zoom-Bug,
  Touch-Targets), einem eigenen „Web-Quality-Audit"-Commit. Ein Teil der
  in deinem Auftrag beschriebenen Arbeit ist damit **bereits erledigt**,
  bevor ich überhaupt angefangen habe.
- GA4 lädt weiterhin unbedingt ohne Consent-Gate — Schritt-4-Punkt bleibt
  in vollem Umfang offen.
- Kein Honeypot/Spam-Schutz am Kontaktformular über die Basis-Netlify-
  Forms-Integration hinaus (nicht explizit angefragt, aber erwähnenswert).
