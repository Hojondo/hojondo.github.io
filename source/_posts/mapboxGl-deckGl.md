---
title: mapboxGl&deckGl
top: false
cover: false
toc: true
mathjax: false
date: 2020-07-07 14:31:50
tags:
password:
summary:
categories:
---
```js
import React, { useEffect, useState } from 'react';
import { StaticMap } from 'react-map-gl';
import DeckGL, { GeoJsonLayer, ArcLayer, PolygonLayer } from 'deck.gl';
import { MAPBOX_ACCESS_TOKEN_PUBLIC } from '../../data/mapbox-token'
// Set your mapbox token here
const MAPBOX_TOKEN = MAPBOX_ACCESS_TOKEN_PUBLIC;

// source: Natural Earth http://www.naturalearthdata.com/ via geojson.xyz
const AIR_PORTS =
    'https://d2ad6b4ur7yvpq.cloudfront.net/naturalearth-3.3.0/ne_10m_airports.geojson';

const INITIAL_VIEW_STATE = {
    latitude: 51.47,
    longitude: 0.45,
    zoom: 2,
    maxZoom: 7,
    bearing: 0,
    // pitch: 30
};

const ployData =
    'https://raw.githubusercontent.com/visgl/deck.gl-data/master/website/sf-zipcodes.json'
export default function Map2D(props, ref) {
    function _onClick(info) {
        if (info.object) {
            // eslint-disable-next-line
            alert(`${info.object.properties.name} (${info.object.properties.abbrev})`);
        }
    }
    const initialLayers = [
        new GeoJsonLayer({
            id: 'airports',
            data: AIR_PORTS,
            // Styles
            filled: true,
            pointRadiusMinPixels: 2,
            pointRadiusScale: 2000,
            getRadius: f => 11 - f.properties.scalerank,
            getFillColor: [200, 0, 80, 180],
            // Interactive props
            pickable: true,
            autoHighlight: true,
            onClick: _onClick
        }),
        new ArcLayer({
            id: 'arcs',
            data: AIR_PORTS,
            dataTransform: d => d.features.filter(f => f.properties.scalerank < 4),
            // Styles
            getSourcePosition: f => [-0.4531566, 51.4709959], // London
            getTargetPosition: f => f.geometry.coordinates,
            getSourceColor: [0, 128, 200],
            getTargetColor: [200, 0, 80],
            getWidth: 1,
            opacity: .4,
            coef: 0.005
        }),
        new PolygonLayer({
            id: 'polygon-layer',
            data: ployData,
            pickable: true,
            stroked: true,
            filled: true,
            opacity: 1,
            wireframe: true,
            autoHighlight: true,
            lineWidthMinPixels: 1,
            getPolygon: d => d.contour,
            getElevation: d => d.population / d.area / 10,
            getFillColor: '#de6868',
            getLineColor: [80, 80, 80],
            getLineWidth: 1,
            extruded: true,
            visible: true
        })
    ];
    const [layers, setLayers] = useState(initialLayers)
    useEffect(() => {
        // 
        return () => {
            //   cleanup
        }
    }, [])
    return (
        <DeckGL
            initialViewState={INITIAL_VIEW_STATE}
            controller
            layers={layers}
            // getTooltip={({object}) => object && `${object.zipcode}\nPopulation: ${object.population}`}
        >
            <StaticMap mapboxApiAccessToken={MAPBOX_TOKEN} mapStyle="mapbox://styles/mapbox/light-v10" />
        </DeckGL>
    );
}
```