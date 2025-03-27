<script>
    import Button from "$lib/components/ui/button/button.svelte";
    import FlickeringGrid from "../flickeringGrid/FlickeringGrid.svelte";
    import { Check } from "@lucide/svelte";
    import BorderBeam from "../borderbeam/BorderBeam.svelte";
  
    let list = [
      "Unlock advanced analytics for deeper insights",
      "Customize your profile with banners, themes, and colors",
      "Access exclusive community perks and early features",
      "Support the development of Psychonaut Wrapped",
      "Enjoy an ad-free experience"
    ];
  </script>
  
  <div class="relative py-16 md:py-32">
    <div class="mx-auto max-w-5xl px-6">
      <!-- Flickering Grid -->
      <FlickeringGrid
        class="z-0 absolute inset-0 w-full"
        squareSize={2}
        gridGap={8}
        color="#E0DEF4"
        maxOpacity={0.1}
        flickerChance={0.05}
      />
      <div class="z-0 absolute inset-0 bg-gradient-to-b from-background-1 via-transparent to-background-1 pointer-events-none"></div>
  
      <div class="mx-auto max-w-2xl text-center relative z-10">
        <h2 class="text-balance text-3xl font-bold md:text-4xl lg:text-5xl">
          Unlock Your Journey with Psychonaut Wrapped Premium
        </h2>
      </div>
  
      <div class="mt-8 md:mt-20">
        <div class="bg-card relative rounded-3xl border border-border shadow-2xl shadow-shadow">
        <BorderBeam size={150} duration={12} />
          <div class="grid items-center gap-12 divide-y p-12 md:grid-cols-2 md:divide-x md:divide-y-0 divide-border">
            <div class="pb-12 text-center md:pb-0 md:pr-12">
              <h3 class="text-2xl font-semibold">Psychonaut Wrapped Premium</h3>
              <p class="mt-2 text-lg">Elevate your self-exploration journey</p>
              <span class="mb-6 mt-12 inline-block text-6xl font-bold">
                <span class="text-4xl">$</span>4.99
              </span>
  
              <div class="flex justify-center">
                <Button href="#" size="lg">Get Started</Button>
              </div>
  
              <p class="text-foreground-2 mt-12 text-sm">
                Includes: Advanced analytics, customizable profiles, exclusive perks, and more.
              </p>
            </div>
  
            <div class="relative">
              <ul role="list" class="space-y-4">
                {#each list as item}
                  <li class="flex items-center gap-2">
                    <Check class="size-4 text-primary" />
                    <span>{item}</span>
                  </li>
                {/each}
              </ul>
              <p class="text-foreground-2 mt-6 text-sm">
                Join thousands of explorers who trust Psychonaut Wrapped to track their journeys and gain meaningful insights.
              </p>
              <div class="mt-12 flex flex-wrap items-center justify-between gap-6">
                <img
                  class="h-5 w-fit dark:invert"
                  src="https://html.tailus.io/blocks/customers/nvidia.svg"
                  alt="Nvidia Logo"
                  height="20"
                  width="auto"
                />
                <img
                  class="h-4 w-fit dark:invert"
                  src="https://html.tailus.io/blocks/customers/column.svg"
                  alt="Column Logo"
                  height="16"
                  width="auto"
                />
                <img
                  class="h-4 w-fit dark:invert"
                  src="https://html.tailus.io/blocks/customers/github.svg"
                  alt="GitHub Logo"
                  height="16"
                  width="auto"
                />
                <img
                  class="h-5 w-fit dark:invert"
                  src="https://html.tailus.io/blocks/customers/nike.svg"
                  alt="Nike Logo"
                  height="20"
                  width="auto"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>