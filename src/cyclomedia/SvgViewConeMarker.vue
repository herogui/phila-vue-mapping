<!--
  wraps leaflet svg-icons as a vue component
  https://github.com/iatkin/leaflet-svgicon
-->

<script>
  import * as L from 'leaflet';
  // import DivIcon from 'leaflet';
  import TriangleIcon from '../util/triangle-icon';

  export default {
    name: 'SvgViewConeMarker',
    props: [
      'latlng',
      'rotationAngle',
      'hFov'
    ],
    render(h) {
      // this.orientation;
      return;
    },
    mounted() {
      const leafletElement = this.$leafletElement = this.createLeafletElement();
      // console.log('WHO IT IS', leafletElement);
      const map = this.$store.state.map.map;

      // REVIEW kind of hacky/not reactive?
      if (map) {
        leafletElement.addTo(map);
      }
    },
    destroyed() {
      // console.log('svgMarker destroyed fired, latlng is', this.latlng);
      this.$leafletElement._map.removeLayer(this.$leafletElement);
    },
    watch: {
      rotationAngle(nextRotationAngle) {
        // console.log('pngMarker orientation changed', nextRotationAngle);
        this.$leafletElement._map.removeLayer(this.$leafletElement);
        const leafletElement = this.$leafletElement = this.createLeafletElement();
        const map = this.$store.state.map.map;

        // REVIEW kind of hacky/not reactive?
        if (map) {
          leafletElement.addTo(map);
        }
      },
      latlng(nextLatLng) {
        // console.log('pngMarker orientation changed', nextRotationAngle);
        this.$leafletElement._map.removeLayer(this.$leafletElement);
        const leafletElement = this.$leafletElement = this.createLeafletElement();
        const map = this.$store.state.map.map;

        // REVIEW kind of hacky/not reactive?
        if (map) {
          leafletElement.addTo(map);
        }
      }
    },
    computed: {
      coneCoords() {
        const hFovDegrees = this.hFov * (180/3.14159265359);
        const scale = 50//options.scale;
        const angle = hFovDegrees / 2.0;
        const width = Math.sin(angle * Math.PI / 180);
        const length = Math.sqrt(1.0 - width * width);
        const coneCoords = [width * scale, length * scale];

        return coneCoords;
      },
    },
    methods: {
      createLeafletElement() {
        const coneCoords = this.coneCoords;
        const icon = new TriangleIcon({
          iconSize: L.point(this.coneCoords[0], this.coneCoords[1]),
          iconAnchor: [this.coneCoords[0] / 2, this.coneCoords[1]],
        });
        return L.marker(this.latlng, {
          icon: icon,
          rotationAngle: this.rotationAngle,
        });
      },
      parentMounted(parent) {
        const map = parent.$leafletElement;
        this.$leafletElement.addTo(map);
      },
    }
  };
</script>

<style>
  .svg-icon-noClick-svg {
    pointer-events: none;
  }

  .svg-icon-noClick {
    pointer-events: none;
  }
</style>
