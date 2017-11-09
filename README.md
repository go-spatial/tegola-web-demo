# Tegola Web Demo

This repo contains a web page demonstrating the rendering of a map utilizing [Tegola](https://github.com/terranodo/tegola) for vector tile delivery.

## Configuration

In the root of this repo, there is an index.html file which contains a configuration JSON that allows for customizable styles, renderers, and sources.

The following is an example JSON configuration:

```json
{
	"start":{
		"zoom":15,
		"longitude":-117.15117725013909,
		"latitude":32.72269876352742,
		"bearing":0,
		"pitch":0,
		"theme":"hot-osm",
		"source":"ec2",
		"renderer":"mapbox"
	},
    "themes":[{
    	"key":"hot-osm",
    	"label":"HOT OSM",
    	"tileSrc":"images/hot-osm.png",
    	"mapboxStyle":"styles/hot-osm3d.json",
    	"olStyle":"styles/hot-osm.json"
    },{
    	"key":"camo",
    	"label":"Camo",
    	"tileSrc":"images/camo.png",
    	"mapboxStyle":"styles/camo3d.json",
    	"olStyle":"styles/camo.json"
    },{
    	"key":"night-vision",
    	"label":"Night",
    	"tileSrc":"images/night-vision.png",
    	"mapboxStyle":"styles/night-vision3d.json",
    	"olStyle":"styles/night-vision.json"
    }],
    "sources":[{
    	"key":"ec2",
    	"label":"EC2",
    	"url":"://osm.tegola.io/capabilities/osm.json?debug=true",
    	"styleName":"osm"
    }],
    "renderers":[{
    	"key":"mapbox",
    	"label":"Mapbox",
    	"library":"mapbox"
    },{
    	"key":"ol",
    	"label":"Open Layers",
    	"library":"ol"
    }]
}
```

The following sections describe each of the configuration parameters in more detail.

## Start

The `start` JSON configuration defines the properties of the map on initial load (if not overwritten by query variables).

- `zoom` - defines the starting zoom of the map based on the zoom levels in the Mapbox rendering library (Open Layers zoom value is one higher than Mapbox's for the same viewport)
- `longitude` - defines the longitude value of the center of the map (web mercator 3857 projection)
- `latitude` -  defines the latitude value of the center of the map (web mercator 3857 projection)
- `bearing` - the Mapbox value for the rotation of the map, if not defined will default to 0
- `pitch` - the Mapbox value for the tilt of the map, if not defined will default to 0
- `theme` - defines the initial theme to be used to render the map, this theme value must match the key of one of the themes defined later in the config JSON
- `source` - defines the initial source to be used to render the map, this source value must match the key of one of the sources defined later in the config JSON
- `renderer` - defines the initial renderer to be used to render the map, this renderer value must match the key of one of the renderers defined later in the config JSON

## Themes

The `themes` JSON configuration defines the themes which can be used to style the map. Themes is an array of theme options to be populated in the page to be selected by the user. Each theme is defined by the following properties:

- `key` - a unique identifier for the theme. Use the same key in the start config to render the map with this theme initially.
- `label` - what will show up in the interface to represent this theme (human readable version of the name).
- `tileSrc` - path to the tile image that will be presented alongside the label.
- `mapboxStyle` - path to the Mapbox style json which defines how the map will be styled. Ensure that the style json matches the source, layers, and features defined in the source.
- `olStyle` - path to the Mapbox style json which defines how the map will be rendered with Open Layers. Note that the olStyle json is in Mapbox style format. It is converted using [ol-mapbox-style](https://github.com/boundlessgeo/ol-mapbox-style).

## Sources

The `sources` JSON configuration defines the Tegola sources which will deliver vector tile information for the map to render. Sources is an array of source options to be populated in the page to be selected by the user. Each source is defined by the following properties:

- `key` - a unique identifier for the source. Use the same key in the start config to render the map with this source initially.
- `label` - what will show up in the interface to represent this source (human readable version of the name).
- `url` - path to the Tegola capabilities json endpoint.
- `styleName` - the source key in the theme style json. This must match the key defined inside the sources parameter in the Mapbox style json.

## Renderers

The `renderers` JSON configuration defines the Tegola renderers which will render vector tiles in the map. Renderers is an array of options that may be selected by the user to render the map. There are two possible renderer options, `mapbox` and `ol`.

- `key` - a unique identifier for the renderer. Use the same key in the start config to render the map with this renderer initially.
- `label` - what will show up in the interface to represent this renderer (human readable version of the name).
- `library` - may be either `mapbox` or `ol`.
- `accessToken` - if using Mapbox styles, define the access token to be able to request styles from the Mapbox API.

## State (Query)

The state of the map is stored in the URI to allow for sharing of links that will recreate the map. The query state parameters are defined below:

- `zoom` - defines the zoom of the map based on the zoom levels in the Mapbox rendering library (Open Layers zoom value is one higher than Mapbox's for the same viewport)
- `longitude` - defines the longitude value of the center of the map (web mercator 3857 projection)
- `latitude` -  defines the latitude value of the center of the map (web mercator 3857 projection)
- `bearing` - the Mapbox value for the rotation of the map, if not defined will default to 0
- `pitch` - the Mapbox value for the tilt of the map, if not defined will default to 0
- `theme` - defines the theme to be used to render the map, this theme value must match the key of one of the themes defined in the config JSON
- `source` - defines the source to be used to render the map, this source value must match the key of one of the sources defined in the config JSON
- `renderer` - defines the renderer to be used to render the map, this renderer value must match the key of one of the renderers defined in the config JSON

Clear the query (everything after the ? in the URL) to restore the start settings of the map.
