<script>
	import { UV, s1, s1Outline} from '../../store';
	$: color = 'variant-glass-tertiary';
	$: state = null;
	s1.subscribe((value) => {
		state = value;
	});
	UV.subscribe((value) =>{
		color = value
	})
	async function on() {
		fetch('http://10.0.0.55:8080/on/0')
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
				//state = data;
				s1.set(data);
				if (state == true){
					UV.set(' ease-in duration-300 bg-gradient-to-tl from-blue-500 to-purple-800 shadow-xl shadow-indigo-500/50 ');
					s1Outline.set("variant-outline-primary")
				}

				
			})
			.catch((error) => {
				console.error(error);
			});
	}
	async function off() {
		if (state) {
			fetch('http://10.0.0.55:8080/off/0')
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
					//state = data;
					s1.set(data);
					if (state != true){
						UV.set('ease-out duration-700 variant-glass-tertiary');
						s1Outline.set('')
					}

					
				})
				.catch((error) => {
					console.error(error);
				});
		}
	}
</script>

<div class="card">
	<h1 class="h4 text-center p-1">UV Light:</h1>
	<div class="flex justify-center gap-10 p-5">
		<div id="onStatus" class="w-36 h-36 {color} rounded-full" />
		<div class="flex flex-col justify-center gap-3 pr-5">
			<button id="on" class="btn variant-soft-primary" disabled={state} on:click={() => on()}
				>ON</button
			>
			<button class="btn variant-soft-primary" disabled={!state} on:click={() => off()}>OFF</button>
		</div>
	</div>
</div>
