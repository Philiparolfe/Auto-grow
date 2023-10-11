<script>
// 

	import { ProgressRadial } from '@skeletonlabs/skeleton';
	import { onMount } from 'svelte';
	import { humidity_switch } from '../../store';
	/**
	 * @type {?number}
	 */
	let humidity;
	/**
	 * @type {number}
	 */
	let temp
	/**
	 * @type {string}
	 */
	let humidity_switch_status;
	


	/**
	 * @type {string}
	 */
	let outline;
	const url = 'http://10.0.0.236:8080'

    $: state = null;
	$: humidity;
	$:humidity_switch_status;
	$: temp = 0;
	$: outline;

	humidity_switch.subscribe((value) =>{
		humidity_switch_status = value
		state = value
	})

    
	onMount(() => {
		status_helper()
		hum()
		const interval = setInterval(() => {
			status_helper()
			hum()
		}, 8000);

		return () => {
			clearInterval(interval);
		};
	});

	async function hum() {
		fetch(`${url}/hum`)
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
                
				humidity = parseInt(data[0])
				temp = parseInt(data[1])
			})
			.catch((error) => {
				console.error(error);
			});
	}

	async function on() {
		fetch(`${url}/on/0`)
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
                state = data
				outline_helper()
                
                
			})
			.catch((error) => {
				console.error(error);
			});
	}
    async function off() {
        if (state){
            fetch(`${url}/off/0`)
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
                state = data
				outline_helper()
			})
			.catch((error) => {
				console.error(error);
			});
        }

	}
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
				humidity_switch.set(data[0])
				outline_helper()
				//console.log(switches)
				
				
			})
			.catch((error) => {
				console.error(error);
			});
	}

	function outline_helper(){
		if (state == true){
			outline = "variant-outline-primary"
		}else{
			outline = ""
		}
	}

</script>
<div class="card {outline}">
    <h1 class="h4 text-center p-1 ">Humidity:</h1>
    <div class="flex justify-center gap-10 p-5">
        <ProgressRadial   value={humidity}>{humidity}%</ProgressRadial>
        <div class="flex flex-col justify-center gap-3 pr-5">
            <button id="on" class="btn variant-soft-primary" disabled={state} on:click={() => (on())}>ON</button>
            <button id="off" class="btn variant-soft-primary" disabled={!state}  on:click={() => (off())} >OFF</button>
        </div>
    </div>
	<h1 class="h5 text-center p-1">{temp}°C</h1>
</div>