<script lang="ts">
  import OptionSelector from '../optionSelector/OptionSelector.svelte';
  import { FileUp, LoaderCircle, Maximize2, AudioLines, Cog, FolderOpen, FileOutput, Video } from 'lucide-svelte';
  import Button from "../jsrepo/button/button.svelte";
  import TerminalInput from "../terminal-input/TerminalInput.svelte";
  import { cubicOut } from "svelte/easing";
  import { slide, scale } from 'svelte/transition';

  let selectedFile = $state<File | null>(null);
  let isLoading = $state(false);
  let fileName = $state("");
  let errorMessage = $state("");

  let testValue:any = $state();

  //option variables
  let ffmpegOptions = $state({
    filePath: "",
    outputName: "",
    encoder: "",
    resolution: "",
    mediaType: "",
    bitrate: 0,
  });

  //ffmpreg command building
  let ffmpegCommand = $derived.by(() => {
  const _filePath = ffmpegOptions.filePath || "input_file";
  const _outputName = ffmpegOptions.outputName || "output";
  const _mediaType = ffmpegOptions.mediaType || "mp4";

  // Construct the FFmpeg command with conditional inclusions
  return `ffmpeg -i ${_filePath} ${
    ffmpegOptions.resolution ? `-vf scale=${ffmpegOptions.resolution}` : ""
  } ${
    ffmpegOptions.encoder ? `-c:v ${ffmpegOptions.encoder}` : ""
  } ${
    ffmpegOptions.bitrate ? `-b:v ${ffmpegOptions.bitrate}k` : ""
  } ${_outputName}.${_mediaType}`
    .replace(/\s+/g, " ") // Replace multiple spaces with a single space
    .trim(); // Remove leading/trailing spaces
});

  //actual option inputs, key must be ffmpegOptions variable name
  const options = [
    {
      variant: "file",
      name: "File Path",
      key: "filePath",
      Icon: FolderOpen,
      tabs: [
        { name: "Field1", values: [] },
        { name: "Field2", values: [] },
      ],
    },
    {
      variant: "text",
      name: "Output Name",
      key: "outputName",
      Icon: FileOutput,
      tabs: [
        { name: "Low", values: [] },
        { name: "High", values: [] },
      ],
    },
    {
      variant: "single",
      name: "Encoder",
      key: "encoder",
      Icon: Cog,
      tabs: [
        {
          name: "Software",
          values: ["H.264", "H.265", "VP9", "AV1"],
        },
        {
          name: "Hardware",
          values: ["H.264", "H.265", "VP9", "AV1"],
        },
      ],
    },
    {
      variant: "multiple",
      name: "Resolution",
      key: "resolution",
      Icon: Maximize2,
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
      variant: "single",
      name: "Media Type",
      key: "mediaType",
      Icon: Video,
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
      variant: "slider",
      name: "Bitrate Range",
      key: "bitrate",
      Icon: AudioLines,
      tabs: [
        { name: "Low", values: [] },
        { name: "High", values: [] },
      ],
    },
  ];

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
    setTimeout(() => {
      isLoading = false;
    }, 2000);
  };

  let selectedValues = $state<Record<string, string>>({});

  $effect(() => {
    for (const [key, value] of Object.entries(selectedValues)) {
      switch (key) {
        case "File Path":
          ffmpegOptions.filePath = value;
          break;
        case "Output Name":
          ffmpegOptions.outputName = value;
          break;
        case "Encoder":
          ffmpegOptions.encoder = value;
          break;
        case "Resolution":
          ffmpegOptions.resolution = value;
          break;
        case "Media Type":
          ffmpegOptions.mediaType = value;
          break;
        case "Bitrate Range":
          ffmpegOptions.bitrate = parseInt(value, 10);
          break;
      }
    }
  });
</script>

<main class="overflow-hidden">
  <section>
    <div class="relative pt-24">
      <div
        class="absolute inset-0 -z-10 size-full [background:radial-gradient(125%_125%_at_50%_100%,transparent_0%,var(--color-background)_75%)]"
      ></div>
      {#if !selectedFile}
      <div transition:slide class="mx-auto max-w-5xl px-6 mt-8">
        <div class="sm:mx-auto lg:mr-auto lg:mt-0">
          <h1
            class="mt-8 max-w-2xl text-balance text-5xl font-medium md:text-6xl lg:mt-16"
          >
            Effortless Multimedia Processing with <span class="text-primary font-bold underline">FFmpreg</span>
          </h1>
          <p class="mt-8 max-w-2xl text-pretty text-lg">
            Harness the power of FFmpeg directly in your browser. Convert formats, edit metadata, compress files, and process videos, audio, images, subtitles, and streams—all without installation.
          </p>
          <div class="mt-12 flex items-center gap-2">
            <div
              class="bg-transparent border border-primary p-0.5"
              style="border-radius: calc(var(--radius) + 4px + 0.125rem);"
            >
              <Button size="lg" class="rounded-xl px-5 text-base cursor-pointer">
                <a href="/">
                  <span class="text-nowrap">Get Started Now</span>
                </a>
              </Button>
            </div>
            <Button
              size="lg"
              variant="ghost"
              class="h-10.5 rounded-xl px-5 text-base cursor-pointer"
            >
              <a href="/">
                <span class="text-nowrap">Try a Demo</span>
              </a>
            </Button>
          </div>
        </div>
      </div>
      {/if}
      {#if selectedFile}
      <div class="mx-auto max-w-7xl px-6 grid grid-cols-1 md:grid-cols-2 gap-8 mt-12">
        <div class="flex flex-col gap-8 overflow-y-auto min-h-[calc(100vh-10rem)] pb-8">
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
              <div class="flex flex-col items-center justify-center gap-2">
                <LoaderCircle class="animate-spin text-primary size-12" />
                <p class="text-muted-foreground text-center">Uploading..</p>
              </div>
            {:else if fileName}
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
          {#each options as option}
            <OptionSelector
              variant={option.variant}
              name={option.name}
              Icon={option.Icon}
              tabs={option.tabs}
              bind:value={ffmpegOptions[option.key]}
            />
          {/each}
        </div>
        <div class="flex flex-col gap-4 min-h-[calc(100vh-10rem)]">
          <TerminalInput command={ffmpegCommand} />
        </div>
      </div>
      {:else}
      <div
        class="ring-ring bg-card relative mx-auto max-w-2xl h-[15rem] rounded-radius border border-border p-4 shadow-lg shadow-shadow ring-1 flex flex-col items-center justify-center cursor-pointer transition-transform duration-500 ease-in-out mt-12"
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