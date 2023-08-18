<script setup>
import * as Plot from "@observablehq/plot";
import PlotFigure from "./plotFigure.js";
import * as TWEEN from '@tweenjs/tween.js';
import { ref } from "vue";

let interventionData = [
  {
    intervention: "Stipend",
    value: 0,
  },
  {
    intervention: "Gratis skoleuniformer",
    value: 0,
  },
  {
    intervention: "Behandling av tropesykdommer",
    value: 0
  }
]

const barYConfig = {
  x: "intervention",
  y: "value",
  sort: {x: "-x"}
}

const xConfig = {
  label: null,
  ticks: 0
}

let yConfig = {
  label: "Ekstra skoleår per ~1000 kroner brukt",
  domain: [0, 0.22],
}

let styleConfig = {
  background: "black",
  fontSize: "0.5rem",
  fontFamily: "ES Klarheit Grotesk",
  fill: "#fafafa",
  width: 980,
  height: 400
};

let options = ref({
  x: xConfig,
  y: yConfig,
  style: styleConfig,
  marks: [
    Plot.ruleY([0]),
    Plot.barY(interventionData, barYConfig)
  ],
})

let clickCount = ref(0)

const click = () => {
  if (clickCount.value == 0) {
    clickCount.value++
    const tween = new TWEEN.Tween({stipend: 0, uniformer: 0})
      .to({stipend: 0.09, uniformer: 0.12 }, 1000)
      .easing(TWEEN.Easing.Quadratic.Out)
      .onUpdate((object) => {
        interventionData = [
          {
            intervention: "Stipend",
            value: object.stipend,
          },
          {
            intervention: "Gratis skoleuniformer",
            value: object.uniformer,
          },
          {
            intervention: "Behandling av tropesykdommer",
            value: 0
          }
        ]

        options.value = {
          x: xConfig,
          y: yConfig,
          style: styleConfig,
          marks: [
            Plot.ruleY([0]),
            Plot.barY(interventionData, barYConfig)
          ],
        }
      })
      .start()
    
    function animate(time) {
      tween.update(time)
      requestAnimationFrame(animate)
    }
    requestAnimationFrame(animate)
  } else {
    const tween = new TWEEN.Tween({ymax: 0.22, ntds: 0})
      .to({ymax: 14, ntds: 12.1 }, 5000)
      .easing(TWEEN.Easing.Quadratic.InOut)
      .onUpdate((object) => {
        interventionData = [
          {
            intervention: "Stipend",
            value: 0.09,
          },
          {
            intervention: "Gratis skoleuniformer",
            value: 0.12,
          },
          {
            intervention: "Behandling av tropesykdommer",
            value: object.ntds
          }
        ]

        options.value = {
          x: xConfig,
          y: {
            label: "Ekstra skoleår per ~1000 kroner brukt",
            domain: [0, object.ymax],
          },
          style: styleConfig,
          marks: [
            Plot.ruleY([0]),
            Plot.barY(interventionData, barYConfig)
          ],
        }
      })
      .start()
    
    function animate(time) {
      tween.update(time)
      requestAnimationFrame(animate)
    }
    requestAnimationFrame(animate)
  }
}
</script>

<template>
  <div 
    class="chart-wrapper mt-10"
    :onClick="click">
    <PlotFigure
      :options="options"
    />
  </div>
</template>