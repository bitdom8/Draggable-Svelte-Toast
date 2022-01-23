<script lang="ts">
	export let message;

	// import Pickr from '@simonwep/pickr';
	// import Pickr from '@simonwep/pickr/dist/pickr.es5.min';
	import { t } from 'svelte-i18n';
	// import { t } from 'svelte-intl-precompile';

	import { ntc } from '../../../static/colorblindnesshex/ntc';
	import trcolor from '../../../locales/trcolor';

	import { colord } from 'colord';

	import { HsvPicker } from 'svelte-color-picker';
	import { colors0, colors0name, colors0nameen } from '../../stores/api';
	// import Draggable from './Draggable.svelte';



	let colorname = '';
	let colornametranslate = '';
	let colornameresult = '';
	let colornameresultname = '';

	function colorCallback(rgba) {
		let a = rgba.detail.r;
		let b = rgba.detail.g;
		let c = rgba.detail.b;
		let hex = colord(`rgb(${a}, ${b}, ${c})`).toHex();

		colorname = ntc.name(hex)[1];
		colornameresult = ntc.name(hex)[0];

		colors0nameen.update((n) => []);
		$colors0nameen = [...$colors0nameen, colorname];

		// for(var i = 0; i < trcolor.names.length; i++)
		//   {
		//     console.log(colorname, "tr", trcolor.names)
		// }
		trcolor.names.map(function gu(x) {
			if ('#' + x[0] == colornameresult) {
				// console.log("#" + x[0], "buldum ", colornameresult)
				colornameresultname = x[1];
				return (colornametranslate = x[1]);
			}
		});
		colors0.update((n) => []);
		colors0name.update((n) => []);

		$colors0 = [...$colors0, colornameresult];
		$colors0name = [...$colors0name, colornameresultname];
	}

	import { closeModal, modals } from 'svelte-modals';
	import { fly } from 'svelte/transition';

	export let isOpen;
	export let title;
	//   export let message
	// export let onOpenAnother

	let stackIndex = $modals.length;
</script>

{#if isOpen}
	<div
		role="dialog"
		class="modal"
		transition:fly={{ y: 50 }}
		on:introstart
		on:outroend
		style="z-index: 666666;"
	>
		<!-- <Draggable> -->
		<div class="card">
			<h2>{$t(title)}</h2>

			<h1>🎉 {$t(message)}</h1>

			<p class="chips-container">
				<HsvPicker on:colorChange={colorCallback} startColor={$colors0[0]} />
			</p>
			<!-- <p>{colornametranslate} ( {colorname} )</p> -->
			{#if $colors0.length >0}
			<p>{$t($colors0[0].toString().substring(1))}</p>
			{/if}
			
			

			<div class="actions">
				{#if stackIndex > 1}
					<button on:click={closeModal}>{$t('Save')}</button>
				{:else}
					<button on:click={closeModal}>{$t('Close')}</button>
				{/if}
			</div>
		</div>
		<!-- </Draggable>	 -->
	</div>
{/if}

<style scoped>
	h1 {
		margin-top: 2px;
		margin-bottom: 2px;
		font-size: 2rem;
		text-align: center;
	}

	.modal {
		position: fixed;
		top: calc(50% - 220px);
		left: calc(50% - 140px);
		/* bottom: calc(50% - 50px); */
		z-index: 1;
	}

	h2 {
		text-align: center;
		font-size: 10px;
		margin-bottom: 0px;
	}

	.actions {
		margin-top: 32px;
		display: flex;
		justify-content: space-between;
		gap: 8px;
	}
</style>
