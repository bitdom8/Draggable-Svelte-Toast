<script lang="ts">
	let message;

	// import Pickr from '@simonwep/pickr';
	// import Pickr from '@simonwep/pickr/dist/pickr.es5.min';
	import { t } from 'svelte-i18n';
	// import { t } from 'svelte-intl-precompile';

	import { ntc } from '../../../static/colorblindnesshex/ntc';
	import trcolor from '../../../locales/trcolor';
	import { config } from '../../variables';
	import { colord } from 'colord';

	let modaleditcomp2;
	
	import {
		colors0,
		colors0name,
		title0,
		description0,
		price0,
		category0,
		attach0,
		trademark0,
		userid0,
		postid0,
		submitbuttonupdate,
		colors0nameen
	} from '../../stores/api';
	// import Draggable from './Draggable.svelte';
	import Trademarkauto from '../../../src/lib/Trademarkauto.svelte';
	import Categoryauto from '../../../src/lib/Categoryauto.svelte';
	

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
		// console.log(colornameresult, "uuu")

		// for(var i = 0; i < trcolor.names.length; i++)
		//   {
		//     console.log(colorname, "tr", trcolor.names)
		// }
		trcolor.names.map(function gu(x) {
			// console.log(x[0], 22,colornameresult )
			if ('#' + x[0] == colornameresult) {
				// console.log('#' + x[0], 'buldum ', colornameresult);
				colornameresultname = x[1];
				return (colornametranslate = x[1]);
			}
		});
		colors0.update((n) => []);
		$colors0 = [...$colors0, colornameresult];

		colors0name.update((n) => []);
		$colors0name = [...$colors0name, colornameresultname];
	}

	import { closeModal, closeAllModals, openModal, modals } from 'svelte-modals';

	import { fly } from 'svelte/transition';
	import Filepond from '$lib/filepond/filepond.svelte';
	import Tags from 'svelte-tags-input';
	// export let onOpenAnother;

	import autosize from 'autosize';


	import Modal from './Modal.svelte';
	// let Modal;

	let stackIndex = $modals.length;

	function handleOpen() {
		modaleditcomp2 = true
		
		openModal(Modal, {
			title: `#${$modals.length + 1}`,
			message: 'Color',
			onOpenAnother: () => {
				handleOpen();
			}
		});
	}
	// let editor

	import { onMount } from 'svelte';
	onMount(async () => {
		autosize(document.querySelectorAll('textarea'));
		var ta = document.querySelector('textarea');
		// ta.value = "Some new value";
		autosize.update(ta);

	
	});

	export let isOpen;


	let tags = $category0;

	function handleTags(event) {
		// tag = event.detail.tags;
		$category0 = [];
		$category0 = [event.detail.tags][0];
	}
</script>



{#if isOpen}
	<div
		role="dialog"
		transition:fly={{ y: 50 }}
		on:introstart
		on:outroend
		style="z-index: 5; position:  fixed;
top: calc(0% );
left: calc(0%);
z-index: 11;"
	>
		<!-- <Draggable> -->
		<div class="contents">
			<Filepond />


			{#if config.name === 'aldolap'}
				<div class="flex-container">
					<button on:click={handleOpen}>
						{$t('Color')}
					</button>
					<div class="chips-container" />
				</div>
			{/if}

			<div class="actions">
					<button
						on:click={closeModal}
						on:click={() => submitbuttonupdate(
							$title0,
							$price0,
							$description0,
							$userid0,
							$category0,
							$attach0,
							$colors0,
							$trademark0,
							$postid0,
							$t('Success'),
							$colors0nameen
						)}>{$t('Close')}</button
					>
			</div>

			<p>
				<strong>{$t('Title')}</strong>
			</p>
			<textarea
				id="autosize"
				style="white-space: pre-wrap;"
				bind:value={$title0}
				placeholder={$t('Title')}
			/>

			<p>
				<strong>{$t('Description')}</strong>
			</p>
			{#if config.name === 'aldolap'}
				<textarea
					style="white-space: pre-wrap;"
					placeholder={$t('Description')}
					bind:value={$description0}
				/>
			{/if}
			{#if config.name === 'bitfinicon'}
				<div id="container">
					<div class="editable" contenteditable="true" bind:innerHTML={$description0} />
				</div>
			{/if}

			<!-- APPLYCHANGE DONE ALDOLAP -->
			{#if config.name === 'aldolap'}
				<div class="left">
					<p>
						<strong>{$t('Price')}</strong>
					</p>
					<input type="number" id="quantity"  bind:value={$price0} placeholder={$t('Price')} >
					<!-- <textarea bind:value={$price0} id="autosize" placeholder={$t('Price')} /> -->
				</div>

				<p>
					<strong>{$t('Trademark')}</strong>
				</p>
				<Trademarkauto />
			{/if}

			<p>
				<strong>{$t('Category')}</strong>
			</p>
		
			{#if config.name === 'aldolap'}
				<Categoryauto />
			{/if}
			
			{#if config.name === 'bitfinicon'}
				<Tags
					{tags}
					on:tags={handleTags}
					maxTags={5}
					onlyUnique={true}
					allowPaste={true}
					splitWith={','}
				/>
			{/if}
		</div>
		<!-- </Draggable> -->
	</div>
{/if}

<style scoped>
	.left {
		display: flex;
		flex-direction: column;
	}

	.flex-container {
		display: flex;
		flex-direction: row;
		font-size: 20px;
		text-align: center;
	}

	@media (max-width: 200px) {
		.flex-container {
			flex-direction: column;
		}
	}

	button {
		background-color: #7b6ca3;
		border-radius: 8px;
	}

	button:hover {
		background-color: #7b7ca9;
		color: white;
	}

	* {
		box-sizing: border-box;
	}

	p strong {
		display: block;
	}

	.contents {
		z-index: 1;
		min-width: 310px;
		max-width: 310px;
		background: var(--card-bg2);
		position: relative;
		padding: 0.8rem;
		max-height: calc(95vh - 4rem);
		overflow: auto;
	}

	.actions {
		margin-top: 22px;
		display: flex;
		justify-content: space-between;
		gap: 8px;
	}
</style>
