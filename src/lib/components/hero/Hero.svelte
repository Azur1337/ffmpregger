<script lang="ts">
  import { ChevronRight } from "@lucide/svelte";
  import Button from "../jsrepo/button/button.svelte";
  import { slide } from "svelte/transition";
  import { scrollY } from "svelte/reactivity/window";
  import { FileUp } from "lucide-svelte";

  let selectedFile = null;
  let isLoading = false; // State for loading animation
  let fileName = ""; // To display the uploaded file name
  let errorMessage = ""; // To display error messages

  // Handle file upload
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
    errorMessage = ""; // Clear any previous error message
    isLoading = true;

    console.log("Selected file:", selectedFile);

    setTimeout(() => {
      isLoading = false; // Simulate processing completion
    }, 2000);
  };

  let menuItems = [
    { name: "Features", href: "#features" },
    { name: "How It Works", href: "#how-it-works" },
    { name: "Pricing", href: "#pricing" },
    { name: "About", href: "#about" },
  ];
  let menuState = $state(false);
  let isScrolled = $derived.by(() => {
    if (scrollY.current !== undefined && scrollY.current > 50) {
      return true;
    }
    return false;
  });
</script>

<main class="overflow-hidden">
  <img src="/Gradient.png" class="absolute w-full h-full" />
  <div class="absolute w-full h-full bg-gradient-to-b from-transparent via-transparent to-background-1"></div>
  <div
    aria-hidden
    class="absolute inset-0 isolate hidden contain-strict lg:block"
  >
    <div
      class="w-140 h-320 -translate-y-87.5 absolute left-0 top-0 -rotate-45 rounded-full bg-[radial-gradient(68.54%_68.72%_at_55.02%_31.46%,hsla(268,21%,57%,.08)_0,hsla(267,57%,78%,.02)_50%,hsla(267,82%,87%,0)_80%)]"
    />
    <div
      class="h-320 absolute left-0 top-0 w-60 -rotate-45 rounded-full bg-[radial-gradient(50%_50%_at_50%_50%,hsla(268,21%,57%,.06)_0,hsla(267,57%,78%,.02)_80%,hsla(267,82%,87%,0)_100%)] [translate:5%_-50%]"
    />
    <div
      class="h-320 -translate-y-87.5 absolute left-0 top-0 w-60 -rotate-45 bg-[radial-gradient(50%_50%_at_50%_50%,hsla(268,21%,57%.04)_0,hsla(267,57%,78%,.02)_80%,transparent_100%)]"
    />
  </div>
  <section>
    <div class="relative pt-24">
      <div
        class="absolute inset-0 -z-10 size-full [background:radial-gradient(125%_125%_at_50%_100%,transparent_0%,var(--color-background)_75%)]"
      ></div>
      <div class="mx-auto max-w-5xl px-6">
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
      <div>
        <div
          class="relative -mr-56 mt-8 overflow-hidden px-2 sm:mr-0 sm:mt-12 md:mt-20"
        >
          <div
            in:slide={{ duration: 1000 }}
            class="inset-shadow-2xs ring-ring bg-card relative mx-auto max-w-2xl h-[15rem] rounded-radius border border-border p-4 shadow-lg shadow-shadow ring-1 flex flex-col items-center justify-center cursor-pointer hover:bg-primary/10 hover:border-transparent hover:shadow-transparent hover:ring-transparent transition-all duration-150"
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
              <div class="relative size-12 mb-4">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  class="size-12 text-primary animate-spin"
                >
                  <path d="M12 2v4" />
                  <path d="M16.24 7.76l-2.12 2.12" />
                  <circle cx="12" cy="12" r="9" />
                </svg>
              </div>
              <p class="text-muted-foreground text-center">Processing...</p>
            {:else if fileName}
              <FileUp class="size-12 text-primary mb-4" />
              <p class="text-muted-foreground text-center">{fileName}</p>
            {:else}
              <FileUp class="size-12 text-primary mb-4" />
              <p class="text-muted-foreground text-center">
                Drag and drop or click to upload a file
              </p>
            {/if}
          </div>
        </div>
      </div>
    </div>
  </section>
  <section class="bg-background-1 pb-16 pt-16 md:pb-32">
    <div class="group relative m-auto max-w-5xl px-6">
      <div
        class="absolute inset-0 z-10 flex scale-95 items-center justify-center opacity-0 duration-500 group-hover:scale-100 group-hover:opacity-100"
      >
        <a href="/" class="block text-sm duration-150 hover:opacity-75">
          <span> Meet Our Users</span>

          <ChevronRight class="ml-1 inline-block size-3" />
        </a>
      </div>
    </div>
  </section>
</main>