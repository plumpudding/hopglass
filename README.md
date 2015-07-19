[![Build Status](https://travis-ci.org/tcatm/meshviewer.svg?branch=master)](https://travis-ci.org/tcatm/meshviewer)

# Meshviewer

Meshviewer is a frontend for
[ffmap-backend](https://github.com/ffnord/ffmap-backend).


[Changelog](CHANGELOG.md)

# Screenshots

![](doc/mapview.png?raw=true)
![](doc/graphview.png?raw=true)
![](doc/allnodes.png?raw=true)
![](doc/links.png?raw=true)
![](doc/statistics.png?raw=true)

# Dependencies

- npm
- bower
- grunt-cli
- Sass (>= 3.2)

# Installing dependencies

Install npm and Sass with your package-manager. On Debian-like systems run:

    sudo apt-get install npm ruby-sass

Execute these commands on your server as a normal user to prepare the dependencies:

    git clone https://github.com/tcatm/meshviewer.git
    cd meshviewer
    npm install
    npm install bower grunt-cli
    node_modules/.bin/bower install

# Building

Just run the following command from the meshviewer directory:

    node_modules/.bin/grunt

This will generate `build/` containing all required files.

# Configure

Copy `config.json.example` to `build/config.json` and change it to match your community.

## dataPath (string)

`dataPath` must point to a directory containing `nodes.json` and `graph.json`
(both are generated by
[ffmap-backend](https://github.com/ffnord/ffmap-backend)). Don't forget the
trailing slash! Data may be served from a different domain with [CORS enabled].
Also, GZip will greatly reduce bandwidth consumption.

## siteName (string)

Change this to match your communities' name. It will be used in various places.

## mapSigmaScale (float)

This affects the initial scale of the map. Greater values will show a larger
area. Values like 1.0 and 0.5 might be good choices.

## showContact (bool)

Setting this to `false` will hide contact information for nodes.

## maxAge (integer)

Nodes being online for less than maxAge days are considered "new". Likewise,
nodes being offline for less than than maxAge days are considered "lost".

## mapLayers (List)

A list of objects describing map layers. Each object has at least `name`
property and optionally `url` and `config` properties. If no `url` is supplied
`name` is assumed to name a
[Leaflet-provider](http://leaflet-extras.github.io/leaflet-providers/preview/).

## nodeInfos (array, optional)

This option allows to show client statistics depending on following case-sensitive parameters:

- `name` caption of statistics segment in infobox
- `href` absolute or relative URL to statistics image
- `thumbnail` absolute or relative URL to thumbnail image,
  can be the same like `href`
- `caption` is shown, if `thumbnail` is not present (no thumbnail in infobox)

To insert current node-id in either `href`, `thumbnail` or `caption`
you can use the case-sensitive template string `{NODE_ID}`.

Examples for `nodeInfos`:

    "nodeInfos": [
      { "name": "Clientstatistik",
        "href": "nodes/{NODE_ID}.png",
        "thumbnail": "nodes/{NODE_ID}.png",
        "caption": "Knoten {NODE_ID}"
      },
      { "name": "Uptime",
        "href": "nodes_uptime/{NODE_ID}.png",
        "thumbnail": "nodes_uptime/{NODE_ID}.png",
        "caption": "Knoten {NODE_ID}"
      }
    ]

In order to have statistics images available, you have to run the backend with parameter `--with-rrd` or generate them in other ways.

## globalInfos (array, optional)

This option allows to show global statistics on statistics page depending on following case-sensitive parameters:

- `name` caption of statistics segment in infobox
- `href` absolute or relative URL to statistics image
- `thumbnail` absolute or relative URL to thumbnail image,
  can be the same like `href`
- `caption` is shown, if `thumbnail` is not present (no thumbnail in infobox)

In contrast to `nodeInfos` there is no template substitution in  `href`, `thumbnail` or `caption`.

Examples for `globalInfos`:

    "globalInfos": [
      { "name": "Wochenstatistik",
        "href": "nodes/globalGraph.png",
        "thumbnail": "nodes/globalGraph.png",
        "caption": "Bild mit Wochenstatistik"
      },
      { "name": "Jahresstatistik",
        "href": "nodes/globalGraph52.png",
        "thumbnail": "nodes/globalGraph52.png",
        "caption": "Bild mit Jahresstatistik"
      }
    ]

In order to have global statistics available, you have to run the backend with parameter `--with-rrd` (this only creates globalGraph.png) or generate them in other ways.

[CORS enabled]: http://enable-cors.org/server.html
