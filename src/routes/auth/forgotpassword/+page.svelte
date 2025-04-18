<script lang="ts">
    import Button from "@/components/ui/button/button.svelte";
    import Input from "@/components/ui/input/input.svelte";
    import Label from "@/components/ui/label/label.svelte";
    import * as InputOTP from "$lib/components/ui/input-otp/index.js";
    let emailSent: boolean = $state(false);
    let emailConfirmed: boolean = $state(false);
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
                <h1 class="mb-1 mt-4 text-xl font-semibold">Forgot your password?</h1>
                <p class="text-sm">Enter your Email to reset your password</p>
            </div>

            <div class="mt-6 space-y-6">
                {#if !emailSent}
                    <div class="space-y-2">
                        <Label for="email" class="block text-sm">Email</Label>
                        <Input type="email" required name="email" id="email" />
                    </div>
                    <Button class="w-full" onclick={() => emailSent = !emailSent}>Send Email</Button>
                {:else}
                    {#if !emailConfirmed}
                    <div class="space-y-2">
                        <Label for="email" class="block text-sm">Code</Label>
                        <div class="flex justify-center">
                            <InputOTP.Root maxlength={6}>
                                {#snippet children({ cells })}
                                <InputOTP.Group>
                                    {#each cells.slice(0, 3) as cell (cell)}
                                    <InputOTP.Slot {cell} />
                                    {/each}
                                </InputOTP.Group>
                                <InputOTP.Separator />
                                <InputOTP.Group>
                                    {#each cells.slice(3, 6) as cell (cell)}
                                    <InputOTP.Slot {cell} />
                                    {/each}
                                </InputOTP.Group>
                                {/snippet}
                            </InputOTP.Root>
                        </div>
                    </div>
                    <Button class="w-full" onclick={() => emailConfirmed = !emailConfirmed}>Confirm Email</Button>
                    {:else}
                    <div class="space-y-2">
                        <Label for="newPassword" class="block text-sm">New Password</Label>
                        <Input type="password" required name="newPassword" id="newPassword" />
                    </div>
                    <div class="space-y-2">
                        <Label for="confirmPassword" class="block text-sm">Confirm Password</Label>
                        <Input type="password" required name="confirmPassword" id="confirmPassword" />
                    </div>
                    <Button class="w-full" onclick={() => emailConfirmed = !emailConfirmed}>Change Password</Button>
                    {/if}
                {/if}
            </div>

            <div class="p-3">
                <p class="text-accent-foreground text-center text-sm">
                    Remember your password?
                    <Button href="/auth/login" variant="link" class="px-2">Back to login</Button>
                </p>
            </div>
        </div>
    </form>
</section>