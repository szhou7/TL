# tl

An command line alternative to
[`tilelive`](https://github.com/mapbox/tilelive.js)'s command that supports
streaming.

## Installation

```bash
npm install -g tl
npm install -g mbtiles tilelive-file
# install any other tilelive modules you'd like to read/write from
```

## Usage

Copy tiles from a filesystem to an MBTiles archive:

```bash
tl copy -r mbtiles -r tilelive-file file://./tiles mbtiles://./tiles.mbtiles
```

Render a [TM2](https://github.com/mapbox/tm2) style for a small area into an
MBTiles archive:

```bash
npm install -g mbtiles tilelive-tmstyle tilelive-mapbox tilejson \
  tilelive-vector@^0.13.0
tl copy 
  -r mbtiles -r tilelive-tmstyle -r tilelive-mapbox -r tilejson \
  -r tilelive-vector \
  -z 6 -Z 6 -b "-124.4096 32.5343 -114.1308 42.0095" \
  tmstyle://./project.tm2/ mbtiles://./tiles.mbtiles
```

## Caveats

Some tilelive providers use other providers but don't depend on them directly
(relying on `tilelive.load()`), so you may need to install (and `--require|-r`)
some modules that you're not using directly.

For example, [`tilelive-tmstyle`](https://github.com/mojodna/tilelive-tmstyle)
loads data using
[`tilelive-mapbox`](https://github.com/mojodna/tilelive-mapbox), which actually
uses [`tilejson`](https://github.com/mapbox/node-tilejson) under the hood to do
the fetching. Rendering is then done using
[`tilelive-vector`](https://github.com/mapbox/tilelive-vector) (which is
declared as a peer dependency of `tilelive-tmstyle`, so versions may not always
match up to whatever's most up-to-date).
