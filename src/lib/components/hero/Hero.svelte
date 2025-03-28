<script lang="ts">
  import {FileOutput, FolderOpen } from "@lucide/svelte";
  import { Cog, Video, Maximize2, AudioLines } from 'lucide-svelte';
  import Button from "../jsrepo/button/button.svelte";
  import { slide, scale } from "svelte/transition";
  import { cubicOut } from "svelte/easing";
  import TerminalInput from "../terminal-input/TerminalInput.svelte";
  import OptionSelector from "../optionSelector/OptionSelector.svelte";
  import { FileUp, LoaderCircle } from "lucide-svelte";

  let selectedFile = $state(null);
  let isLoading = $state(false);
  let fileName = $state("");
  let errorMessage = $state("");

  const customTabs = [
    {
      name: "Software",
      values: ["H.264", "H.265", "VP9", "AV1"],
    },
    {
      name: "Hardware",
      values: ["H.264", "H.265", "VP9", "AV1"],
    },
  ];

  let ffmpegOptions = $state({
    resolution: "",
    format: "",
    bitrate: "",
  });

  const handleFileUpload = (event: Event) => {
    const input = event.target as HTMLInputElement;
    const files = input.files;

    if (!files || files.length === 0) {
      console.error("No file selected.");
      errorMessage = "No file selected.";
      return;
    }

    const file = files[0];

    selectedFile = file;
    fileName = file.name;
    errorMessage = "";
    isLoading = true;

    console.log("Selected file:", selectedFile);

    setTimeout(() => {
      isLoading = false;
    }, 2000);
  };

  let ffmpegCommand = $derived.by(() => {
    return `ffmpeg -i ${fileName} ${
      ffmpegOptions.resolution ? `-vf scale=${ffmpegOptions.resolution} ` : ""
    }${ffmpegOptions.format ? `-f ${ffmpegOptions.format} ` : ""}${
      ffmpegOptions.bitrate ? `-b:v ${ffmpegOptions.bitrate} ` : ""
    }output.${ffmpegOptions.format || "mp4"}`;
  });

  const optionSelectors = [
    {
      type: "file",
      title: "File Path",
      IconComponent: FolderOpen,
      tabs: [
        {
          name: "Field1",
          values: [],
        },
        {
          name: "Field2",
          values: [],
        },
      ],
    },
    {
      type: "text",
      title: "Output Name",
      IconComponent: FileOutput,
      tabs: [
        {
          name: "Low",
          values: [],
        },
        {
          name: "High",
          values: [],
        },
      ],
    },
    {
      title: "Encoder",
      IconComponent: Cog,
      tabs: customTabs,
    },
    {
      type: "multiple",
      title: "Resolution",
      IconComponent: Maximize2,
      tabs: [
        {
          name: "Standard",
          values: ["1920x1080", "1280x720", "640x480"],
        },
        {
          name: "Custom",
          values: ["3840x2160", "2560x1440"],
        },
      ],
    },
    {
      title: "Media Type",
      IconComponent: Video,
      tabs: [
        {
          name: "Video",
          values: ["MP4", "MKV", "AVI"],
        },
        {
          name: "Audio",
          values: ["MP3", "WAV", "FLAC"],
        },
      ],
    },
    {
      type: "slider",
      title: "Bitrate Range",
      IconComponent: AudioLines,
      tabs: [
        {
          name: "Low",
          values: [],
        },
        {
          name: "High",
          values: [],
        },
      ],
    },
  ];
</script>

