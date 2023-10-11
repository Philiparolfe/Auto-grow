<script>
	import '../app.postcss';
	import { LightSwitch } from '@skeletonlabs/skeleton';

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });
</script>
<section class="">
	<slot />

</section>
<footer class="flex justify-center items-center p-5">
	<LightSwitch />
</footer>
