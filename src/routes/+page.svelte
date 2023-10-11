<script>
	import Auto from '$lib/components/Auto.svelte';
	import Humidity from '$lib/components/Humidity.svelte';
	import Relay from '$lib/components/Relay.svelte';
	import Time from '$lib/components/Time.svelte';
	import UVlight from '$lib/components/UVlight.svelte';
	import { onMount } from 'svelte';
	import {
		switchStatus,
		s1,
		s2,
		s3,
		s4,
		s1Outline,
		s2Outline,
		s3Outline,
		s4Outline,
		UV
	} from '../store';

		/**
	 * @type {boolean}
	 */
	let loading = false;
	const url = 'http://webserver:8080';

	/**
	 * @type {Array<boolean>}
	 */
	let switches;

	let switchStores = [s1, s2, s3, s4];
	let switchStoresOutlines = [s1Outline, s2Outline, s3Outline, s4Outline];

	$: switches;
	switchStatus.subscribe((value) => {
		switches = value;
	});

	onMount(() => {
		status_helper();
		const interval = setInterval(() => {
			status_helper();
		}, 5000);

		return () => {
			clearInterval(interval);
		};
	});

	async function status_helper() {
		fetch(`${url}/init`)
			.then((response) => {
				if (response.ok) {
					return response.json(); // Parse the response as JSON
				} else {
					throw new Error('Request failed');
				}
			})
			.then((data) => {
				// Handle the data returned from the Flask endpoint
				//console.log(data); // Data is now available for
				switchStatus.set(data);
				//console.log(switches)
				[0, 1, 2, 3].forEach((i) => {
					switchStores[i].set(switches[i]);
					//console.log(switches[i])
				});
				[0, 1, 2, 3].forEach((i) => {
					if (switches[i] == true) {
						if (i == 0) {
							UV.set(
								' ease-in duration-300 bg-gradient-to-tl from-blue-500 to-purple-800 shadow-xl shadow-indigo-500/50 '
							);
						}
						[s1Outline, s2Outline, s3Outline, s4Outline][i].set('variant-outline-primary');
					} else {
						if (i == 0) {
							UV.set('ease-out duration-700 variant-glass-tertiary');
						}
						[s1Outline, s2Outline, s3Outline, s4Outline][i].set(
							'ease-out duration-700 variant-glass-tertiary'
						);
					}
				});
			})
			.catch((error) => {
				console.error(error);
			});
	}
</script>

<section
	class="flex h-10 p-5 sm:px-5 md:px-5 lg:px-10 pb-10 shadow-md mb-5 dark:bg-surface-100-800-token"
>
	<Time />
</section>

<main class="p-5">
	{#if loading}
		<section class="card w-full mb-5">
			<div class="p-4 space-y-4">
				<div class="placeholder animate-pulse" />
				<div class="grid grid-cols-3 gap-8 animate-pulse">
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
				</div>
				<div class="grid grid-cols-4 gap-4 animate-pulse">
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
				</div>
			</div>
		</section>
		<section class="card w-full">
			<div class="p-4 space-y-4">
				<div class="placeholder animate-pulse" />
				<div class="grid grid-cols-3 gap-8 animate-pulse">
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
				</div>
				<div class="grid grid-cols-4 gap-4 animate-pulse">
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
					<div class="placeholder" />
				</div>
			</div>
		</section>
	{:else}
		<div class="flex flex-col md:flex-row lg:flex-row gap-10 justify-center">
			<Humidity />
			<UVlight />
		</div>
		<div class="flex flex-col md:flex-row lg:flex-row gap-10 justify-center p-5">
			<Relay switchNum="1" />
			<Relay switchNum="2" />
			<Relay switchNum="3" />
			<Relay switchNum="4" />
		</div>
		<div class="flex flex-col md:flex-row lg:flex-row gap-10 justify-center p-5">
			<Auto switchNum="2" />
		</div>
	{/if}
</main>