<main class="overflow-hidden">
  <section>
    <div class="relative pt-24">
      <div
        class="absolute inset-0 -z-10 size-full [background:radial-gradient(125%_125%_at_50%_100%,transparent_0%,var(--color-background)_75%)]"
      ></div>

      <!-- Initial Hero Section -->
      {#if !selectedFile}
      <div transition:slide class="mx-auto max-w-5xl px-6 mt-8">
        <div class="sm:mx-auto lg:mr-auto lg:mt-0">
          <h1
            class="mt-8 max-w-2xl text-balance text-5xl font-medium md:text-6xl lg:mt-16"
          >
            Effortless Multimedia Processing with <span class="text-primary font-bold underline">ffmpreg</span>
          </h1>
          <p class="mt-8 max-w-2xl text-pretty text-lg">
            Harness the power of FFmpeg directly in your browser. Convert formats, edit metadata, compress files, and process videos, audio, images, subtitles, and streams—all without installation.
          </p>

          <div class="mt-12 flex items-center gap-2">
            <div
              class="bg-transparent border border-primary p-0.5"
              style="border-radius: calc(var(--radius) + 4px + 0.125rem);"
            >
              <Button size="lg" class="rounded-xl px-5 text-base">
                <a href="#get-started">
                  <span class="text-nowrap">Get Started Now</span>
                </a>
              </Button>
            </div>
            <Button
              size="lg"
              variant="ghost"
              class="h-10.5 rounded-xl px-5 text-base"
            >
              <a href="#demo">
                <span class="text-nowrap">Try a Demo</span>
              </a>
            </Button>
          </div>
        </div>
      </div>
      {/if}

      <!-- Two-Column Layout After File Upload -->
      {#if selectedFile}
      <div class="mx-auto max-w-7xl px-6 grid grid-cols-1 md:grid-cols-2 gap-8 mt-12">
        <!-- Left Column: File Display and Additional Inputs -->
        <div class="flex flex-col gap-8 overflow-y-auto min-h-[calc(100vh-10rem)] pb-8">
          <!-- File Display -->
          <div
            class="ring-ring bg-card relative w-full h-[15rem] rounded-radius border border-border p-4 shadow-lg shadow-shadow ring-1 flex items-center justify-center cursor-pointer transition-transform duration-500 ease-in-out"
            in:scale={{ duration: 500, easing: cubicOut }}
          >
            <input
              type="file"
              onchange={handleFileUpload}
              aria-label="Upload a file"
              class="absolute inset-0 opacity-0 cursor-pointer w-full h-full z-20 pointer-events-auto"
            />

            {#if errorMessage}
              <p class="text-destructive text-center">{errorMessage}</p>
            {:else if isLoading}
              <!-- Loading Screen -->
              <div class="flex flex-col items-center justify-center gap-2">
                <LoaderCircle class="animate-spin text-primary size-12" />
                <p class="text-muted-foreground text-center">Uploading..</p>
              </div>
            {:else if fileName}
              <!-- Uploaded File Metadata -->
              <div class="flex flex-row items-center justify-between w-full gap-4 px-12">
                <div class="flex flex-col justify-center items-center gap-2">
                  <FileUp class="size-12 text-primary" />
                  <p class="text-muted-foreground">{fileName}</p>
                </div>
                <div class="flex flex-col gap-1 text-right text-sm text-muted-foreground">
                  <p>Resolution: 1920x1080</p>
                  <p>Format: {selectedFile?.type || "Unknown"}</p>
                  <p>Size: {(selectedFile?.size / 1024 / 1024).toFixed(2)} MB</p>
                </div>
              </div>
            {:else}
              <div class="flex flex-col items-center justify-center gap-2">
                <FileUp class="size-12 text-primary" />
                <p class="text-muted-foreground text-center">
                  Drag and drop or click to upload a file
                </p>
              </div>
            {/if}
          </div>

          <!-- Additional OptionSelectors on the Left -->
          {#each optionSelectors as selector}
            <OptionSelector
              type={selector.type || "single"}
              title={selector.title}
              IconComponent={selector.IconComponent}
              tabs={selector.tabs}
            />
          {/each}
        </div>

        <!-- Right Column: TerminalInput and Additional Inputs -->
        <div class="flex flex-col gap-4 min-h-[calc(100vh-10rem)]">
          <TerminalInput />
          <!-- Add more components here as needed -->
        </div>
      </div>
      {:else}
      <!-- File Upload Area -->
      <div
        class="ring-ring bg-card relative mx-auto max-w-2xl h-[15rem] rounded-radius border border-border p-4 shadow-lg shadow-shadow ring-1 flex flex-col items-center justify-center cursor-pointer transition-transform duration-500 ease-in-out mt-8"
        in:scale={{ duration: 500, easing: cubicOut }}
        out:slide={{ duration: 500, easing: cubicOut }}
      >
        <input
          type="file"
          onchange={handleFileUpload}
          aria-label="Upload a file"
          class="absolute inset-0 opacity-0 cursor-pointer w-full h-full z-20 pointer-events-auto"
        />

        {#if errorMessage}
          <p class="text-destructive text-center">{errorMessage}</p>
        {:else if isLoading}
          <div class="flex flex-col items-center justify-center gap-2">
            <LoaderCircle class="animate-spin text-primary size-12" />
            <p class="text-muted-foreground text-center">Uploading..</p>
          </div>
        {:else if fileName}
          <div class="flex flex-col items-center justify-center gap-2">
            <FileUp class="size-12 text-primary" />
            <p class="text-muted-foreground text-center">{fileName}</p>
          </div>
        {:else}
          <div class="flex flex-col items-center justify-center gap-2">
            <FileUp class="size-12 text-primary" />
            <p class="text-muted-foreground text-center">
              Drag and drop or click to upload a file
            </p>
          </div>
        {/if}
      </div>
      {/if}
    </div>
  </section>
</main>