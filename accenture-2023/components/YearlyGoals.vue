<script setup>
import * as Plot from "@observablehq/plot";
import PlotFigure from "./plotFigure.js";
import * as TWEEN from '@tweenjs/tween.js';
import { ref } from "vue";

let yearlySums = [
  {
    year: "2016",
    value: 440439,
  },
  {
    year: "2017",
    value: 1811173,
  },
  {
    year: "2018",
    value: 2335252
  },
  {
    year: "2019",
    value: 3786522
  },
  {
    year: "2020",
    value: 6426070
  },
  {
    year: "2021",
    value: 12274641
  },
  {
    year: "2022",
    value: 17712204
  }
]

const barYConfig = {
  x: "year",
  y: "value",
  sort: {x: "x"},

}

const xConfig = {
  label: null,
  type: "band"
}

let yConfig = {
  label: "Kroner samlet inn per år",
  tickFormat: (nok) => `${nok / 1000000} mill.`
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
    Plot.barY(yearlySums, barYConfig)
  ],
})


</script>

<template>
  <div class="chart-wrapper mt-10">
    <PlotFigure
      :options="options"
    />
  </div>
</template>