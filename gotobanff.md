# PRD: GoToBanff.com

**Domain:** gotobanff.com  
**Niche:** Banff National Park travel guide  
**Vibe:** Adventure, nature, rugged elegance, mountain aesthetic  

---

## Site Concept

The definitive guide to visiting Banff National Park. Cover everything from hiking trails to hotels, seasonal activities, and practical tips. Focus on helping visitors plan the perfect trip with data-driven comparisons.

**Differentiator:**
- Trail difficulty comparisons with actual data
- Crowd level predictions by time/season
- Cost breakdown calculator
- Wildlife spotting probability by location
- "Skip the crowds" alternative suggestions

---

## Target Keywords

Research with Serper:
- "banff travel guide"
- "best time to visit banff"
- "banff hiking trails"
- "banff vs jasper"
- "banff hotels"
- "lake louise guide"
- "banff in winter"
- "banff itinerary 3 days"

---

## Data Sources

1. **Parks Canada** - official trail info, fees, regulations
2. **AllTrails** (scrape via Jina) - trail reviews, difficulty
3. **Booking.com/TripAdvisor** - hotel data
4. **Weather data** - seasonal patterns
5. **Tourism Banff** - events, activities

---

## Data Schema

```json
{
  "trails": [
    {
      "name": "Plain of Six Glaciers",
      "slug": "plain-of-six-glaciers",
      "area": "Lake Louise",
      "distance": "13.8 km",
      "elevationGain": "365 m",
      "difficulty": "Moderate",
      "time": "4-5 hours",
      "crowdLevel": "High",
      "bestMonths": ["July", "August", "September"],
      "highlights": ["Glacier views", "Tea house", "Lake Louise start"],
      "overview": "One of the most popular hikes in Banff...",
      "tips": ["Start early to avoid crowds", "Tea house is cash only"],
      "wildlife": ["Grizzly bears", "Marmots", "Pikas"],
      "faqs": [
        {"q": "Is the tea house open?", "a": "June to early October, weather permitting"}
      ]
    }
  ],
  "accommodations": [
    {
      "name": "Fairmont Banff Springs",
      "slug": "fairmont-banff-springs",
      "type": "Luxury Hotel",
      "location": "Banff Town",
      "priceRange": "$400-800/night",
      "rating": 4.7,
      "bestFor": ["Luxury seekers", "History buffs", "Spa lovers"],
      "nearbyTrails": ["bow-falls", "tunnel-mountain"],
      "overview": "The iconic 'Castle in the Rockies'...",
      "pros": ["Stunning architecture", "On-site spa", "Central location"],
      "cons": ["Expensive", "Can feel crowded", "Parking fees"]
    }
  ],
  "activities": [
    {
      "name": "Canoeing on Lake Louise",
      "slug": "canoeing-lake-louise",
      "type": "Water Activity",
      "season": "June-September",
      "duration": "1-2 hours",
      "cost": "$155/hour",
      "bookingRequired": true,
      "overview": "Paddle the iconic turquoise waters..."
    }
  ],
  "areas": [
    {
      "name": "Lake Louise",
      "slug": "lake-louise",
      "distanceFromBanff": "57 km",
      "highlights": ["Lake Louise", "Moraine Lake", "Hiking"],
      "bestFor": ["Photography", "Hiking", "Skiing"],
      "crowdLevel": "Very High",
      "tips": ["Arrive before 6am in summer", "Take shuttle to Moraine Lake"]
    }
  ]
}
```

---

## Site Structure

```
/                              # Homepage
/trails                        # All trails
/trails/plain-of-six-glaciers  # Trail page
/areas                         # Areas overview
/areas/lake-louise             # Area page
/stay                          # Accommodations hub
/stay/fairmont-banff-springs   # Hotel page
/activities                    # Things to do
/activities/canoeing-lake-louise
/getting-here                  # Transportation hub (IMPORTANT)
/getting-here/shuttle          # Shuttle comparison
/getting-here/car-rental       # Car rental guide + winter warning
/getting-here/from-calgary     # Airport to Banff guide
/plan                          # Trip planning hub
/plan/best-time-to-visit       # Seasonal guide
/plan/3-day-itinerary          # Sample itinerary
/plan/budget-breakdown         # Cost guide
/compare/banff-vs-jasper       # Comparison
/about
/contact
```

---

## Page Templates

### Homepage
- Hero: Stunning mountain panorama + "Your Banff Adventure Starts Here"
- Quick stats: Park size, trails, lakes, wildlife species
- Season selector (what to do now)
- Featured trails grid
- Area cards
- Trip planning CTA

### Trail Page
- Hero with trail photo
- Stats bar: Distance, Elevation, Time, Difficulty, Crowd Level
- Overview
- Highlights list
- Best time to hike
- Wildlife to watch for
- Tips from locals
- Nearby trails
- FAQs

### Area Page
- Area overview
- Map placeholder
- Top trails in area
- Where to stay nearby
- Activities
- Crowd avoidance tips

### Accommodation Page
- Hotel photo
- Stats: Price, Rating, Location
- Overview
- Pros/Cons
- Nearby trails
- Booking links (affiliate)

---

## Theme

