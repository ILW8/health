# Notes: editing Strava routes through Chrome (Claude in Chrome)

**Prepared:** 11 September 2026
**Context:** Learned while plotting the 80 km Ripley Lanes Loop variant ([route file](../cycling/route_ripley_lanes_loop.md)). Strava's route builder (strava.com/maps/create) is a Mapbox app; not everything responds to synthetic browser input the way a human's mouse does. This is what worked, what didn't, and the workflow to reuse.

---

## Starting from an existing route

- Open the saved route page (`strava.com/routes/<id>`), click the chevron next to **Edit** → **Duplicate**. This opens the builder with a "Save route" dialog already up; click **Edit Route** in the dialog to get into the editor without saving yet. The original route is untouched.
- Editing the original instead (**Edit**) overwrites it. Duplicate unless replacing on purpose.

## Sidebar waypoint list (reliable)

- **Show all waypoints** expands the list. Hovering a row reveals an "Edit Waypoint" label and a **drag handle (⋮⋮) on the far right** of the row.
- **Reordering rows by dragging the handle works** with synthetic drag (`left_click_drag` from the handle to a point between two other rows). This is the most reliable way to put a waypoint in the right place in the route.
- Hovering a row does *not* highlight its marker on the map, so you can't tell where "Waypoint 8" is from the list alone.
- **Clicking a marker on the map** opens a popup titled with the waypoint's number (e.g. "Waypoint 12") plus options: Use manual mode / Customize / **End here** / **Delete**. This is how to work out which list row corresponds to which place. Escape closes it.
- The **Start** marker's popup also has **End here**, which sets the End to the start location without disturbing anything else. Useful for closing a loop.

## Adding waypoints

- **Add waypoint** button (below "End"): per the user, the current End becomes a numbered waypoint and the End field becomes the empty entry for the next point. In practice the list didn't visibly change until something was typed/selected; the flow that worked was: click **Add waypoint** → click into the **End** field → type a place name → pick from the geocoder suggestions. The chosen place becomes End; repeat Add waypoint to turn it into a numbered waypoint and free the End field again.
- Geocoder (Geocode Earth / OSM) suggestions are decent for named places: "Chertsey Bridge", "Roehampton Gate" worked; "Kingston Gate Richmond Park" only offered "Kingston Gate Car Park", which is fine (it's inside the park by the gate). A geocoded gate may sit a few metres outside the park, producing a tiny out-and-back at the gate in the plotted line — harmless.
- Typing a place into the End field when End already holds a location **replaces** it (the old End is lost, not shifted into the list).
- Because new waypoints land at the end of the route, the full pattern for a mid-route insertion is: add via End field → **Add waypoint** → set End back to home (click Start marker → End here) → **drag the new row(s) up** to the right place. Delete any duplicate "home" waypoint that this leaves behind via its map popup (clicking the home marker gets the numbered waypoint, not Start, when a duplicate exists).

## Dragging the route line (unreliable)

- The user's tip: drag anywhere on the orange line to where the new waypoint should go. This *can* work with synthetic input, but:
  - It only worked after **hovering the exact line pixel first** (use the `zoom` screenshot to find it; the line is ~4 px wide) and then dragging from that pixel. Misses pan the map instead — there's no error, the distance just doesn't change.
  - Where outbound and return legs **overlap** (Kingston town centre, the west side of Richmond Park, London Road), Strava picks one leg and inserted the waypoint into the outbound leg when the return leg was intended. Only drag from stretches that belong to one leg (e.g. Hurst Road for the return, Portsmouth Road for the outbound).
- Given the above, the list-drag approach is preferred for anything that has to land between specific waypoints.

## Map navigation

- Scroll-wheel zoom often only registers one or two ticks per call; the **+ / − buttons** at (top-left of map) are more predictable. Panning by dragging empty map works normally.
- Undo/redo buttons sit in the toolbar next to Save Route. **Undo can step back further than expected** (one click undid both an Add waypoint and an earlier reorder); check the distance readout after each undo and use redo if it went too far.
- The distance / elevation / est. moving time readout under the map updates after every change and is the quickest sanity check that an edit did what you meant.

## Saving

- **Save Route** → dialog with name, description, privacy (defaults to Only You) → **Save route**. Lands on the new route's page; the URL is the route ID to record in the repo.
- Triple-click the name field to select the pre-filled name before typing a new one.

## Time cost

Plotting the 80 km variant (one detour waypoint + a three-waypoint park lap, plus fixing two wrong turns) took ~40 browser actions. A single mid-route insertion via the list-drag method is roughly 8–10 actions once the target rows are known.
