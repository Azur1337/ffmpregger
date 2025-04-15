<script lang="ts">
	import OptionSelector from '../optionSelector/OptionSelector.svelte';
	import {
		FileUp,
		LoaderCircle,
		Maximize2,
		AudioLines,
		Cog,
		FolderOpen,
		FileOutput,
		Video
	} from 'lucide-svelte';
	import Button from '../jsrepo/button/button.svelte';
	import TerminalInput from '../terminal-input/TerminalInput.svelte';
	import { cubicOut } from 'svelte/easing';
	import { slide, scale } from 'svelte/transition';
	import { FFmpeg } from '@ffmpeg/ffmpeg';
	import { toBlobURL } from '@ffmpeg/util';

	let selectedFile = $state<File | null>(null);
	let isLoading = $state(false);
	let fileName = $state('');
	let errorMessage = $state('');

	const ffmpeg = new FFmpeg();

	let testValue: any = $state();

	//option variables
	let ffmpegOptions = $state({
		filePath: '',
		outputName: '',
		encoder: '',
		resolution: '',
		mediaType: '',
		bitrate: 0
	});

	//ffmpreg command building
	let ffmpegCommand = $derived.by(() => {
		const _filePath = fileName || 'input.mp4';
		const _outputName = ffmpegOptions.outputName || 'output';
		const _mediaType = ffmpegOptions.mediaType || 'mp4';

		return [
			'-i',
			_filePath,
			...(ffmpegOptions.resolution ? [`-vf`, `scale=${ffmpegOptions.resolution}`] : []),
			...(ffmpegOptions.encoder ? [`-c:v`, ffmpegOptions.encoder] : []),
			...(ffmpegOptions.bitrate ? [`-b:v`, `${ffmpegOptions.bitrate}k`] : []),
			`${_outputName}.${_mediaType}`
		].filter(Boolean);
	});

	//actual option inputs, key must be ffmpegOptions variable name
	const options = {
    filePath: {
      variant: "file",
      name: "File Path",
      Icon: FolderOpen,
      tabs: [
        { name: "Field1", values: [] },
        { name: "Field2", values: [] },
      ],
      value: {},
    },
    outputName: {
      variant: "text",
      name: "Output Name",
      Icon: FileOutput,
      tabs: [
        { name: "Low", values: [] },
        { name: "High", values: [] },
      ],
      value: {},
    },
    encoder: {
      variant: "single",
      name: "Encoder",
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
      value: {},
    },
    resolution: {
      variant: "multiple",
      name: "Resolution",
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
      value: {},
    },
    mediaType: {
      variant: "single",
      name: "Media Type",
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
      value: {},
    },
    bitrate: {
      variant: "slider",
      name: "Bitrate Range",
      Icon: AudioLines,
      tabs: [
        { name: "Low", values: [] },
        { name: "High", values: [] },
      ],
      value: {},
    },
  };

	const handleFileUpload = async (event: Event) => {
		const input = event.target as HTMLInputElement;
		const files = input.files;
		if (!files || files.length === 0) {
			console.error('No file selected.');
			errorMessage = 'No file selected.';
			return;
		}
		const file = files[0];
		selectedFile = file;
		fileName = file.name;
		errorMessage = '';
		isLoading = true;

		await ffmpeg.writeFile(fileName, await selectedFile.bytes());

		setTimeout(() => {
			isLoading = false;
		}, 2000);
	};

	let selectedValues = $state<Record<string, string>>({});

	$effect(() => {
		for (const [key, value] of Object.entries(selectedValues)) {
			switch (key) {
				case 'File Path':
					ffmpegOptions.filePath = value;
					break;
				case 'Output Name':
					ffmpegOptions.outputName = value;
					break;
				case 'Encoder':
					ffmpegOptions.encoder = value;
					break;
				case 'Resolution':
					ffmpegOptions.resolution = value;
					break;
				case 'Media Type':
					ffmpegOptions.mediaType = value;
					break;
				case 'Bitrate Range':
					ffmpegOptions.bitrate = parseInt(value, 10);
					break;
			}
		}

		const baseURL = 'https://unpkg.com/@ffmpeg/core-mt@0.12.6/dist/esm';
		ffmpeg.on('log', ({ message: msg }) => {
			console.log(msg);
		});

		ffmpeg.on('progress', (progress) => {
			let percent = progress.progress.toFixed(2);
			console.log('Processing... ' + percent + '% done');
		});

		(async () => {
			await ffmpeg.load({
				coreURL: await toBlobURL(`${baseURL}/ffmpeg-core.js`, 'text/javascript'),
				wasmURL: await toBlobURL(`${baseURL}/ffmpeg-core.wasm`, 'application/wasm'),
				workerURL: await toBlobURL(`${baseURL}/ffmpeg-core.worker.js`, 'text/javascript')
			});
		})();
	});

	async function processMedia() {
		let command = ffmpegCommand;

		const _outputName = ffmpegOptions.outputName || 'output';
		const _mediaType = ffmpegOptions.mediaType || 'mp4';

		await ffmpeg.exec(command);
		const output = await ffmpeg.readFile(`${_outputName}.${_mediaType}`);
		const blob = new Blob([output], { type: `video/${ffmpegOptions.mediaType}` });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = `${_outputName}.${_mediaType}`;
		a.click();
		URL.revokeObjectURL(url);
	}
</script>

