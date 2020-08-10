<template>
  <div class="map-container">
    <div id="mapbox-map"/>
    <canvas id="deck-canvas"/>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import { Deck } from "@deck.gl/core";
import { IconLayer } from "@deck.gl/layers";
export default {
  name: "MapComponent",
  data: () => {
    return {
      deck: null,
      map: null
    };
  },
  mounted() {
    if (!mapboxgl.supported()) {
      alert("your browser is unsupported");
    } else {
      this.initMap();
    }
  },
  methods: {
    initMap() {
      mapboxgl.accessToken =
        "pk.eyJ1IjoibGFicy1zYW5kYm94IiwiYSI6ImNrMTZuanRtdTE3cW4zZG56bHR6MnBkZG4ifQ.YGRP0sZNYdLw5_jSa9IvXg";
      this.map = new mapboxgl.Map({
        container: "mapbox-map",
        interactive: false,
        style: `mapbox://styles/mapbox/streets-v11`,
        center: [0, 0],
        zoom: 3,
        pitch: 0
      });
      this.map.on("style.load", () => {
        this.initDeck();
      });
    },
    initDeck() {
      const ICON_MAPPING = {
        coal: { x: 0, y: 0, width: 32, height: 32 }
      };
      const data = [
        {
          name: "Colma (COLM)",
          address: "365 D Street, Colma CA 94014",
          exits: 4214,
          coordinates: [-122.466233, 37.684638]
        }
      ];

      this.deck = new Deck({
        canvas: "deck-canvas",
        width: "100%",
        height: "100%",
        initialViewState: {
          latitude: 0,
          longitude: 0,
          zoom: 3,
          center: [0, 0],
          pitch: 0
        },
        controller: true,
        onViewStateChange: ({ viewState, interactionState }) => {
          this.map.jumpTo({
            center: [viewState.longitude, viewState.latitude],
            zoom: viewState.zoom,
            pitch: viewState.pitch,
            bearing: viewState.bearing
          });
        },
        layers: [
          new IconLayer({
            id: "icon-layer",
            data,
            pickable: true,
            iconAtlas:
              "https://raw.githubusercontent.com/UttamRahane/IconLayer/master/src/assets/coal.svg",
            iconMapping: ICON_MAPPING,
            getIcon: d => "coal",
            sizeScale: 15,
            getPosition: d => d.coordinates,
            getSize: d => 4
          })
        ]
      });
      this.addLayer();
    },
    addLayer() {}
  }
};
</script>
<style scoped>
.map-container {
  position: relative;
  height: 100vh;
  width: 100vw;
}
#mapbox-map {
  width: 100%;
  height: 100%;
  position: absolute;
}
</style>