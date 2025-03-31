<script lang="ts">
	import '../app.css';
	import Navbar from "@/components/navbar/Navbar.svelte";
	import Footer from '@/components/footer/Footer.svelte';
    import FlickeringGrid from "@/components/flickeringGrid/FlickeringGrid.svelte";
	let { children } = $props();
</script>
<Navbar/>
<FlickeringGrid
class="z-0 fixed inset-0 w-full"
squareSize={2}
gridGap={8}
color="#E0DEF4"
maxOpacity={0.1}
flickerChance={0.05}
/>
<img src="/Gradient.png" alt="Background Gradient" class="fixed w-full h-full" />
<div class="fixed w-full h-full bg-gradient-to-b from-transparent via-transparent to-background-1"></div>
{@render children()}
<Footer/>

<style>

</style>
