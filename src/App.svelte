<script>
  import Logo from "./svg/Logo.svelte";
  import Front from "./svg/Front.svelte";
  import Back from "./svg/Back.svelte";
  import Caption from "./components/Caption.svelte";
  import Sessions from './constants/sessions.js';

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

  let bound = [0];
  let cumulative = false;
  let parts = null;

  $: if(bound) {
    if(cumulative) {
      const sessions = Sessions.filter(el => el.step <= bound[0]);
      parts = sessions.flatMap(el => el.parts);
    } else {
      const session = Sessions.find(el => el.step === bound[0]);
      parts = session.parts;
    }
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
  <div class="row">
    <div class="column" style="text-align: center;">
      <Front />
      <h5 style="margin-top:20px;">Front</h5>
    </div>
    <div class="column" style="text-align: center;">
      <Back />
      <h5 style="margin-top:20px">Back</h5>
    </div>
  </div>
  <RangeSlider bind:values={bound} pips id="reverse-pips" min={0} max={4}/>
  {#if bound > 0}
  <p style="text-align:center; color:#cdd1e4"> Tattoo session #{bound}</p>
  {:else}
  <p style="text-align:center; color:#cdd1e4"> Slide to see the evolution </p>
  {/if}
  <label style="text-align: center;">
    <input type=checkbox bind:checked={cumulative}>
    Make the sessions overlap
  </label>
  <Caption />
</div>

<style>
  .button-black {
    background-color: black;
    border-color: black;
  }

</style>
