<!-- <script context="module" lang="ts">
	export async function load({ page }) {
		console.log("slug")
	  return {
		props: {
		  slug: page.params.slug
		  
		},
	  };
	}
  </script> -->
<script>
	import { userStore } from '../../stores/api';
	import { config } from '../../variables';
	// import { Router, Route } from 'svelte-navigator';
	import {
		attach0,
		colors0,
		trademark0,
		colors0name,
		category0,
		submitbuttonpost,
		price0,
		colors0nameen
	} from '../../stores/api';

	// import { t } from 'svelte-intl-precompile';
	import { t, addMessages } from 'svelte-i18n';

	addMessages('tr', {"The title field must be between 2-125 chars!": "Başlık alanı 2-125 karakter arasında olmalıdır!", "The description field must be between 2-10000 chars!": "Açıklama alanı 2-10000 karakter arasında olmalıdır!"
});
	addMessages('en', {"The title field must be between 2-125 chars!": "The title field must be between 2-125 chars!", "The description field must be between 2-10000 chars!": "The description field must be between 2-10000 chars!"});

	// import {
	// 	register,
	// 	t,
	// 	init,
	// 	getLocaleFromNavigator,
	// 	isLoading,
	// 	addMessages,
	// 	locale,
	// 	locales
	// } from 'svelte-i18n';

	import Filepond from '$lib/filepond/filepond.svelte';

	import Trademarkauto from '../../../src/lib/Trademarkauto.svelte';
	import Categoryauto from '../../../src/lib/Categoryauto.svelte';

	import { Modals, closeModal, openModal, modals } from 'svelte-modals';
	import { fade } from 'svelte/transition';
	import Modal from './Modal.svelte';
	import Tags from 'svelte-tags-input';
	// import marked from 'marked';
	// import MediumEditor from 'medium-editor'

	var editor;

	function handleOpen() {
		openModal(Modal, {
			title: `#${$modals.length + 1}`,
			message: 'Color',
			onOpenAnother: () => {
				handleOpen();
			}
		});
	}
	// 	if (config.name == "aldolap") {
	// 		console.log("aldolap", config)
	// 	}

	console.log('config', config);

	// 	});

	let inputVal;
	function handleInput(e, y) {
		console.log(e.target.innerHTML);
		let yu = e.target.innerHTML;
		return e.target.innerHTML;
	}

	let title = '';
	let description = '';
	let userid = 0;
	let price = 0;
	let color = '';
	let trademark = '';

	let category = '';

	let username = '';
	let attach = '';
	let productstate = '';

	function asd(e) {
		console.log(e, 88);
	}

	// import EmojiSelector from 'svelte-emoji-selector';
	// import emojihere from '../../../static/svelte-emoji backup/emoji';
	// import totrans from '../../../static/svelte-emoji backup/totranslate';

	import { SvelteToast } from '@zerodevx/svelte-toast';
	import { toast } from '@zerodevx/svelte-toast';
	import autosize from 'autosize';

	import { onMount } from 'svelte';
	onMount(async () => {
		autosize(document.querySelectorAll('textarea'));
		var ta = document.querySelector('textarea');
		// ta.value = "Some new value";
		autosize.update(ta);
		const { default: MediumEditor } = await import('medium-editor');

		editor = new MediumEditor('.editable', {
			placeholder: {
				text: $t('Description'),
				hideOnClick: true
			},
			toolbar: {
				/* These are the default options for the toolbar,
           if nothing is passed this is what is used */
				allowMultiParagraphSelection: true,
				buttons: ['bold', 'italic', 'underline', 'anchor', 'h2', 'h3', 'quote'],
				diffLeft: 0,
				diffTop: -10,
				firstButtonClass: 'medium-editor-button-first',
				lastButtonClass: 'medium-editor-button-last',
				relativeContainer: null,
				standardizeSelectionStart: false,
				static: false,
				/* options which only apply when static is true */
				align: 'center',
				sticky: false,
				updateOnEmptySelection: false
			}
		});
	});

	trademark0.update((n) => []);
	category0.update((n) => []);
	price0.update((n) => '0');
	colors0.update((n) => []);

	function handleTags(event) {
		$category0 = [];
		$category0 = [event.detail.tags][0];
	}
</script>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<SvelteToast options={{ reversed: true, intro: { y: 192 } }} />

<Filepond />

