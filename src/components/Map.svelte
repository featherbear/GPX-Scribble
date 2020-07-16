<script>
  import "leaflet/dist/leaflet.css";
  import { onMount } from "svelte";

  onMount(async () => {
    const leaflet = (await import("leaflet-filelayer")).default();
    var control;

    var osm = leaflet.tileLayer(
      "http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      {
        attribution: "Map data &copy; 2013 OpenStreetMap contributors",
      }
    );

    var map = leaflet
      .map("map", {
        center: [0, 0],
        zoom: 2,
      })
      .addLayer(osm);

    var style = {
      color: "red",
      opacity: 1.0,
      fillOpacity: 1.0,
      weight: 2,
      clickable: false,
    };

    leaflet.Control.FileLayerLoad.LABEL =
      '<img class="icon" src="./folder.svg" alt="file icon" style="max-width: 70%; max-height: 70%; margin: 4px;" />';
    control = leaflet.Control.fileLayerLoad({
      fitBounds: true,
      layerOptions: {
        style: style,
        pointToLayer: function (data, latlng) {
          returnleaflet.circleMarker(latlng, { style: style });
        },
      },
    });
    control.addTo(map);
    control.loader.on("data:loaded", function (e) {
      var layer = e.layer;
      console.log(layer);
    });
  });
</script>

<style>
  main {
    flex: 1 1 100%;
    position: relative;
    width: 100%;
  }

  #map {
    height: 100%;
    width: 100%;
    /* height: 100vh; */
    position: absolute;
  }

  .help {
    font-size: 1rem;
    position: absolute;
    top: 7.5rem;
    left: 20px;
    right: 0;
    height: auto;
    max-width: 170px;
    z-index: 10;
    background-color: rgba(32, 39, 60, 0.7);
    color: white;
    padding: 1rem;
    margin: 0px;
    border-radius: 0 4px 4px 4px;
  }

  .help::before {
    content: "";
    position: absolute;
    top: -1rem;
    left: 0;
    width: 0;
    height: 0;
    border-right: 1rem solid transparent;
    border-bottom: 1rem solid rgba(32, 39, 60, 0.7);
  }
</style>

<main>

  <p class="help">
    Click on the icon to browse and load local files on the map.
  </p>
  <div id="map" />

</main>
