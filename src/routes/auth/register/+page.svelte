<script>
    import Button from "$lib/components/ui/button/button.svelte";
    import Input from "$lib/components/ui/input/input.svelte";
    import Label from "$lib/components/ui/label/label.svelte";
    import { fade } from "svelte/transition";

  </script>
  
  <section
    class="flex min-h-screen px-4 py-16 md:py-32 relative z-10"
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
            <img
              src="/Logo.png"
              alt="FFmpregger Logo"
              class="size-12 group-hover:scale-105 transition-transform duration-150">
          </a>
          <h1 class="mb-1 mt-4 text-xl font-semibold">
            Create a FFmpregger Account
          </h1>
          <p class="text-sm">Welcome! Create an account to get started</p>
        </div>
  
        <div class="mt-6 space-y-6">
          <div class="grid grid-cols-2 gap-3">
            <div class="space-y-2">
              <Label for="firstname" class="block text-sm">Firstname</Label>
              <Input type="text" required name="firstname" id="firstname" />
            </div>
            <div class="space-y-2">
              <Label for="lastname" class="block text-sm">Lastname</Label>
              <Input type="text" required name="lastname" id="lastname" />
            </div>
          </div>
  
          <div class="space-y-2">
            <Label for="email" class="block text-sm">Username</Label>
            <Input type="email" required name="email" id="email" />
          </div>
  
          <div class="space-y-0.5">
            <div class="flex items-center justify-between">
              <Label for="pwd" class="text-sm">Password</Label>
              <Button
                variant="link"
                size="sm"
                class="text-sm"
              >
                Forgot your Password ?
              </Button>
            </div>
            <Input
              type="password"
              required
              name="pwd"
              id="pwd"
            />
          </div>
  
          <Button class="w-full">Sign In</Button>
        </div>
  
      <div class="p-3">
        <p class="text-foreground-2 text-center text-sm">
          Have an account ?
          <Button variant="link" class="px-2">Sign In</Button>
        </p>
      </div>
    </form>
  </section>
  