

<script lang="ts">
  export let message;

  // import Pickr from '@simonwep/pickr';
// import Pickr from '@simonwep/pickr/dist/pickr.es5.min';
import { addMessages, t } from 'svelte-i18n';
// import { t } from 'svelte-intl-precompile';
import {ntc}  from '../../../static/colorblindnesshex/ntc'
import trcolor  from '../../../locales/trcolor'

import { colord } from 'colord';

import {HsvPicker} from 'svelte-color-picker';
import { colors0, colors0name } from '../../stores/api';
import Draggable from './Draggable.svelte';

let colorname = ""
let colornametranslate = ""
let colornameresult = ""
let colornameresultname = ""

function colorCallback(rgba) {

  let a = rgba.detail.r
  let b = rgba.detail.g
  let c = rgba.detail.b
  let hex = colord(`rgb(${a}, ${b}, ${c})`).toHex()

  colorname = ntc.name(hex)[1]
  colornameresult = ntc.name(hex)[0]
  // console.log(colornameresult, "uuu")

    // for(var i = 0; i < trcolor.names.length; i++)
    //   {
    //     console.log(colorname, "tr", trcolor.names)
    // }
    trcolor.names.map(
      function gu(x) {
        // console.log(x[0], 22,colornameresult )
        if ("#" + x[0] == colornameresult) {
          
          console.log("#" + x[0], "buldum ", colornameresult)
          colornameresultname = x[1]
          return colornametranslate = x[1]
        }
      }
    )
    colors0.update((n) => []);
    colors0name.update((n) => []);
    $colors0 = [...$colors0, colornameresult];
    $colors0name = [...$colors0name, colornameresultname];

}

</script>

<style>
  h1 {
		font-size: 2rem;
		text-align: center;
	}

</style>


<h1>🎉 {message} </h1>

<HsvPicker on:colorChange={colorCallback} startColor={"#FBFBFB"}/>


<!-- {$t(colorname)} 77 -->
{colornametranslate} ( {colorname} )


	<h1>
		Drag Me
	</h1>
