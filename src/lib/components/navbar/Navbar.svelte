<script lang='ts'>
  import Button from "../jsrepo/button/button.svelte";

  import { fly } from "svelte/transition";

  import { Menu, X } from "@lucide/svelte";
  import { scrollY } from "svelte/reactivity/window";

  let menuItems = [
    { name: "Features", href: "/" },
    { name: "Solution", href: "/" },
    { name: "Pricing", href: "/" },
    { name: "About", href: "/" },
  ];
  let menuState = $state(false);
  let isScrolled = $derived.by(() => {
    if (scrollY.current !== undefined && scrollY.current > 50) {
      return true;
    }
    return false;
  });
</script>

<header>
    <nav class="fixed z-[999] w-full px-2" transition:fly={{ y: "-100%", duration: 300 }}>
      <div
        class={[
          "mx-auto mt-2 max-w-6xl px-6 transition-all duration-300 lg:px-12 rounded-radius border border-transparent",
          isScrolled &&
            "bg-accent border border-border backdrop-blur-lg lg:px-5",
        ]}
      >
        <div
          class="relative flex flex-wrap items-center justify-between gap-6 py-3 lg:gap-0 lg:py-4"
        >
          <div class="flex w-full justify-between lg:w-auto">
            <a href="/" aria-label="home" class="flex items-center space-x-2 group">
              <img src="/Logo.png" alt="Psychonaut Wrapped Logo" class="size-8 group-hover:scale-105 transition-transform duration-150"/>
            </a>

            <button
              onclick={() => (menuState = !menuState)}
              aria-label={menuState == true ? "Close Menu" : "Open Menu"}
              class="relative z-20 -m-2.5 -mr-4 block cursor-pointer p-2.5 lg:hidden"
            >
              <Menu
                class={[
                  "m-auto size-6 duration-200",
                  menuState && "rotate-180 scale-0 opacity-0",
                ]}
              />
              <X
                class={[
                  "absolute inset-0 m-auto size-6 -rotate-180 scale-0 opacity-0 duration-200",
                  menuState && "rotate-0 scale-100 opacity-100",
                ]}
              />
            </button>
          </div>

          <div class="absolute inset-0 m-auto hidden size-fit lg:block">
            <ul class="flex gap-8 text-sm">
              {#each menuItems as item, index}
                <li>
                  <a
                    href={item.href}
                    class="text-foreground-2 hover:text-foreground-1 block duration-150"
                  >
                    <span>{item.name}</span>
                  </a>
                </li>
              {/each}
            </ul>
          </div>
          <div
            class={[
              "bg-background-1 mb-6  w-full flex-wrap items-center justify-end space-y-8 rounded-3xl border border-border p-6 shadow-2xl shadow-shadow md:flex-nowrap lg:m-0 lg:flex lg:w-fit lg:gap-6 lg:space-y-0 lg:border-transparent lg:bg-transparent lg:p-0 lg:shadow-none",
              menuState ? "block lg:flex" : "hidden lg:flex",
            ]}
          >
            <div class="lg:hidden">
              <ul class="space-y-6 text-base">
                {#each menuItems as item, index}
                  <li>
                    <a
                      href={item.href}
                      class="text-foreground-2 hover:text-foreground-1 block duration-150"
                    >
                      <span>{item.name}</span>
                    </a>
                  </li>
                {/each}
              </ul>
            </div>
            <div
              class="flex w-full flex-col space-y-3 sm:flex-row sm:gap-3 sm:space-y-0 md:w-fit"
            >
              <Button
                variant="outline"
                size="sm"
                href="/auth/login"
              >
                Login
              </Button>
              <Button href="/auth/register" size="sm">
                Register
              </Button>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </header>