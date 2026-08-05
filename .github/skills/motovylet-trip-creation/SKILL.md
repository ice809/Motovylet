---
name: motovylet-trip-creation
description: Create consistent, well-structured motorcycle trip itineraries with embedded maps (static, Google Maps, Mapy.cz) and Waze navigation links for the Motovylet project.
---

# Motovylet Trip Creation & Enhancement Guide

This skill ensures all new motorcycle trip itineraries follow a consistent format with professional mapping integration, smooth user experience, and complete navigation guidance.

## Quick Start: Adding a New Trip

### 1. Create the Directory

Create a new folder under `content/` with a URL-friendly name:
```
content/my-trip-name/
  ├── index.md          # Main trip content
  └── images/           # Trip images and maps
      └── route-map.png
```

### 2. Essential Front Matter

Every trip MUST include this front matter in `index.md`:

```yaml
---
title: "Descriptive trip title (location, duration, key feature)"
description: "2-3 sentence summary - destination, highlights, Sportster suitability"
date: YYYY-MM-DD         # Trip date or publish date
image: "images/route-map.png"  # Static map image
distance: "240-250 km / day"   # Format: "XXX km" or "XXX-YYY km / day"
days: "2"                      # Number of days
country: "SK"                  # ISO 2-letter: CZ, SK, HU, PL, etc.
---
```

**Why each field matters:**
- `title`: Users see this on homepage card + browser tab
- `description`: Homepage card preview + SEO
- `date`: Sorting trips (newest first on homepage)
- `image`: Trip card thumbnail + article hero image
- `distance`, `days`, `country`: Metadata badges + future filtering

### 3. Content Structure

Follow this proven structure for maximum clarity:

