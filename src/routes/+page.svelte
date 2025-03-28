<script lang="ts">
	import Hero from "@/components/hero/Hero.svelte";
	import Features from "$lib/components/features/Features.svelte";
	import Pricing from "$lib/components/pricing/Pricing.svelte";

	import FolderDown from "lucide-svelte/icons/folder-down";
	import ArrowRightLeft from "lucide-svelte/icons/arrow-right-left";
	import CircleFadingArrowUp from "lucide-svelte/icons/circle-fading-arrow-up";

	let features = [
  {
    id: 1,
    title: "Compress Videos",
    content: "Reduce video file size without losing quality using FFMpreg.",
    beforeImage: "/femboy1.jpg",
    afterImage: "/femboy1.jpg",
    beforeSize: "50 MB",
    afterSize: "10 MB",
    icon: FolderDown,
  },
  {
    id: 2,
    title: "Convert Formats",
    content: "Effortlessly convert videos to different formats for compatibility.",
    beforeImage: "/femboy2.jpg",
    afterImage: "/femboy2.jpg",
    beforeSize: "30 MB",
    afterSize: "15 MB",
    icon: ArrowRightLeft,
  },
  {
    id: 3,
    title: "Optimize Images",
    content: "Optimize images for faster loading times while maintaining clarity.",
    beforeImage: "/femboy3.jpg",
    afterImage: "/femboy3.jpg",
    beforeSize: "2 MB",
    afterSize: "500 KB",
    icon: CircleFadingArrowUp,
  },
];
</script>

<Hero/>
<Features data={features} />
<Pricing />