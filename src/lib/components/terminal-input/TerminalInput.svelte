<script lang="ts">
  import { Copy } from 'lucide-svelte';
  import { Button } from '$lib/components/ui/button';

  let { command = "" }: { command: string } = $props();
  let showCopied = $state(false);

  function copyToClipboard() {
    navigator.clipboard.writeText(command);
    showCopied = true;
    setTimeout(() => (showCopied = false), 2000);
  }
</script>

<div class="w-full max-w-3xl mx-auto">
  <div class="bg-card rounded-radius overflow-hidden border border-border hadow-lg shadow-shadow ">
    <div class="flex items-center px-4 py-2 bg-card border-b border-border text-foreground-2">
      <span class="ml-2 text-sm">Output</span>
      <div class="ml-auto flex items-center gap-2">
        {#if showCopied}
          <span class="text-xs text-green-10">Copied!</span>
        {/if}
        <Button
          variant="ghost"
          size="icon"
          class="h-8 w-8 hover:text-primary hover:bg-border cursor-pointer"
          onclick={copyToClipboard}
        >
          <Copy class="h-4 w-4" />
          <span class="sr-only">Copy</span>
        </Button>
      </div>
    </div>
    <div class="p-4">
      <textarea
        value={command}
        class="w-full bg-transparent font-Geist-Mono font-semibold text-sm resize-none outline-none border-none focus:ring-0 whitespace-pre-wrap break-words"
        rows="3"
        spellcheck="false"
        readonly
      ></textarea>
    </div>
  </div>
</div>