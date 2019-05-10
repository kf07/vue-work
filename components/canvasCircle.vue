<template>
  <div>
    <v-stage ref="stage" :config="configKonva">
      <v-layer>
        <v-circle ref="circle" :config="configCircle" />
      </v-layer>
    </v-stage>
    <p>{{ configCircle.fill }}</p>
    <p>{{ colorHsl }}</p>
    <p>{{ colorHex }}</p>
  </div>
</template>

<script>
import tinycolor from 'tinycolor2'
export default {
  name: 'Circles',
  data() {
    return {
      configKonva: {
        width: 500,
        height: 500
      },
      configCircle: {
        x: 100,
        y: 100,
        radius: 30,
        fill: ''
      },
      colorsHsl: {
        h: 0,
        s: 100,
        l: 50
      },
      center: {
        x: 250,
        y: 250
      }
    }
  },
  computed: {
    colorObject: function() {
      return tinycolor(
        `hsl(${this.colorsHsl.h},${this.colorsHsl.s},${this.colorsHsl.l}`
      )
    },
    colorHex: function() {
      return this.colorObject.toHexString()
    },
    colorHsl: function() {
      return this.colorObject.toHslString()
    }
  },
  watch: {
    colorsHsl: {
      handler: function() {
        this.configCircle.fill = this.colorObject.toRgbString()
      },
      deep: true
    }
  },
  mounted() {
    let x = this.center.x
    let y = this.center.y
    const distanceFromCenter = 150
    const interval = 10
    let angleRad = 0
    setInterval(() => {
      if (this.colorsHsl.h <= 360) {
        this.colorsHsl.h = this.colorsHsl.h + 1
      } else {
        this.colorsHsl.h = 0
      }
    }, 50)
    setInterval(() => {
      angleRad += (1 * Math.PI) / 180
      // distanceFromCenter = distanceFromCenter + 1 / 20
      x = distanceFromCenter * Math.cos(angleRad) + this.center.x
      y = distanceFromCenter * Math.sin(angleRad) + this.center.y
      this.configCircle.x = x
      this.configCircle.y = y
      // interval = interval + 1 / 5
    }, interval)
  },
  methods: {}
}
</script>

<style scoped>
p {
  margin: 0 0 8px;
}
</style>
