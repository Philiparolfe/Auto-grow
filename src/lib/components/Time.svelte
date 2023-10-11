<script>
	import { onMount } from "svelte";

// Create a new Date object
let time = new Date();

// Create an options object with the desired time zone
const options = { timeZone: 'America/New_York' };

// Format the date and time in the Eastern Time zone

$: hours = time.getHours();
$: minutes = time.getMinutes();
$: seconds = time.getSeconds();
$: currentDate = time.toLocaleDateString('en-US', options)




onMount(() => {
		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>
<section class="flex w-full flex-row justify-between place-content-between">
    <h1>{currentDate}</h1>
	<h1>Grow-Box Dashboard</h1>
	<h1>{hours}:{minutes}</h1>
</section>


