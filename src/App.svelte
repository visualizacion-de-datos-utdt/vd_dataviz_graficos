<script>
  import * as d3 from "d3"

  import { writable, derived } from "svelte/store";
  import {LayerCake, Svg} from "layercake"

  import Line from "./_components/Line.svelte"
  import Area from "./_components/Area.svelte"
  import AxisX from "./_components/AxisX.svelte"
  import AxisY from "./_components/AxisY.svelte"

  // This example loads csv data as json using @rollup/plugin-dsv
  import data from "./_data/points.csv"

  const xKey = "myX"
  const yKey = "myY"

  let year = writable(2015);

  const filteredData = derived(year, $year => {
    return data.filter(d => d[xKey] <= $year);
  });

</script>

<main>
  <div class="header">
    <h3 class="headline">
      <b>Gráfico interactivo</b>
    </h3>
    <p class="bajada">Líneas y áreas con slider de selección</p>
    <div class="input-container">
      <input
        type="range"
        min="1985"
        max="2015"
        step="1"
        bind:value={$year}
      /> 
      <span class="counter-container"><i>Mostrar hasta</i> <b>{$year}</b></span>
    </div>
  </div>

  <div class="chart-container">
    <LayerCake
      padding={{ top: 8, right: 10, bottom: 20, left: 25 }}
      x={xKey}
      y={yKey}
      yDomain={[0, null]}
      data={$filteredData}
    >
      <Svg>
        <AxisX
          ticks={4}
        />
        <AxisY
          ticks={4}
        />
        <Line
          stroke = "#38538A"
        />
        <Area
          fill = "#38538A20"
        />
      </Svg>
      
    </LayerCake>
  </div>
</main>

<style>

.header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    margin-bottom: 30px;
  }
  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px 0 0 0;
  }
  .bajada {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 10px 0 30px 0;
  }
  .headline b {
    display: block;
  }

  .chart-container {
    width: 100%;
    height: 500px;
  }

  .input-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 10px;
    width: 100%;
  }

  .input-container input {
    margin: 0 10px;
    -webkit-appearance: none;
    appearance: none;
    width: 200px;
    height: 5px;
    background: #ddd;
    outline: none;
    opacity: 0.7;
  }

  .input-container input::-webkit-slider-runnable-track {
    width: 100%;
    height: 5px;
    cursor: pointer;
    background: #38538A50;
  }

  .input-container input::-webkit-slider-thumb {
    border: 1px solid #000000;
    height: 15px;
    width: 15px;
    border-radius: 50%;
    background: #38538A;
    cursor: pointer;
    -webkit-appearance: none;
    margin-top: -5px;
  }
</style>