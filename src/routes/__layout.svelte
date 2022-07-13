<script>
	import '../app.css';

	
</script>

<div id="theme-container">
	<div id="app-content">

		<main class="flex flex-grow flex-col items-center justify-center">
			<slot />
		</main>
	</div>
</div>



<style>
	
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 80vw;
		margin: 0 auto;
		box-sizing: border-box;
		font-family: 'Chakra Petch', sans-serif !important;
	}

</style>