<div class="new">
	{#if config.name === 'aldolap'}
		<div class="flex-container">
			<button on:click={handleOpen}>
				{$t('Color')}
			</button>
			<div class="chips-container">
				<span class="chips"
					><div
						class="selected"
						style="background-color:{$colors0}"
						alt="Color"
						width="96"
						height="96"
					/>

					{$t($colors0nameen.toString())}
					</span
				>
			</div>
		</div>
	{/if}

	<Modals>
		<div slot="backdrop" class="backdrop" transition:fade on:click={closeModal} />
	</Modals>

	{#if !$userStore}
		<a href="/login" style="color:red">{$t('Please login')} {$t('First')}: {$t('Here')}</a>
	{/if}

	<div class="left">
		<p>
			<strong>{$t('Title')}</strong>
		</p>
		<textarea bind:value={title} id="autosize" placeholder={$t('Title')} />
	</div>

	<p>
		<strong>{$t('Description')}</strong>
	</p>
	{#if config.name === 'aldolap'}
		<div class="left">
			<!-- <h1>Stuff</h1> -->
			<textarea bind:value={description} id="autosize" placeholder={$t('Description')} />
		</div>
	{/if}
	{#if config.name === 'bitfinicon'}
		<div id="container">
			<div class="editable" contenteditable="true" bind:innerHTML={description} />
		</div>
	{/if}

	{#if config.name === 'aldolap'}
		<div class="left">
			<p>
				<strong>{$t('Price')}</strong>
			</p>
			<input type="number" id="quantity"  bind:value={$price0} placeholder={$t('Price')} >
			<!-- <textarea bind:value={$price0} id="autosize" placeholder={$t('Price')} /> -->
		</div>
	{/if}

	{#if config.name === 'aldolap'}
		<p><strong>{$t('Trademark')}</strong></p>
		<Trademarkauto />
	{/if}

	<p><strong>{$t('Category')} {$category0}</strong></p>
	{#if config.name === 'aldolap'}
		<Categoryauto />
	{/if}

	{#if config.name === 'bitfinicon'}
		<Tags
			on:tags={handleTags}
			maxTags={5}
			onlyUnique={true}
			allowPaste={true}
			allowDrop={true}
			splitWith={','}
			placeholder={$t('Category')}
		/>
	{/if}

	<!-- <input bind:value={category} type="text" placeholder={$t('Category')}>
	<EmojiSelector on:emoji={onEmoji}/> -->

	<div class="my-3" style="margin-bottom: 4cm;">
		<button
			class="button--grey"
			on:click={() =>
				$userStore
					? submitbuttonpost(
							title,
							$price0,
							description,
							$userStore._id,
							$category0,
							$attach0,
							$colors0,
							$trademark0,
							$t('Success'),
							$t('Here'),
							$t('Click for the details'),
							$colors0nameen
					  ).then(x => x ? toast.push($t(String(x.message)), {
							theme: {
								'--toastBackground': '#F56565',
								'--toastBarBackground': '#C53030'
							}
					  }) : "")
					: toast.push($t('Failure') + '. ' + $t('Please login'), {
							theme: {
								'--toastBackground': '#F56565',
								'--toastBarBackground': '#C53030'
							}
					  })}
			type="submit">{$t('Submit')}</button
		>
	</div>
</div>

<style>
	@import 'medium-editor/dist/css/medium-editor.css';

	.left {
		display: flex;
		flex-direction: column;
	}

	* {
		box-sizing: border-box;
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
		background-color: #7b7ca9; /* Green */
		color: white;
	}

	.chips {
		display: inline-block;
		padding: 0 25px;
		height: 50px;
		font-size: 18px;
		line-height: 50px;
		border-radius: 25px;
		background-color: #f1f1f1;
	}

	.chips div {
		float: left;
		margin: 0 10px 0 -25px;
		height: 50px;
		width: 50px;
		border-radius: 50%;
	}

	.backdrop {
		position: fixed;
		top: 0;
		bottom: 0;
		right: 0;
		left: 0;
		background: rgba(0, 0, 0, 0.5);
	}

	* {
		box-sizing: border-box;
	}

	p strong {
		display: block;
	}

	[contenteditable] {
		color: black;
		background-color: rgb(255, 255, 255);
		padding: 0.5em;
		border: 1px solid #eee;
		border-radius: 4px;
	}
</style>
