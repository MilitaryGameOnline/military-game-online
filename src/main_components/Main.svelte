<script>
	import Header from '../main_components/Header.svelte';
	import Footer from '../main_components/Footer.svelte';
</script>

<div class='main'>
<Header />
	<div class='content'>
		<slot />
	</div>
</div>
<Footer />

<style>
	.main {
      min-height: 100vh;
	}
	.content{
			padding: 5px 20px;
	}

	:global(body) {
			margin: 0px;
	}
</style>