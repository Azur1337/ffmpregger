<script lang="ts">
  import * as Tabs from "$lib/components/ui/tabs/index.js";
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
  import { Slider } from "$lib/components/ui/slider/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { FileUp } from 'lucide-svelte';

  export interface OptionSelectorProps {
    name: string;
    Icon: IconType;
    tabs: {
      name: string;
      values: string[];
    }[];
    variant: "single" | "multiple" | "slider" | "file" | "text";
    value: any;
  };

  type IconType = typeof FileUp;

  let { 
    name = "Option Selector",
    Icon = FileUp,
    tabs,
    variant,
    value = $bindable()
  }: OptionSelectorProps = $props();

  $effect(() => {
    console.log(value);
  })

</script>

<div class="bg-transparent flex flex-col h-[5rem]">
  <Tabs.Root value={tabs[0]?.name || "Default"}>
    <div class="w-full h-2/5 bg-transparent flex flex-row items-center">
      <div
        class="bg-card rounded-t-radius flex flex-row items-center justify-start gap-1 text-foreground-2 hover:text-primary transition-colors duration-150 whitespace-nowrap px-4 py-2"
        style="min-width: max-content;"
      >
        <Icon size={19} />
        <span>{name}</span>
      </div>

      <div class="flex flex-row items-center px-4 gap-x-2">
        {#if tabs.length > 1}
          <Tabs.List>
            {#each tabs as tab}
              <Tabs.Trigger
                value={tab.name}
                class="px-4 py-2 cursor-pointer transition-colors duration-150 data-[state=active]:text-primary hover:text-primary"
              >
                {tab.name}
              </Tabs.Trigger>
            {/each}
          </Tabs.List>
        {/if}
      </div>
    </div>

    <div class="bg-card w-full h-3/5 flex items-center p-2 rounded-b-radius rounded-tr-radius">
        {#each tabs as tab}
          <Tabs.Content value={tab.name}>
            {#if variant === "single" || variant === "multiple"}
              <ToggleGroup.Root
                type={variant}
                bind:value
              >
                {#each tab.values as option}
                  <ToggleGroup.Item
                    value={option}
                    class="px-4 py-1.5 rounded-radius text-sm font-medium transition-all duration-150 cursor-pointer bg-muted text-foreground-2 hover:bg-accent hover:text-primary"
                  >
                    {option}
                  </ToggleGroup.Item>
                {/each}
              </ToggleGroup.Root>
            {:else if variant === "slider"}
              <div class="flex flex-row items-center gap-4 w-3/4">
                <p class="text-md text-muted-foreground font-bold shrink-0 px-2">
                  Original
                </p>
                <Slider
                  bind:value
                  type="single"
                  min={0}
                  max={100}
                  step={1}
                />
                <div class="size-8 aspect-square bg-accent rounded-radius flex items-center justify-center text-sm font-bold">
                  {value}
                </div>
              </div>
            {:else if variant === "file"}
              <div class="flex flex-row items-center gap-4 w-full">
                <Input
                  type="text"
                  readonly
                  bind:value
                  placeholder="No file selected"
                  class="flex-grow px-3 py-1.5 rounded-radius border border-border bg-background text-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring focus-visible:border-accent"
                />
                <label
                  for="file-upload"
                  class="bg-accent px-3 py-1.5 rounded-radius border-none text-foreground-1 cursor-pointer hover:bg-accent-hover"
                >
                  Browse
                </label>
                <input
                  id="file-upload"
                  type="file"
                  accept="*"
                  class="hidden"
                  bind:value
                />
              </div>
            {:else if variant === "text"}
              <div class="flex flex-row items-center gap-4 w-full">
                <Input
                  type="text"
                  bind:value
                  placeholder="Enter value..."
                  class="flex-grow px-3 py-1.5 rounded-radius border border-border bg-background text-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring focus-visible:border-accent"
                />
              </div>
            {/if}
          </Tabs.Content>
        {/each}
    </div>
  </Tabs.Root>
</div>