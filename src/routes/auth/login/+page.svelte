<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import Input from "$lib/components/ui/input/input.svelte";
  import Label from "$lib/components/ui/label/label.svelte";
</script>

<section
  class="flex min-h-screen bg-transparent px-4 py-16 md:py-32 relative z-10"
>
  <form
    action=""
    class="bg-accent m-auto h-fit w-full max-w-sm overflow-hidden rounded-radius border border-border shadow-md shadow-shadow"
  >
    <div
      class="bg-card -m-px rounded-radius border border-border p-8 pb-6"
    >
      <div class="text-center">
        <a href="/" aria-label="go home" class="mx-auto block w-fit group">
          <img src="/Logo.png" alt="FFmpreg Logo" class="size-12 group-hover:scale-105 transition-transform duration-150" />
        </a>
        <h1 class="mb-1 mt-4 text-xl font-semibold">Sign In to FFmpregger</h1>
        <p class="text-sm">Welcome back! Sign in to continue</p>
      </div>

      <div class="mt-6 space-y-6">
        <div class="space-y-2">
          <Label for="email" class="block text-sm">Username</Label>
          <Input type="email" required name="email" id="email" />
        </div>

        <div class="space-y-0.5">
          <div class="flex items-center justify-between">
            <Label for="pwd" class="text-title text-sm">Password</Label>
            <Button
              href="/auth/forgotpassword"
              variant="link"
              size="sm"
              class="link intent-info variant-ghost text-sm"
            >
              Forgot your Password ?
            </Button>
          </div>
          <Input
            type="password"
            required
            name="pwd"
            id="pwd"
            class="input sz-md variant-mixed"
          />
        </div>

        <Button class="w-full">Sign In</Button>
      </div>

    <div class="p-3">
      <p class="text-accent-foreground text-center text-sm">
        Don't have an account ?
        <Button href="/auth/register" variant="link" class="px-2">Create account</Button>
      </p>
    </div>
  </form>
</section>