```markdown
---
[front matter]
---

1-2 paragraph introduction explaining the trip's character and why it suits Harley-Davidson Sportster

---

## 🗺️ Route Overview / Přehled trasy

**Summary table with:**
- Day-by-day breakdown
- Main stops/route
- Distance per day
- Highlight for each day

| Den | Trasa | Vzdálenost | Highlight |
|:---|:---|:---|:---|
| **1. Den** | Start → Middle → End | 240 km | Key feature |

---

## 🏍️ Day-by-Day Breakdown

### Day 1: Title describing character/destination (X km)

**Static map image** (if available):
```
![Route map day 1](images/day1-map.png)
```

**Google Maps link** (interactive full route):
```
📍 [View full route on Google Maps](https://www.google.com/maps/dir/START_LOCATION/WAYPOINT1/WAYPOINT2/END_LOCATION)
```

**Mapy.cz link** (interactive route):
```
📍 [Zobrazit trasu na Mapy.cz](https://mapy.com/fnc/v1/route?start=LAT1,LNG1&end=LAT2,LNG2&waypoints=LAT3,LNG3&routeType=car_fast&mapset=outdoor)
```

**Waze navigation** (segmented for riding safety):
```
📱 **Navigace pro Waze (postupné úseky):**
1. [1. úsek: Start ➔ Stop1](https://waze.com/ul?ll=LAT,LNG&navigate=yes)
2. [2. úsek: Stop1 ➔ Stop2](https://waze.com/ul?ll=LAT,LNG&navigate=yes)
3. [3. úsek: Stop2 ➔ End](https://waze.com/ul?ll=LAT,LNG&navigate=yes)
```

**Segment-by-segment description:**
- Road character (curves, elevation, asphalt quality)
- Photo stops and attractions
- Meal recommendations (with emoji: 💡 Tip na oběd)
- Practical warnings (fuel, tolls, speed limits)

### Day 2: [Same structure]

---

## 🛠️ Practical Tips

**Dálniční poplatky / Toll stickers:**
- Motorcycle exemptions for each country
- Where to buy if needed

**Tankování / Fuel:**
- Distance between gas stations
- Recommended refuel locations

**Počasí / Weather:**
- Elevation specifics if mountain pass
- Seasonal considerations

**Doklady / Required documents:**
- If crossing international borders

---

## 🏕️ Accommodation

- Camp options (price/person, security for bikes)
- Private rooms
- Hotel recommendations with secure parking
```

### 4. Generating Map Links

#### Static Map Image
- Use Google My Maps or similar to create a route
- Export as PNG (1200x800px ideal)
- Save to `content/your-trip/images/`
- Embed with: `![Route map](images/route-map.png)`

#### Google Maps Link
Google Maps link format:
```
https://www.google.com/maps/dir/
  START_ADDRESS/
  WAYPOINT1_ADDRESS/
  WAYPOINT2_ADDRESS/
  END_ADDRESS
```

Example:
```
https://www.google.com/maps/dir/Ostrava+Radvanice/Terchová+Slovakia/Oravský+Podzámok/Liptovská+Mara
```

**Getting coordinates:**
1. Right-click on Google Maps
2. Select "What's here?"
3. Copy lat/lng from bottom of screen

#### Mapy.cz Link (Czech map service)
Format:
```
https://mapy.com/fnc/v1/route?
  start=LAT1,LNG1&
  end=LAT2,LNG2&
  waypoints=LAT3,LNG3;LAT4,LNG4&
  routeType=car_fast&
  mapset=outdoor
```

**Steps:**
1. Open mapy.com
2. Right-click on start point → Copy coordinates (format: `49.820923, 18.283088`)
3. Do same for end point
4. Intermediate stops go in `waypoints` parameter, separated by semicolon
5. Always use `routeType=car_fast` (best for motorcycles)
6. `mapset=outdoor` shows topography (useful for mountain passes)

#### Waze Links (Segmented Navigation)
**Important:** Break routes into 3-6 segments max. Don't overwhelm riders with 10+ waypoints.

Each segment gets its own link:
```
https://waze.com/ul?ll=LAT,LNG&navigate=yes
```

- `ll` = latitude, longitude of destination
- Waze will automatically route from current location to this point
- Riders can tap segment-by-segment without needing full route in app

**Example segmented approach:**
```
1. [1. úsek: Ostrava ➔ Bílá](https://waze.com/ul?ll=49.442650,18.455200&navigate=yes)
2. [2. úsek: Bílá ➔ Terchová](https://waze.com/ul?ll=49.258900,19.034780&navigate=yes)
3. [3. úsek: Terchová ➔ Oravský Podzámok](https://waze.com/ul?ll=49.261890,19.379760&navigate=yes)
```

### 5. Homepage Metadata (Automatic)

The theme automatically displays:
- Trip card with `image` thumbnail
- `title` as card heading
- `description` as preview text
- Badges: `distance`, `days`, `country` (emoji formatted)

No additional homepage edits needed—just add the trip to `content/`.

---

## Best Practices

### Writing Style
✅ **DO:**
- Use Czech language throughout
- Include practical rider tips (fuel, toll, speed limits)
- Mention photo stops ("📷 Foto-stop:")
- Use emojis for sections (🏍️, 📍, 💡, 🛠️, 🏕️)
- Be specific: "Dobrý asfalt, táhlé zatáčky" vs vague "pěkná cesta"
- Mention bike security concerns

❌ **DON'T:**
- Mix Czech and English
- Omit practical details (fuel gaps, dangerous curves)
- Create map links without testing them
- Assume rider familiarity with remote areas

### Metadata Quality
- **distance:** Include per-day if multi-day (e.g., "240 km / day")
- **days:** Use "1 (jednodenní)" for clarity
- **country:** Use ISO codes (CZ, SK, HU, PL) for consistency and future filtering
- **date:** Use actual trip date; helps with sorting and archive

### Map Link Testing
Before committing:
1. Test each Google Maps link in browser
2. Test each Mapy.cz link
3. Click 2-3 Waze segments to verify coordinates
4. Verify static map image loads correctly

### Images
- Trip map: 1200x800px PNG or JPG, <500KB
- Store in `images/` subfolder
- Use descriptive names: `day1-route-map.png`, not `map1.png`

---

## Learnings & Pitfalls

**Learnings:**

1. **Waze segmentation prevents rider frustration** — A 300 km route with 15 waypoints in Waze becomes impossible to follow while riding. Breaking into 3-5 logical segments (stop for fuel, meal, photo) matches real-world riding behavior and reduces route abandonment.

2. **Mapy.cz is crucial for Czech riders** — While Google Maps works everywhere, Czech and Slovak riders strongly prefer Mapy.cz for navigation in Central Europe. Missing Mapy.cz links = incomplete trip info for target audience.

3. **Static map + interactive links = best UX** — Display a beautiful static map image in the article hero; supplement with Google/Mapy.cz/Waze links. This gives offline-ready context plus navigation options.

4. **Country metadata enables future filtering** — Even if filtering isn't implemented yet, storing ISO country codes (CZ, SK, HU, PL) means future site updates can easily add "filter by country" or "all trips in Tatry region" without re-editing past trips.

5. **Specific elevation/speed warnings save lives** — "Horský přechod Huty platí omezení 60 km/h" is not just flavor text; it's safety-critical info riders need before arriving at mountain pass at speed.

---

## Example: Complete New Trip Template

```markdown
---
title: "Čtyřdenní okruh Vysoké Tatry & Dunaj"
description: "Technické horské zatáčky Tater, české lénosti a termální lázně. Celek 900 km na kvalitním asfaltu bez dálnic."
date: 2026-09-15
image: "images/tatry-dunaj-map.png"
distance: "220-250 km / den"
days: "4"
country: "SK"
---

Tento čtyřdenní motovýlet kombinuje nejlepší, co střední Evropa nabízí: technické alpské zatáčky Tater, milosrdně krásné dunajské nížiny a při návratu tradičně české lesy.

---

## 🗺️ Přehled trasy

| Den | Trasa | Vzdálenost | Highlight |
|:---|:---|:---|:---|
| **1. Den** | Ostrava → Liptovská Mara | 240 km | Horský přechod Huty |
| **2. Den** | Liptovská Mara → Tatranská Lomnica | 120 km | Serpentiny pod Vysokými Tatrami |
| **3. Den** | Tatranská Lomnica → Veľký Meder | 280 km | Dunajská nížina |
| **4. Den** | Veľký Meder → Ostrava | 260 km | Strážovské vrchy |

---

## 🏍️ 1. Den: Do srdce Liptova (240 km)

[... rest of content ...]
```

---

## File Checklist Before Publishing

- [ ] Front matter complete (title, description, date, image, distance, days, country)
- [ ] Static map image exists and displays
- [ ] Google Maps link tested and working
- [ ] Mapy.cz link tested and working
- [ ] Waze segments (3-5 per day) all tested
- [ ] Segment coordinates are accurate
- [ ] Content in Czech (or consistent with site language)
- [ ] Practical tips included (fuel, tolls, speed limits, bike security)
- [ ] Photo stops marked with 📷
- [ ] Accommodation section completed
- [ ] No broken links
- [ ] Metadata badges align with content

---

## Related Skills & Instructions

- None yet, but future skills could include:
  - Automated map generation from GPX tracks
  - Gallery image optimization
  - Trip archive/tagging system
  - Difficulty rating system (Easy / Medium / Challenging)
