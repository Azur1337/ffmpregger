<script lang="ts">
  import * as Tabs from "$lib/components/ui/tabs/index.js";
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
  import { Slider } from "$lib/components/ui/slider/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  let {
    type,
    title,
    IconComponent,
    tabs,
  }: {
    type: "single" | "multiple" | "slider" | "file" | "text";
    title: string;
    IconComponent: typeof import("lucide-svelte").Settings;
    tabs: { name: string; values: string[] }[];
  } = $props();

  let sliderValue: number = $state(0);
  let inputValue: string = $state("");
  let selectedFile: File | null = $state(null);
  let filePath: string = $state("");
  let selectedTab = tabs[0]?.name || "Software";

  const handleFileUpload = (event: Event) => {
    const input = event.target as HTMLInputElement;
    if (!input.files || input.files.length === 0) return;

    selectedFile = input.files[0];
    filePath = selectedFile.name;
  };
</script>

<div class="bg-transparent flex flex-col h-[5rem]">
  <Tabs.Root value={selectedTab}>
    <div class="w-full h-2/5 bg-transparent flex flex-row items-center">
      <!-- Title Div -->
      <div
        class="bg-card rounded-t-radius flex flex-row items-center justify-start gap-1 text-foreground-2 hover:text-primary transition-colors duration-150 whitespace-nowrap px-4 py-2"
        style="min-width: max-content;"
      >
        <IconComponent size={19} />
        <span>{title}</span>
      </div>

      <!-- Tabs List -->
      <div class="flex flex-row items-center px-4 gap-x-2">
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
      </div>
    </div>

    <!-- Tabs Content -->
    <div class="bg-card w-full h-3/5 flex items-center p-2 rounded-b-radius rounded-tr-radius">
      {#each tabs as tab}
        <Tabs.Content value={tab.name}>
          {#if type === "single" || type === "multiple"}
            <!-- ToggleGroup for Single/Multiple Selection -->
            <ToggleGroup.Root type={type}>
              {#each tab.values as value}
                <ToggleGroup.Item
                  value={value}
                  class="px-4 py-1.5 rounded-radius text-sm font-medium transition-all duration-150 cursor-pointer bg-muted text-foreground-2 hover:bg-accent hover:text-primary"
                >
                  {value}
                </ToggleGroup.Item>
              {/each}
            </ToggleGroup.Root>
          {:else if type === "slider"}
            <!-- Slider for Range Selection -->
            <div class="flex flex-row items-center gap-4 w-3/4">
              <p class="text-md text-muted-foreground font-bold shrink-0 px-2">
                Original
              </p>
              <Slider
                bind:value={sliderValue}
                type="single"
                min={0}
                max={100}
                step={1}
              />
              <div class="size-8 aspect-square bg-accent rounded-radius flex items-center justify-center text-sm font-bold">
                {sliderValue}
              </div>
            </div>
          {:else if type === "file"}
            <!-- File Upload Input -->
            <div class="flex flex-row items-center gap-4 w-full">
              <Input
                type="text"
                readonly
                bind:value={filePath}
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
                onchange={handleFileUpload}
                class="hidden"
              />
            </div>
          {:else if type === "text"}
            <!-- Plain Input Field -->
            <div class="flex flex-row items-center gap-4 w-full">
              <Input
                type="text"
                bind:value={inputValue}
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