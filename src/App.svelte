<script>
  import RangeSlider from "svelte-range-slider-pips";
  import Front from "./svg/Front.svelte";
  import Sessions from './constants/front-sessions.js';
  import { onMount } from 'svelte';

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
<div style="width:600px;margin: auto; display:block;">
		<Front />
<RangeSlider bind:values={bound} pips min={1} max={4}/>

<p style="text-align:center"> Tattoo sesh #{bound}</p>
</div>

