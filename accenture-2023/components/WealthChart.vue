<script setup>
import * as Plot from "@observablehq/plot";
import PlotFigure from "./plotFigure.js";
import wealthMountainData from "./wealthMountainData.js";

let incomeXPositon = 111.6764514024788
let adjustedBelowMax = 920569295.5697684

let areaConfig = { curve: "natural", x: "x", y: "y", domain: [0,incomeXPositon] }
let areaEndConfig = { curve: "natural", x: "x", y: "y", fill: "#000000", stroke: "#fafafa", domain: [incomeXPositon, 4000000] }

let styleConfig = {
  background: "black",
  fontSize: "0.5rem",
  fontFamily: "ES Klarheit Grotesk",
  fill: "fafafa",
  width: 980,
  height: 400
};
let xConfig = {
  type: "log",
  domain: [1000, 4000000],
  insetRight: 0,
  transform: (dailyIncome) => dailyIncome * 365 * 10.5,
  label: "Årsinntekt i kroner (logaritmisk skala)",
};
let yConfig = {
  axis: null,
}

let lineConfig = {
  y: adjustedBelowMax,
}

let labelConfig = {
  lineWidth: 15,
  lineHeight: 1.5,
  textAnchor: "start",
  frameAnchor: "top",
  fontSize: 9,
  fontFamily: "ESKlarheitGrotesk",
  stroke: "#000000",
  fill: "#fafafa",
  paintOrder: "stroke",
  strokeWidth: 10,
  x: incomeXPositon,
  y: adjustedBelowMax,
  dx: 10,
}

let labelString = "En nordmann med medianinntekt er i dag blant de 2,4% rikeste i verden."

</script>

<template>
  <div class="chart-wrapper mt-10">
    <PlotFigure
      :options="{
        x: xConfig,
        y: yConfig,
        style: styleConfig,
        marks: [
          Plot.areaY(wealthMountainData, areaEndConfig),
          Plot.areaY(wealthMountainData, areaConfig),
          Plot.ruleX([incomeXPositon], lineConfig),
          Plot.text([labelString], labelConfig),
        ],
      }"
    />
  </div>
</template>