# ClimaRadar

App de clima (español) sin API keys: GPS + buscador, mapa Leaflet, radar
animado RainViewer, satélite Sentinel-2 (EOX), pronóstico Open-Meteo y
alertas de tormenta calculadas en el navegador.

## Archivos
- `main.pjs` — solo `$meta` (título/descripción). Sin imports.
- `index.html` — toda la app. Layout: columna lateral con clima +
  mapa (62vh) con grilla 2×2 debajo (Luna, Capas, Sol, Aire).

## Notas técnicas (límites descubiertos verificados)
- RainViewer tiles x/y/z: **zoom máximo 7** → usar `maxNativeZoom:7` en
  Leaflet o devuelve "Zoom Level Not Supported". Los 8-10 fotogramas se
  precargan (todos añadidos al mapa, solo el activo con opacidad) para
  que la animación sea instantánea; si no hay manchas de color es que no
  hay precipitación en la zona visible.
- `satellite.infrared` de RainViewer v2 viene vacío → satélite = EOX
  Sentinel-2 cloudless (`tiles.maps.eox.at`), no RainViewer.
- `tile.openstreetmap.org` da 403 en el preview (política de uso);
  `tile.openstreetmap.fr/hot` da 404 → bases = Esri
  (Street/Imagery/Topo, sin key).
- Geolocalización en el iframe del preview puede no responder; hay
  fallback a Madrid y botón GPS reintentable.
- Luna/pesca: cálculo astronómico local en JS (edad desde luna nueva
  2000-01-06, mes sinódico 29.53058867) + teoría solunar aproximada
  (tránsito ≈ 12h + edad×0.813h). Sin API. Vistas Día (detalle +
  picada en vivo), Semana (7 filas con rating e inicios de periodos
  mayores) y Mes (calendario con resaltado de llena/nueva y navegación).
- Polen: estimación local (base por estación + modificadores de calor,
  viento y lluvia), siempre etiquetado como estimado.
- Aire: `air-quality-api.open-meteo.com` (CAMS: AQI US, PM2.5, PM10,
  polvo en µg/m³). Rocío: `dew_point_2m` del pronóstico + riesgo por
  spread térmico.
- Sol (amanecer/atardecer/alba/crepúsculo civil): algoritmo NOAA en JS
  con `utc_offset_seconds` de Open-Meteo; verificado ±1 min contra
  `daily=sunrise,sunset`.
  2000-01-06, mes sinódico 29.53058867) + teoría solunar aproximada
  (tránsito ≈ 12h + edad×0.813h). Sin API. Vistas Día (detalle +
  picada en vivo), Semana (7 filas con rating e inicios de periodos
  mayores) y Mes (calendario con resaltado de llena/nueva y navegación).
