<script lang="ts">
	import '@fontsource-variable/raleway';
	import { Toaster } from 'svelte-french-toast';
</script>

<title> Website </title>

<slot />
<Toaster />

<style lang="scss">
	:global(*) {
		margin: 0;
		box-sizing: border-box;
	}

	:global(body) {
		margin: 0;
		height: 100vh;
		width: 100%;
		background: #eee;
		font-family: 'Raleway Variable', sans-serif;
	}
</style>
