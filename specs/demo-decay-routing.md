# Demo: decay-aware routing

status: draft · locked: none

A public Python repository that plans deliveries of a product that decays in transit, on an invented
city. A static page shows pre-computed scenarios on interactive maps. Built first.

## Criteria

1. The repository runs from a clean clone with one install command and one command per scenario.
   All data is invented: random customer points on a public basemap, with no real hospital, site or
   order.
2. A sweep over departure time and fleet size solves a vehicle routing problem with time windows
   (OR-Tools) in each cell. The dose to load is back-calculated from each stop's requested dose at its
   time of use, anchored to the departure.
3. At least three scenarios show a trade-off a reader can see on the map: a short half-life, a late
   order, and a vehicle shortage. Each one compares the sweep's choice against the shortest-route plan,
   and reports total activity produced, late stops and vehicles used.
4. A test suite checks the decay physics against hand-computed cases, and checks that every delivered
   dose meets its request within the stated tolerance.
5. The static page loads with no server. The maps use a CDN library and pre-computed JSON, and the
   page is deployable to GitHub Pages.

## Operator brief

**Scenario page.** A map with depots, stops and coloured routes on the left. The scenario switcher and
the comparison table sit on the right. Hovering over a stop shows its requested dose, arrival time and
loaded dose. A timeline under the map shows the activity on each vehicle decaying along its route.

## Check

- `make test` passes, and `make scenarios` regenerates the JSON identically.
- A denylist grep over the repository for internal names returns zero hits.
