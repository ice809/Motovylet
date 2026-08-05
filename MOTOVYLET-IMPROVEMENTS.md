# Motovylet.cz – Struktura & Doporučení na Vylepšení

## 📊 Analýza Současné Struktury

### Co funguje výborně ✅

| Aspekt | Stav | Poznámka |
|:---|:---|:---|
| **Architektura** | Hugo SSG | Perfektní pro statický obsah, žádné backend |
| **Design** | Temný motovýletový styl | Autentický, legible, pasuje na komunitu |
| **Metadata** | Distance, days, country, image | Logické a připravené na rozšíření |
| **Navigace** | Waze + Mapy.cz | Praktické pro motorkáře na cestě |
| **Obsah** | Detailní itineráře | Špičková kvalita textů, praktické tipy |

### Co chybí nebo lze zlepšit ⚠️

| Problém | Dopad | Řešení |
|:---|:---|:---|
| Chybí **Google Maps** | Zahraniční uživatelé | Přidat Google Maps link vedle Mapy.cz |
| Bez **interaktivních map** | Horší UX, údaje v obrázku | Přidat statické mapy + interactive overlays |
| **Nejednotný formát** | Chaotické hledání tras | Standardizovat: 1 Google + 1 Mapy.cz + 1-6 Waze segmentů |
| Chybí **tagging/filtering** | Nelze filtrovat po kraju/obtížnosti | Přidat taxonomii (difficulty, region, type) |
| Bez **galerie fotek** | Nudnější obsah | Section: "Fotografie ze staveniska" |
| Chybí **komentáře/tipy** | Osamělé itineráře | GitHub Issues nebo Disqus na článcích |
| Žádný **RSS/newsletter** | Uživatelé miss updates | Přidat RSS feed v `<head>` |
| Bez **SEO optimalizace** | Špatná vyhledavačová viditelnost | Meta tags, structured data (JSON-LD) |

---

## 🚀 DOPORUČENÁ VYLEPŠENÍ – PRIORITA

### FÁZE 1: KRITICKÉ (Tímto týdnem)

#### 1.1 Standardizovat Map Linky Všech Výletů
**Cíl:** Každý trip má Google Maps + Mapy.cz + Waze segmenty

**Co dělat:**
- [ ] Projít všechny 3 stávající výlety (`slovensko-2dny`, `tatry`, `slovensko-madarsko`)
- [ ] Přidat **Google Maps link** u každého dne
  - Příklad: `https://www.google.com/maps/dir/Ostrava/Terchová+SK/Liptovská+Mara`
- [ ] Ověřit **Mapy.cz link** (již tam je, jen check)
- [ ] Logicky segmentovat **Waze** na 3-5 úseků místo chaotických

**Příklad konečného formátu:**

```markdown
## 🏍️ 1. Den: Ostrava → Liptovská Mara (240 km)

📍 **[Otevřít v Google Maps](https://www.google.com/maps/dir/Ostrava/...)**

📍 **[Otevřít na Mapy.cz](https://mapy.com/fnc/v1/route?...)**

📱 **Navigace pro Waze (postupné úseky):**
1. [1. úsek: Ostrava ➔ Bílá](https://waze.com/ul?ll=49.442650,18.455200&navigate=yes)
2. [2. úsek: Bílá ➔ Terchová](https://waze.com/ul?ll=49.258900,19.034780&navigate=yes)
3. [3. úsek: Terchová ➔ Liptovská Mara](https://waze.com/ul?ll=49.109880,19.544150&navigate=yes)
```

---

#### 1.2 Přidat Google Maps Skript v `baseof.html`
**Cíl:** Možnost zobrazit embeded mapu (future feature)

**Akce:**
- [ ] Do `themes/moto/layouts/_default/baseof.html` přidat v `<head>`:
```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script>
```

*(API key je free tier, max 1000 denních requests)*

---

#### 1.3 Přidat `difficulty` metadata do front-matter
**Cíl:** Připravit site na filtrování