```css
:root {
  --color-primary: #2d5016;      /* Forest green */
  --color-secondary: #4a7c23;    /* Lighter green */
  --color-accent: #f4a460;       /* Sandy orange (sunset) */
  --color-accent-2: #87ceeb;     /* Sky blue */
  --color-bg: #f5f5f0;           /* Warm off-white */
  --color-card: #ffffff;         /* White cards */
  --color-text: #2d3436;         /* Dark gray */
  --color-text-muted: #636e72;   /* Muted gray */
  
  --font-heading: 'Bebas Neue', sans-serif;
  --font-body: 'Source Sans Pro', sans-serif;
}
```

**Visual Style:**
- Bold, adventurous
- Mountain silhouette decorations
- Earthy greens and sunset oranges
- Large hero images
- Condensed uppercase headings
- Trail-marker inspired icons

---

## Unique Content Angles

1. **Crowd Predictor** - Best times to visit each spot
2. **Trail Comparison Tool** - Side-by-side difficulty/views
3. **Wildlife Calendar** - What animals, when, where
4. **Budget Calculator** - Park pass + hotel + activities
5. **Weather Windows** - Best weather probability by month
6. **Alternative Suggestions** - "If X is crowded, try Y"

---

## Getting to Banff (Critical Content)

This is a major pain point for visitors. Create a dedicated `/getting-here` section:

### Transportation Comparison Table
```
| Option | Cost | Time | Winter Safe? | Flexibility | Best For |
|--------|------|------|--------------|-------------|----------|
| Shuttle (Brewster/Banff Airporter) | $75-150 CAD | 2h | ✅ Yes | Low | Solo travelers, winter |
| Car Rental | $50-100/day | 1.5h | ⚠️ Risky | High | Families, summer |
| Private Transfer | $200-400 | 1.5h | ✅ Yes | Medium | Groups, luxury |
| Public Bus (Roam) | $10 | 3h+ | ✅ Yes | Low | Budget travelers |
```

### Winter Driving Warning
Prominent callout:
```html
<div class="warning-card">
  <span class="warning-icon">⚠️</span>
  <h4>Winter Driving Advisory</h4>
  <p>Highway 1 between Calgary and Banff is notorious for winter accidents. 
  Black ice, sudden snowstorms, and whiteout conditions are common Nov-March. 
  <strong>We recommend taking the shuttle</strong> unless you have winter driving experience 
  and proper tires (legally required Oct 1 - Apr 30).</p>
</div>
```

### Shuttle Comparison
```json
{
  "shuttles": [
    {
      "name": "Brewster Express",
      "slug": "brewster-express",
      "price": "$75-85 CAD one-way",
      "frequency": "Multiple daily",
      "pickupLocations": ["YYC Airport", "Downtown Calgary hotels"],
      "dropoffLocations": ["Banff town", "Lake Louise"],
      "bookingRequired": true,
      "winterReliable": true,
      "pros": ["Reliable", "Comfortable", "Direct to hotels"],
      "cons": ["Fixed schedule", "Must book ahead"],
      "affiliateUrl": "https://..."
    },
    {
      "name": "Banff Airporter",
      "slug": "banff-airporter",
      "price": "$70-80 CAD one-way",
      "frequency": "Hourly",
      "winterReliable": true,
      "pros": ["Frequent departures", "Good value"],
      "cons": ["Can be crowded in peak season"]
    }
  ]
}
```

### Car Rental Reality Check
- **Summer**: Great option, stop at viewpoints, explore freely
- **Winter**: Only if you have:
  - Winter driving experience
  - M+S or mountain snowflake tires (REQUIRED by law)
  - Comfort with mountain driving
  - Flexible schedule (roads close in storms)

### Cost Comparison Calculator
Interactive tool showing total cost:
- Shuttle: $150 round trip + $0 parking + $0 stress
- Rental: $70/day × 3 days + $30/day parking + gas + tire upgrade fee

---

## Schema Markup

```json
{
  "@context": "https://schema.org",
  "@type": "TouristAttraction",
  "name": "Plain of Six Glaciers Trail",
  "description": "...",
  "geo": {...}
}

{
  "@context": "https://schema.org",
  "@type": "Hotel",
  "name": "Fairmont Banff Springs",
  "priceRange": "$$$",
  "aggregateRating": {...}
}

{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [...]
}
```

---

## Monetization Plan

1. **Hotel affiliate** - Booking.com, Hotels.com
2. **Activity bookings** - Viator, GetYourGuide
3. **Gear affiliate** - REI, MEC (hiking gear)
4. **Travel insurance** - World Nomads
5. **Sponsored content** - Local tour operators
6. **Premium itineraries** - Downloadable PDF guides

---

## Build Checklist

- [ ] Compile 30+ trails with full data
- [ ] List 20+ accommodations across price ranges
- [ ] Create area guides for 5 main areas
- [ ] Build seasonal activity calendar
- [ ] Add wildlife spotting data
- [ ] Create comparison pages
- [ ] Add affiliate links
- [ ] Create OG images with mountain photos
- [ ] Submit sitemap to GSC

---

## Sample Serper Queries

```javascript
// Keyword research
"banff" + "best hikes"
"lake louise" + "crowds"
"banff" + "budget"
"banff vs jasper" + comparison

// Source discovery
"banff trails" + site:pc.gc.ca
"banff hotels" + site:tripadvisor.com
"banff weather" + by month
```

---

## Future Features

- Interactive trail map
- Trip planner with drag-and-drop
- Real-time crowd data (if API available)
- User photo gallery
- Packing list generator
- Offline trail guides
- Weather alerts integration
