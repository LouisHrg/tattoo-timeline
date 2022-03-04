<script>
  import Logo from "./svg/Logo.svelte";
  import Front from "./svg/Front.svelte";
  import Sessions from './constants/front-sessions.js';

  import { onMount } from 'svelte';

  import "milligram/dist/milligram.min.css";

  import cssVars from 'svelte-css-vars';
  import RangeSlider from "svelte-range-slider-pips";

  $: styleVars = {
    'range-handle': '#96281b',
    'range-handle-focus': '#000',
    'range-slider': "#000",
    "range-handle-inactive": "#a6aab9"
  };

  let bound = [1];
  let parts = null;

  $: if(bound) {
    const sessions = Sessions.filter(el => el.step <= bound[0]);

    parts = sessions.flatMap(el => el.parts);
  }

  $: if(parts) {
    if(document.readyState === "complete") {
      resetZones();
      mapZones();
    }
  }

  function mapZones () {
      parts.forEach(el => {
        document.getElementById(el.id).style.fill = el.color;
        document.getElementById(el.id).style.stroke = el.color;
      });
  }

  function resetZones () {
    const classes = document.querySelectorAll('path');
    classes.forEach(el => el.style = '#FFF');
  }

  onMount(async () => mapZones());

</script>

<svelte:head>
  <style> body { background: #22252a; } </style>
</svelte:head>


<div use:cssVars="{styleVars}" class="container">
  <div class="row">
		<h4> <Logo /> tttimeline </h4>
  </div>
    <Front />
    <RangeSlider bind:values={bound} pips id="reverse-pips" min={1} max={4}/>

  <p style="text-align:center; color:#cdd1e4"> Tattoo sesh #{bound}</p>
</div>

<style>
  .button-black {
    background-color: black;
    border-color: black;
  }

</style>