**Co změnit:**
Přidat do všech trip souborů:
```yaml
difficulty: "Medium"  # Easy / Medium / Challenging
region: "Liptov"      # Region v rámci Slovenska
tags: ["tatry", "termal", "beskydy"]  # Volně definované tagy
```

---

### FÁZE 2: UŽITEČNÉ (Příští týden)

#### 2.1 Vytvořit Index/Filtering Stránku
**Cíl:** Uživatelé mohou filtrovat výlety "Chci jenom lehké jednodenní výlety v ČR"

**Implementace:**
- [ ] Nová stránka: `content/filtry/_index.md` s JavaScript filtrem
- [ ] Hugo Taxonomy: kategorizovat trip dle `difficulty` a `region`
- [ ] Vytvořit `layouts/taxonomy/list.html` pro zobrazení filtered trips

**Příklad výsledku:**
- `/filtry/difficulty/easy/` → Všechny lehké výlety
- `/filtry/difficulty/medium/` → Střední obtížnost
- `/filtry/region/tatry/` → Jenom výlety v Tatrách

---

#### 2.2 Přidat Galerii Fotek u Každého Trip
**Cíl:** Více vizuálního obsahu, lepší sociální sharing

**Struktura:**
```
content/slovensko-2dny/
  ├── index.md
  └── images/
      ├── route-map.png
      ├── day1-stop1.jpg
      ├── day1-stop2.jpg
      └── day2-finish.jpg
```

**V `single.html` přidat:**
```html
{{ if .Resources.ByType "image" }}
  <section class="gallery">
    {{ range .Resources.ByType "image" }}
      {{ if ne .Name "route-map.png" }}
        <figure class="gallery-item">
          <img src="{{ .Permalink }}" alt="Trip photo" loading="lazy" />
        </figure>
      {{ end }}
    {{ end }}
  </section>
{{ end }}
```

---

#### 2.3 Přidat RSS Feed
**Cíl:** Uživatelé mohou subscribovat na nové výlety

**Implementace:**
- [ ] Hugo automaticky generuje `/index.xml`
- [ ] Přidat do `baseof.html`:
```html
<link rel="alternate" type="application/rss+xml" title="Motovýlety RSS" href="{{ .Site.BaseURL }}index.xml" />
```

---

#### 2.4 Přidat JSON-LD Schema (SEO)
**Cíl:** Google Maps, Google Search, sociální sítě vidí strukturovaná data

**Přidat do `single.html`:**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "TouristTrip",
  "name": "{{ .Title }}",
  "description": "{{ .Description }}",
  "itinerary": [
    {
      "@type": "ItemList",
      "name": "Denní itinerář",
      "itemListElement": [...]
    }
  ],
  "image": "{{ .Params.image | absURL }}",
  "duration": "P{{ .Params.days }}D"
}
</script>
```

---

### FÁZE 3: POKROČILÉ (Později)

#### 3.1 Přidat GPX Export
**Cíl:** Uživatelé si mohou stáhnout trasu přímo do GPS navigace

**Postup:**
- [ ] Vytvořit pro každý trip `route.gpx` soubor (XML format)
- [ ] Přidat link: `[Stáhnout GPX trasu](/content/trip-name/route.gpx)`

---

#### 3.2 Přidat Interaktivní Mapu (Leaflet + OpenStreetMap)
**Cíl:** Bez závislosti na Google/Mapy.cz, open-source řešení

**Implementace:**
```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<div id="trip-map" style="height: 500px;"></div>
<script>
  const map = L.map('trip-map').setView([49.0, 19.0], 8);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
  // Load GPX or polyline
