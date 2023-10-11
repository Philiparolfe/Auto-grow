<script>
	import { s1, s2, s3, s4, UV, s1Outline, s2Outline, s3Outline, s4Outline } from "../../store";
	/**
	 * @type {any}
	 */
	 /**
	 * @type {number}
	 */
	  export let switchNum;
     let switchNumForAPI = switchNum -1
	 /**
	 * @type {any}
	 */
	 let setval;
	 /**
	 * @type {import("svelte/store").Writable<any>}
	 */
	 let swOutline;
	 /**
	 * @type {string}
	 */
	 let url = "http://10.0.0.55:8080"
    $: state = null;
    $: outline = "";

	if (switchNum == 1){
		setval = s1
		swOutline = s1Outline
		s1Outline.subscribe((value) =>{
			outline = value;
		})
		s1.subscribe((value) => {
			state = value;
		});
	}
	else if(switchNum == 2){
		setval = s2
		swOutline = s2Outline
		s2Outline.subscribe((value) =>{
			outline = value;
		})
		s2.subscribe((value) => {
			state = value;
		});
	}
	else if (switchNum == 3){
		setval = s3
		swOutline = s3Outline
		s3Outline.subscribe((value) =>{
			outline = value;
		})
		s3.subscribe((value) => {
			state = value;
		});
	}
	else if (switchNum == 4){
		setval = s4
		swOutline = s4Outline
		s4Outline.subscribe((value) =>{
			outline = value;
		})
		s4.subscribe((value) => {
			state = value;
		});
	}


	async function on() {
		fetch(`${url}/on/${switchNumForAPI}`)
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
                //state = data
				setval.set(data);
                if (state == true){
					if(switchNum == 1){
						UV.set(' ease-in duration-300 bg-gradient-to-tl from-blue-500 to-purple-800 shadow-xl shadow-indigo-500/50 ');
					}
					
                    swOutline.set("variant-outline-primary");
                }
                
			})
			.catch((error) => {
				console.error(error);
			});
	}
    async function off() {
        if (state){
            fetch(`${url}/off/${switchNumForAPI}`)
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
                //state = data
				setval.set(data);
                if (state != true){
					if(switchNum == 1){
						UV.set('ease-out duration-700 variant-glass-tertiary');
					}
					
                    swOutline.set('')
                }
			})
			.catch((error) => {
				console.error(error);
			});
        }

	}
</script>

<div class="card {outline}">
	<h1 class="h4 text-center p-1">Switch {switchNum}</h1>
	<div class="flex justify-center gap-10 p-8">
		
		<div class="flex flex-col justify-center gap-3">
			<button id="on" class="btn variant-soft-primary" disabled={state} on:click={() => (on())}
				>ON</button
			>
			<button class="btn variant-soft-primary" disabled={!state} on:click={() => (off())}
				>OFF</button
			>
		</div>
	</div>

</div>


