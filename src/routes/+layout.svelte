<script>
	import Navbar from '$lib/Navbar.svelte';
	import '../app.css';
	import 'iconify-icon';
</script>

<Navbar />
<div class="bg-gray-300 relative">
	<div class="mx-4 min-h-screen">
		<slot />
	</div>
</div>