</script>
```

---

#### 3.3 Přidat Comments/Discussion (GitHub Discussions)
**Cíl:** Komunita si výměňuje tipy, zkušenosti

**Postup:**
- [ ] Aktivovat GitHub Discussions v repo
- [ ] Přidat do `single.html`: giscus widget (GitHub-backed comments)

---

#### 3.4 Přidat Difficulty/Elevation Profile
**Cíl:** Uživatelé vědí, jak náročný výlet je

**Přidat k front-matter:**
```yaml
difficulty: "Medium"
elevation_gain: "1200 m"  # Total climb
elevation_loss: "1200 m"  # Total descent
avg_altitude: "650 m"
```

---

## 📝 WORKFLOW: JAK PŘIDÁVAT NOVÉ VÝLETY

### Před začátkem:
1. Přečti `.github/skills/motovylet-trip-creation/SKILL.md` (právě jsem ti ji vytvořil!)
2. Máš GPS trace nebo alespoň seznam měst?

### Krok za krokem:

```
1. PLÁNOVÁNÍ
   ├─ Definuj jasné body: Start, Stop1, Stop2, ... End
   ├─ Spočítej vzdálenost a čas
   └─ Rozhodněte se na "Golden sections" (nejlepší kousky)

2. MAPY
   ├─ Otevři Google Maps → vytvoř route → zkopíruj link
   ├─ Otevři Mapy.cz → vytvoř route → zkopíruj URL (s parametry)
   └─ Vytvoř Static map: Google My Maps nebo Snimek Mapy.cz (1200x800px)

3. NAVIGACE
   ├─ Vytvoř 3-5 Waze segmentů (ne více!)
   ├─ Testuj každý link na mobilu
   └─ Zkopíruj přesné souřadnice z Mapy.cz

4. OBSAH
   ├─ Napiš detailní popis každého úseku
   ├─ Přidej praktické tipy (benzín, mýto, nebezpečí)
   ├─ Zařaď fotos-topy ("📷 Foto-stop: ...")
   └─ Zahrň ubytování + jidlo + bezpečnost motorek

5. STRUKTURA
   ├─ Vytvoř: content/my-trip-name/index.md
   ├─ Vytvoř: content/my-trip-name/images/
   ├─ Ulož Static Map: content/my-trip-name/images/route-map.png
   └─ Přidej alle ostatní fotky

6. FRONT-MATTER
   ---
   title: "Krásný název výletu (lokace, délka, highlight)"
   description: "2-3 věty – pro co vhodný, jak dlouhý, proč Sportster"
   date: 2026-MM-DD
   image: "images/route-map.png"
   distance: "240 km / day"
   days: "2"
   country: "SK"
   difficulty: "Medium"
   region: "Liptov"
   ---

7. TEST & COMMIT
   ├─ Zkus `hugo server` lokálně
   ├─ Otestuj všechny mapy linky
   ├─ Ověř, že obrázky se zobrazují
   └─ Commitni s zpráva: "Add trip: Název výletu"
```

### Šablona pro kopírování:

```markdown
---
title: "[LOKALITA] – [DÉLKA] motovýlet [HIGHLIGHT]"
description: "Krátký popis zaměřený na proč to je cool a proč to Sportster zvládne."
date: YYYY-MM-DD
image: "images/route-map.png"
distance: "XXX km / day"
days: "X"
country: "SK"
difficulty: "Medium"
region: "Liptov"
---

Úvodní odstavec (2-3 věty) – Proč jet tento výlet? Co je na něm speciálního? Proč je vhodný pro Sportster?

---

## 🗺️ Přehled trasy

| Den | Trasa | Vzdálenost | Highlight |
|:---|:---|:---|:---|
| **1. Den** | Start → Stop1 → Stop2 → End | XXX km | What makes this special |

---

## 🏍️ 1. Den: Popis (XXX km)

📍 [Otevřít v Google Maps](GOOGLE_MAPS_LINK)

📍 [Zobrazit na Mapy.cz](MAPYCZ_LINK)

📱 **Navigace pro Waze (postupné úseky):**
1. [1. úsek: Start ➔ Stop1](WAZE_LINK1)
2. [2. úsek: Stop1 ➔ Stop2](WAZE_LINK2)
3. [3. úsek: Stop2 ➔ End](WAZE_LINK3)

### Detailní popis:
1. **Start → Stop1 (cca X km)**
   - Druh cesty (zatáčky, svah, asfalt kvalita)
   - Co vidět ("📷 Foto-stop: ...", "Landmark XYZ")
   - Jídlo/pití tip (💡)

