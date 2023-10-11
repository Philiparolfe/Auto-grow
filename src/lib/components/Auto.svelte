<script>
    // @ts-nocheck
    
        import { onMount } from 'svelte';

        /**
         * @type {string}
         */
        let outline;
        const url = 'http://ladypi:8080'
    
        $: state = null;

        $: outline;
    
        
        /*onMount(() => {
            
            const interval = setInterval(() => {
                //console.log("hello")
            }, 10000);
    
            return () => {
                clearInterval(interval);
            };
        });*/
    
    
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
                    outline = "variant-outline-primary"
                    
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
                    outline = ""
                })
                .catch((error) => {
                    console.error(error);
                });
            }
    
        }
    
    
    </script>

<div class="card {outline}">
	<h1 class="h4 text-center p-1">Auto-grow:</h1>
	<div class="flex justify-center gap-10 p-3">
		
		<div class="flex flex-row justify-center gap-3">
			<button id="on" class="btn variant-soft-primary" disabled={state} on:click={() => (on())}
				>ON</button
			>
			<button class="btn variant-soft-primary" disabled={!state} on:click={() => (off())}
				>OFF</button
			>
		</div>
	</div>

</div>


