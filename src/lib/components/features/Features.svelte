<script lang="ts">
  import { onMount } from "svelte";
  import Compare from "../compare/Compare.svelte";

  interface Props {
    collapseDelay?: number;
    ltr?: boolean;
    linePosition?: "left" | "right" | "top" | "bottom";
    data?: Array<{
      id: number;
      title: string;
      content: string;
      beforeImage: string; // Before processing
      afterImage: string; // After processing
      beforeSize: string; // File size before
      afterSize: string; // File size after
      icon?: any;
    }>;
  }

  let {
    collapseDelay = 5000,
    ltr = false,
    linePosition = "top", // Default to "top"
    data = []
  }: Props = $props();

  let currentIndex = $state(-1);
  let carouselRef: HTMLUListElement = $state();
  let isInView = false;

  const scrollToIndex = (index: number) => {
    if (carouselRef) {
      const cards = carouselRef.querySelectorAll(".card_code");
      const card = cards[index] as HTMLElement;

      if (card) {
        const cardRect = card.getBoundingClientRect();
        const carouselRect = carouselRef.getBoundingClientRect();
        const offset =
          cardRect.left -
          carouselRect.left -
          (carouselRect.width - cardRect.width) / 2;

        carouselRef.scrollTo({
          left: carouselRef.scrollLeft + offset,
          behavior: "smooth",
        });
      }
    }
  };

  onMount(() => {
    let timer = setTimeout(() => {
      currentIndex = 0;
    }, 100);
    return () => clearTimeout(timer);
  });

  onMount(() => {
    handleScroll();
  });

  let handleScroll = () => {
    let autoScrollTimer: NodeJS.Timeout;
    let handleAutoScroll = () => {
      currentIndex = currentIndex !== undefined 
        ? (currentIndex + 1) % data.length 
        : 0;
    };

    autoScrollTimer = setInterval(handleAutoScroll, collapseDelay);

    return () => clearInterval(autoScrollTimer);
  };
</script>

<section id="features">
  <div class="container">
    <div class="mx-auto mt-48 max-w-6xl">
      <!-- Items Row -->
      <div class="flex flex-col lg:flex-row justify-center items-center gap-10">
        <ul
          bind:this={carouselRef}
          class="flex h-full snap-x snap-mandatory flex-nowrap overflow-x-auto py-10 [-ms-overflow-style:none] [-webkit-mask-image:linear-gradient(90deg,transparent,black_20%,white_80%,transparent)] [mask-image:linear-gradient(90deg,transparent,black_20%,white_80%,transparent)] [scrollbar-width:none] [&::-webkit-scrollbar]:hidden"
        >
          {#each data as item, index}
            <!-- Card Wrapper -->
            <li
              class="card_code relative mr-8 grid h-full max-w-60 shrink-0 items-start justify-center py-4 last:mr-0"
              onclick={() => {
                currentIndex = index;
                scrollToIndex(index);
              }}
              style="scroll-snap-align: center;"
            >
              <div
                class="group relative flex flex-col items-center space-y-4 p-4 rounded-lg transition-all duration-300 hover:bg-primary/10 hover:scale-105 hover:shadow-lg cursor-pointer"
              >
                <!-- Line Positioning -->
                {#if linePosition === "top"}
                  <div
                    class="w-full h-0.5 bg-grey-1 rounded-lg overflow-hidden relative"
                  >
                    <div
                      class={`absolute left-0 ${
                        currentIndex === index ? "w-full" : "w-0"
                      } h-full origin-left bg-primary transition-all ease-linear`}
                      style="
                        transition-duration:
                          {currentIndex === index
                            ? `${collapseDelay}ms`
                            : '0s'};"
                    ></div>
                  </div>
                {/if}

                <!-- Icon -->
                <div
                  class="item-box mx-2 flex size-12 shrink-0 items-center justify-center rounded-full bg-primary/10 sm:mx-6"
                >
                  <item.icon class="size-6 text-primary" />
                </div>

                <!-- Title and Content -->
                <div class="text-center space-y-2">
                  <h3 class="text-lg font-bold">{item.title}</h3>
                  <p class="text-[16px] text-muted-foreground">
                    {item.content}
                  </p>
                  <div class="text-sm text-muted-foreground mt-2">
                    <span>Before: {item.beforeSize}</span>
                    <br />
                    <span>After: {item.afterSize}</span>
                  </div>
                </div>
              </div>
            </li>
          {/each}
        </ul>
      </div>

      <!-- Image/Video Section -->
      <div class="mt-0 flex justify-center relative mx-auto max-w-5xl">
        {#if data[currentIndex]?.beforeImage && data[currentIndex]?.afterImage}
        <Compare
          firstImage={data[currentIndex].beforeImage}
          secondImage={data[currentIndex].afterImage}
          firstImageSize={data[currentIndex].beforeSize}
          secondImageSize={data[currentIndex].afterSize}
          firstImageClass="object-fit object-center"
          secondImageClass="object-fit object-center"
          class="border border-border aspect-15/8 relative hidden rounded-radius dark:block w-full h-full"
          slideMode="drag"
        />
      {:else if data[currentIndex]?.video}
        <video
          preload="auto"
          src={data[currentIndex].video}
          class="aspect-auto w-full md:w-3/4 rounded-radius border border-border object-cover shadow-lg"
          autoplay
          loop
          muted
        ></video>
      {:else}
        <div
          class="aspect-auto w-full md:w-3/4 rounded-xl border border-border bg-grey-1 p-1"
        ></div>
      {/if}
      </div>
    </div>
  </div>
</section>

<style>
  .card_code {
    transition: all 0.3s ease;
  }
  .item-box {
    width: 3rem;
    height: 3rem;
  }
</style>