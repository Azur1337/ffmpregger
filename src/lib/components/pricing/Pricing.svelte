<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import { Check } from "@lucide/svelte";
  import BorderBeam from "../borderbeam/BorderBeam.svelte";

  let list = [
    "30 FFmpeg commands per month (1 token = 1 command)",
    "Priority processing for faster results",
    "Access to exclusive advanced FFmpeg features",
    "Customizable output formats and presets",
    "Ad-free experience and priority support"
  ];
</script>

<div class="relative py-16 md:py-32">
  <div class="mx-auto max-w-5xl px-6">

    <div class="mx-auto max-w-2xl text-center relative z-10">
      <h2 class="text-balance text-3xl font-bold md:text-4xl lg:text-5xl">
        Unlock Unlimited Creativity with <span class="text-primary underline">FFmpreg</span> Premium
      </h2>
    </div>

    <div class="mt-8 md:mt-20">
      <div class="bg-card relative rounded-3xl border border-border shadow-2xl shadow-shadow">
        <BorderBeam size={150} duration={12} />
        <div class="grid items-center gap-12 divide-y p-12 md:grid-cols-2 md:divide-x md:divide-y-0 divide-border">
          <!-- Left Column: Subscription Details -->
          <div class="pb-12 text-center md:pb-0 md:pr-12">
            <h3 class="text-2xl font-semibold"><span class="text-primary">FFmpreg</span> Premium</h3>
            <p class="mt-2 text-lg">Supercharge your multimedia workflow</p>
            <span class="mb-6 mt-12 inline-block text-6xl font-bold">
              <span class="text-4xl">$</span>9.99
            </span>

            <div class="flex justify-center">
              <Button href="#" size="lg">Subscribe Now</Button>
            </div>

            <p class="text-foreground-2 mt-12 text-sm">
              Includes: 30 tokens/month, priority processing, advanced features, and more.
            </p>
          </div>

          <!-- Right Column: Features List -->
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
              Join thousands of creators who trust FFmpreg Premium to process their multimedia files efficiently.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>