2. **Stop1 → Stop2 (cca X km)**
   - ...

3. **Stop2 → End (cca X km)**
   - ...

---

## 🛠️ Praktické tipy

**Dálniční mýto:** [Info pro danou zemi]

**Tankování:** [Kde natankovat]

**Nebezpečí:** [Horský přechod, curve, atd.]

---

## 🏕️ Ubytování a Jídlo

- Privát, kemping, nebo hotel
- Ceny, bezpečí motorek
- Kde jíst
```

---

## 🎨 DESIGN & UX ÚPRAVY

### Minor CSS Tweaks
```css
/* Přidat do themes/moto/static/css/main.css */

/* Better map link styling */
.map-links {
  display: flex;
  gap: 1rem;
  margin: 2rem 0;
  flex-wrap: wrap;
}

.map-link {
  padding: 0.75rem 1.5rem;
  background: var(--badge-bg);
  border: 1px solid var(--badge-border);
  border-radius: 8px;
  color: var(--accent);
  text-decoration: none;
  transition: all 0.3s ease;
}

.map-link:hover {
  background: var(--accent);
  color: var(--bg-primary);
}

/* Gallery styling */
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin: 3rem 0;
}

.gallery-item {
  border-radius: var(--radius);
  overflow: hidden;
  aspect-ratio: 4/3;
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.gallery-item:hover img {
  transform: scale(1.05);
}
```

---

## 📋 CHECKLIST PRO NOVÝ TRIP

Před publikací zkontroluj:

- [ ] Mám statickou mapu (1200x800px)?
- [ ] Google Maps link je testovaný a pracuje?
- [ ] Mapy.cz link je testovaný a pracuje?
- [ ] Mám 3-5 Waze segmentů, všechny jsou testovány?
- [ ] Front-matter obsahuje všechna povinná pole?
- [ ] Obsah je v češtině bez chyb?
- [ ] Jsou přítomné praktické tipy (benzín, bezpečnost)?
- [ ] Jsou označena "Photo stops" a "Meal tips"?
- [ ] Ubytování + parkování motorky = řešeno?
- [ ] `hugo server` běží bez chyb?
- [ ] Obrázky se zobrazují správně?
- [ ] Všechny linky fungují (žádné 404)?

---

## 🔐 GIT WORKFLOW

```bash
# 1. Vytvoř feature branch
git checkout -b add/new-trip-name

# 2. Vytvoř adresář + obsah
# content/new-trip-name/
#   ├── index.md
#   └── images/
#       ├── route-map.png
#       └── [photos]

# 3. Test lokálně
hugo server

# 4. Commit
git add .
git commit -m "Add trip: Název výletu

- Přidej popis co obsahuje
- Třeba kolik dní, kde vede
- Jaké jsou ty nejlepší kousky

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"

# 5. Push
git push origin add/new-trip-name

# 6. Vytvoř Pull Request na GitHub

# 7. Deploy (automaticky po merge)
```

---

## 🎯 SHRNUTÍ

**Hotovo v SKORO NÍC:**
- ✅ Skill file pro budoucí sebou-práci
- ✅ Standardní formát pro mapy (Google + Mapy.cz + Waze)
- ✅ Metadata struktura (difficulty, region, tags)
- ✅ Detailní workflow & checklist

**Příští týden:**
- Aktualizuj existující 3 výlety na nový standard (Add ALL 3 map types)
- Přidej difficulty/region metadata
- Přidej galerii fotek

**Později:**
- Interaktivní filtry
- RSS feed
- GPX export
- Leaflet interactive map
- Community comments

---

## 📚 Soubory Které Máš Teď

1. ✅ `.github/skills/motovylet-trip-creation/SKILL.md` – Kompletní guide
2. ✅ Tato analýza a doporučení
3. 📝 TODO: Aktualizuj existující výlety na nový standard

**Začíná tady: `.github/skills/motovylet-trip-creation/SKILL.md` – Use it!**