<main class="overflow-hidden">
	<section>
		<div class="relative pt-24">
			<div
				class="absolute inset-0 -z-10 size-full [background:radial-gradient(125%_125%_at_50%_100%,transparent_0%,var(--color-background)_75%)]"
			></div>
			{#if !selectedFile}
				<div transition:slide class="mx-auto mt-8 max-w-5xl px-6">
					<div class="sm:mx-auto lg:mt-0 lg:mr-auto">
						<h1 class="mt-8 max-w-2xl text-5xl font-medium text-balance md:text-6xl lg:mt-16">
							Effortless Multimedia Processing with <span class="text-primary font-bold underline"
								>FFmpreg</span
							>
						</h1>
						<p class="mt-8 max-w-2xl text-lg text-pretty">
							Harness the power of FFmpeg directly in your browser. Convert formats, edit metadata,
							compress files, and process videos, audio, images, subtitles, and streams—all without
							installation.
						</p>
						<div class="mt-12 flex items-center gap-2">
							<div
								class="border-primary border bg-transparent p-0.5"
								style="border-radius: calc(var(--radius) + 4px + 0.125rem);"
							>
								<Button size="lg" class="cursor-pointer rounded-xl px-5 text-base">
									<a href="/">
										<span class="text-nowrap">Get Started Now</span>
									</a>
								</Button>
							</div>
							<Button
								size="lg"
								variant="ghost"
								class="h-10.5 cursor-pointer rounded-xl px-5 text-base"
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
				<div class="mx-auto mt-12 grid max-w-7xl grid-cols-1 gap-8 px-6 md:grid-cols-2">
					<div class="flex min-h-[calc(100vh-10rem)] flex-col gap-8 overflow-y-auto pb-8">
						<div
							class="ring-ring bg-card rounded-radius border-border shadow-shadow relative flex h-[15rem] w-full cursor-pointer items-center justify-center border p-4 shadow-lg ring-1 transition-transform duration-500 ease-in-out"
							in:scale={{ duration: 500, easing: cubicOut }}
						>
							<input
								type="file"
								onchange={handleFileUpload}
								aria-label="Upload a file"
								class="pointer-events-auto absolute inset-0 z-20 h-full w-full cursor-pointer opacity-0"
							/>
							{#if errorMessage}
								<p class="text-destructive text-center">{errorMessage}</p>
							{:else if isLoading}
								<div class="flex flex-col items-center justify-center gap-2">
									<LoaderCircle class="text-primary size-12 animate-spin" />
									<p class="text-muted-foreground text-center">Uploading..</p>
								</div>
							{:else if fileName}
								<div class="flex w-full flex-row items-center justify-between gap-4 px-12">
									<div class="flex flex-col items-center justify-center gap-2">
										<FileUp class="text-primary size-12" />
										<p class="text-muted-foreground">{fileName}</p>
									</div>
									<div class="text-muted-foreground flex flex-col gap-1 text-right text-sm">
										<p>Resolution: 1920x1080</p>
										<p>Format: {selectedFile?.type || 'Unknown'}</p>
										<p>Size: {(selectedFile?.size / 1024 / 1024).toFixed(2)} MB</p>
									</div>
								</div>
							{:else}
								<div class="flex flex-col items-center justify-center gap-2">
									<FileUp class="text-primary size-12" />
									<p class="text-muted-foreground text-center">
										Drag and drop or click to upload a file
									</p>
								</div>
							{/if}
						</div>
						{#each Object.entries(options) as [key, option]}
            <OptionSelector
              variant={option.variant}
              name={option.name}
              Icon={option.Icon}
              tabs={option.tabs}
              bind:value={ffmpegOptions[key]}
            />
          {/each}
					</div>
					<div class="flex min-h-[calc(100vh-10rem)] flex-col gap-4">
						<TerminalInput command={ffmpegCommand} />
						<!-- process buton -->
						<Button
							variant="default"
							size="lg"
							class="h-10.5 cursor-pointer rounded-xl px-5 text-base"
							onclick={processMedia}
						>
							<span class="text-nowrap">Process</span>
						</Button>
					</div>
				</div>
			{:else}
				<div
					class="ring-ring bg-card hover:ring-transparent hover:border-transparent hover:shadow-none hover:bg-primary/10 rounded-radius border-border shadow-shadow relative mx-auto mt-12 flex h-[15rem] max-w-2xl cursor-pointer flex-col items-center justify-center border p-4 shadow-lg ring-1 transition-all duration-150 ease-in-out"
					in:scale={{ duration: 500, easing: cubicOut }}
					out:slide={{ duration: 500, easing: cubicOut }}
				>
					<input
						type="file"
						onchange={handleFileUpload}
						aria-label="Upload a file"
						class="pointer-events-auto absolute inset-0 z-20 h-full w-full cursor-pointer opacity-0"
					/>
					{#if errorMessage}
						<p class="text-destructive text-center">{errorMessage}</p>
					{:else if isLoading}
						<div class="flex flex-col items-center justify-center gap-2">
							<LoaderCircle class="text-primary size-12 animate-spin" />
							<p class="text-muted-foreground text-center">Uploading..</p>
						</div>
					{:else if fileName}
						<div class="flex flex-col items-center justify-center gap-2">
							<FileUp class="text-primary size-12" />
							<p class="text-muted-foreground text-center">{fileName}</p>
						</div>
					{:else}
						<div class="flex flex-col items-center justify-center gap-2">
							<FileUp class="text-primary size-12" />
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
