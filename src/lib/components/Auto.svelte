<script>
	// @ts-nocheck

	import { onMount } from 'svelte';
	import { autoStatus } from '../../store';

	/**
	 * @type {string}
	 */
	let outline;
	const url = 'http://10.0.0.236:8080';

	$: state = null;

	$: outline;

	autoStatus.subscribe((value) => {
		state = value;
	});

	/*onMount(() => {
            
            const interval = setInterval(() => {
                //console.log("hello")
            }, 10000);
    
            return () => {
                clearInterval(interval);
            };
        });*/
	onMount(() => {
		status_helper();

		const interval = setInterval(() => {
			status_helper();
		}, 8000);

		return () => {
			clearInterval(interval);
		};
	});

	async function on() {
		fetch(`${url}/auto/1`)
			.then((response) => {
				if (response.ok) {
					return response.json(); // Parse the response as JSON
				} else {
					throw new Error('Request failed');
				}
			})
			.then((data) => {
				// Handle the data returned from the Flask endpoint
				//console.log(data); // Data is now available for use
				status_helper()
				console.log(data)
			})
			.catch((error) => {
				console.error(error);
			});
	}
	async function off() {
		if (state) {
			fetch(`${url}/auto/0`)
				.then((response) => {
					if (response.ok) {
						return response.json(); // Parse the response as JSON
					} else {
						throw new Error('Request failed');
					}
				})
				.then((data) => {
					// Handle the data returned from the Flask endpoint
					//console.log(data); // Data is now available for use
					status_helper()
                    console.log(data)
					
				})
				.catch((error) => {
					console.error(error);
				});
		}
	}

	async function status_helper() {
		fetch(`${url}/autoStatus`)
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
				autoStatus.set(data);
				outline_helper();
				//console.log(switches)
			})
			.catch((error) => {
				console.error(error);
			});
	}

	function outline_helper() {
		if (state == true) {
			outline = 'variant-outline-primary';
		} else {
			outline = '';
		}
	}
</script>

<div class="card {outline}">
	<h1 class="h4 text-center p-1">Auto-grow:</h1>
	<div class="flex justify-center gap-10 p-3">
		<div class="flex flex-row justify-center gap-3">
			<button id="on" class="btn variant-soft-primary" disabled={state} on:click={() => on()}
				>ON</button
			>
			<button class="btn variant-soft-primary" disabled={!state} on:click={() => off()}>OFF</button>
		</div>
	</div>
</div>
