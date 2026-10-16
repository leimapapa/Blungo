# Country media and progression

`settings.json` is the ordered country registry and unlock configuration. Add
an entry to its `countries` array and create a matching folder named for its
`id`; no JavaScript registry changes are needed. Array order determines the
level-card order. To add a level, create the country folder, add its map, hat,
drink, music, collection sound, and `bars.geojson`, then append an entry to the
settings array with a higher `unlockBAC` than the previous country. Copy the
shape of an existing entry and replace its values and outline. Every entry has:

- `id` — lowercase folder name containing letters, digits, and hyphens.
- `name` — label shown in the game.
- `unlockBAC` — cumulative in-game BAC percentage required to unlock the level.
  The first country must have a threshold of `0`; later thresholds must be in
  ascending order.
- `drinkName` — label for the collectible drink, such as `beer` or `wine`.
- `bacPerDrink` — percentage points added to persistent BAC for each drink
  collected (for example, `0.04` for beer or `0.08` for wine).
- `outline` — a closed mainland ring of `[longitude, latitude]` pairs in WGS 84.
  It defines the map's bounds and bounce box; leave islands and disconnected
  areas out.

BAC is a cumulative game-progression meter, not a physiological estimate.
Progress is saved in the browser each time drinks are collected, and unlocks
update immediately. For example, the default America level adds `0.04` BAC
points per beer, so collecting 50 drinks earns `2.00` points and unlocks
France.

Each country folder uses these asset names:

- `country.svg` — mainland illustration aligned to the bounds in `outline`.
- `hat.svg` and `drink.svg` — level art and the collectible.
- `music.mp3` and `collect.mp3` — background music and collection sound.
- `bars.geojson` — an OGC GeoJSON `FeatureCollection` of bar locations.

Bar features should be `Point` geometries with WGS 84 longitude/latitude in
`[longitude, latitude]` order (RFC 7946). Put the 50 most popular venues first;
the game uses up to 50 valid points that fall within the country's mainland.
An empty collection uses generated collectible locations inside the mainland
outline instead.

Serve the HTML and `media` folder from the same local or hosted HTTP/HTTPS
origin rather than opening the HTML as `file://`. The browser needs HTTP to
fetch `settings.json` and GeoJSON and to play local MP3 assets. For a local
test, run `py -m http.server 8000` from the folder containing `drinkin.html`,
then open `http://localhost:8000/drinkin.html`.
