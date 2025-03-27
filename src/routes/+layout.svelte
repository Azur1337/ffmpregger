<script lang="ts">
	import '../app.css';
	import Navbar from "@/components/navbar/Navbar.svelte";
	import Footer from '@/components/footer/Footer.svelte';

	let { children } = $props();
</script>
<Navbar/>
{@render children()}
<Footer/>

<style>

</style>
