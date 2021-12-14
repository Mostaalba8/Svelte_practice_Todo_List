<script>
	import Header from "./components/Header.svelte";
	import List from "./components/List.svelte";
</script>

<style>
	.container{
		width: 500px;
		background: #8132F0;
	}

	:global(.container*){
		font-family: sans-serif;
	}
</style>

<div class="container">
	<Header/>
	<List/>
</div>