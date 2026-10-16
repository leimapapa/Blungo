# Country media

Each country lives in its own folder and uses the same asset names:

- `country.svg` — a mainland illustration aligned to the registry's longitude/latitude outline.
- `hat.svg` and `drink.svg` — level art and the collectible.
- `music.mp3` and `collect.mp3` — background music and collection sound.
- `bars.geojson` — an OGC GeoJSON `FeatureCollection` of bar locations.

Bar features should be `Point` geometries with WGS 84 longitude/latitude in
`[longitude, latitude]` order (RFC 7946). Put the 50 most popular venues first;
the game uses up to 50 valid points that fall within the country's mainland.
An empty collection uses generated collectible locations inside the mainland
outline instead. Serve the HTML and `media` folder from the same local or hosted HTTP/HTTPS
origin rather than opening the HTML as `file://`. Browsers cannot fetch the
GeoJSON in file mode, and may reject local MP3 playback, causing the game to
use its generated bar locations and synthesized music fallback. For a local
test, run `py -m http.server 8000` from the folder containing `drinkin.html`,
then open `http://localhost:8000/drinkin.html`.

To add a destination, add its country object and mainland outline to the
`COUNTRIES` registry in `drinkin.html`, then provide the six assets in its
folder. Keep `country.svg` projected to the same mainland longitude/latitude
bounds as the registry outline so the displayed land and generated drink
locations agree. The outline also determines the invisible rectangular bounce
boundary; disconnected territories should be excluded from both.
