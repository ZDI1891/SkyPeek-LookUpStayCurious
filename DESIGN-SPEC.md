# Sky Peek design specification

Single source of truth for Sky Peek. Code changes and mockups follow this document. Revisions need explicit approval from the owner (ZDI).

Status: approved 19 October 2026. Repository: SkyPeek-LookUpStayCurious.

## 1. Identity

- Product name: Sky Peek
- Tagline: Look Up. Stay Curious.
- Credit: By ZDI
- Logo: approved circular emblem (orbital dotted trail, aircraft silhouette, bright star, coastal silhouette) with the Sky Peek wordmark. Assets expected in `assets/`:
  - `skypeek-emblem.png`, transparent, about 512 x 512, used in the header and as the future app icon.
  - `skypeek-logo.jpg` or `.png`, the full lock-up, used in the About panel.
- Until the emblem file is present, the header shows a temporary CSS placeholder of the same size. The placeholder is never described as final branding.

## 2. Design principle

The sky is the experience; the interface supports it. Visual direction comes from the approved dashboard mockup: atmospheric, instrument-like, dark navy with cyan and blue. Functional and data rules come from the written brief: nothing on screen may look live unless it is live.

## 3. Palette and type

| Role | Value |
|---|---|
| Background | #07111F |
| Surface | #0D1B2A |
| Elevated panel | #122438 |
| Border | #24384C |
| Primary text | #F1F6FC |
| Secondary text | #9AAFC3 |
| Muted or disabled | #607387 |
| Cyan accent (aircraft, active, links) | #39C6F0 |
| Blue accent (user location) | #438BFF |
| Warning | #F2C27A |

System sans-serif stack. Product name 18 to 20 px semibold; body 14 to 15 px; labels 12 to 13 px. Sentence case throughout. No em dashes in interface copy.

## 4. Layout

Map fills the viewport. Everything else floats above it.

Header strip: emblem and name left, with tagline where width allows; selected place name centre; local time and date; day or night status; settings control right that opens About. On narrow phones the header collapses to emblem, name and time, and the place name moves into the console.

Map overlays:
- Five geographic rings at 10, 20, 30, 40, 50 km, thin dashed cyan, all five labelled along the north axis, labels subtle and non-overlapping.
- Cross-hair lines north to south and east to west, out to the outer ring.
- Eight compass labels on true bearings; N, E, S, W carry small arrow glyphs; north is up at all times.
- User location: blue dot with white edge, no glow.
- Scale bar bottom left.
- Right-hand vertical controls: locate, zoom in, zoom out, layers. Zoom buttons hidden on touch screens.
- Basemap: Esri World Dark Gray Canvas, no key. Attribution to Esri and data providers always visible. Night-lights imagery is deferred until a licensed sharp source exists.

View rules: the initial view fits the 50 km ring into the free screen area between header and console. Minimum zoom is locked at that level; panning is limited to 150 km from the selected location.

Bottom console: a collapsed strip by default on phones showing aircraft status and an expand handle. Expanded, it shows one card at a time with a tab row: Visible now, Next visible pass, Selected. On screens 900 px and wider, the three cards sit side by side and stay expanded. The console never covers most of the map.

## 5. Layers

The Layers button is the single control for aircraft, satellites and weather. No duplicate segmented control. Unimplemented layers are visibly disabled and labelled "Coming soon". A disabled control does nothing when tapped.

## 6. Aircraft (implemented)

- Source: adsb.fi open data, `/api/v2/lat/{lat}/lon/{lon}/dist/50`, 50 nautical miles (about 93 km, intentionally wider than the 50 km rings). Refresh every 60 seconds. Personal, non-commercial use; attribution with a link required.
- Marker: cyan aircraft glyph rotated to track, with a chip showing callsign and altitude when available. Missing values are omitted from the chip and shown as "unavailable" in the detail card. Values are never invented.
- Tapping an aircraft fills the Selected card (callsign, altitude, speed, heading, and other fields the response provides) and highlights the marker. No Leaflet popup. Clear deselect action.
- Status line: "N aircraft on the map" counts plotted markers only; "No aircraft in range right now"; or "Aircraft data temporarily unavailable" in the warning colour.

## 7. Location

Default: Trivandrum, India. My location requests browser geolocation, shows a loading state, and on success centres the map and refreshes aircraft. On failure the previous location, label and aircraft stay unchanged and the user is told. The interface shows a place name, never precise coordinates. Coordinates are sent to the aircraft data provider with each request; the About panel says so. Manual location search is Stage 3.

## 8. Satellites and weather (not implemented)

Empty-state copy until real data exists:
- Visible now: "Satellite tracking is not available yet."
- Next visible pass: "Pass predictions will appear here once satellite tracking is enabled."
- Selected satellite: "Select a satellite to view its details."

Agreed rules for Stage 4: fixed 30 degree minimum elevation, no slider; distinguish current position from projected ground track; label brightness as estimated; account for sunlight and Earth shadow; satellite layer auto-enabled after 6:30 PM local device time on mobile; morning cutoff undecided, to be agreed before implementation. No Bortle or light-pollution value until a data source is integrated.

Weather radar (Stage 5): optional overlay with timestamp, only after a source is confirmed.

## 9. Mockups

Future mockups follow this document. Illustrative data must be labelled as such and must not be mistaken for a functioning application.

## 10. Stage plan

1. Visual foundation (this document, header, rings, console, basemap). Done when the live site matches sections 3 to 7.
2. Aircraft polish: glyph markers, Selected card, count accuracy, error states.
3. Manual location selection.
4. Satellite data, propagation, passes, visibility.
5. Weather radar evaluation and implementation.
6. Logo assets, favicon, app icon, PWA.

## 11. Working rules

Single `index.html` in the repository root, edited through GitHub's web editor. Before each change: what, why, whether existing functionality is affected, and how to test on GitHub Pages and a phone. No feature is called working until tested on the live site.
