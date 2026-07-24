<script lang="ts">
    import * as Menubar from "$lib/components/ui/menubar/index.js";
    import { Progress } from "$lib/components/ui/progress/index.js";
    import { Text } from "$lib/components/ui/text/index.js";

    import SunIcon from "@lucide/svelte/icons/sun";
    import MoonIcon from "@lucide/svelte/icons/moon";
    import ChevronRightIcon from "@lucide/svelte/icons/chevron-right";
    import ChevronLeftIcon from "@lucide/svelte/icons/chevron-left";

    import { toggleMode } from "mode-watcher";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as ButtonGroup from "$lib/components/ui/button-group/index.js";
    import * as Card from "$lib/components/ui/card/index.js";
    
    import * as ExhaustionEffect from "$lib/data/exhaustion_effect.json";

    let maxHealth = $state(15);
    let currentHealth = $state(15);

    let maxToughness = $state(15);
    let currentToughness = $state(15);

    let maxAether = $state(15);
    let currentAether = $state(15);

    let maxSanity = $state(15);
    let currentSanity = $state(15);

    let exhaustion = $state(0);
    let light = $state(0);
    
    let exhaustionEffectActive = $state(false);
    let exhaustionEffectText = $state("");

    function changeHealth(amount: number) {
        currentHealth = Math.min(
            maxHealth,
            Math.max(0, currentHealth + amount),
        );
    }

    function changeToughness(amount: number) {
        currentToughness = Math.min(
            maxToughness,
            Math.max(0, currentToughness + amount),
        );
    }

    function changeAether(amount: number) {
        currentAether = Math.min(
            maxAether,
            Math.max(0, currentAether + amount),
        );
    }

    function changeSanity(amount: number) {
        currentSanity = Math.min(
            maxSanity,
            Math.max(0, currentSanity + amount),
        );
    }

    function changeExhaustion(amount: number) {
        exhaustion = Math.max(0, exhaustion + amount);
        
        for(var e of ExhaustionEffect.effects) {
            if(e.lowThreshold <= exhaustion && e.highThreshold >= exhaustion) {
                exhaustionEffectActive = true;
                exhaustionEffectText = e.effect;
                return;
            }
        }
        
        exhaustionEffectActive = false;
    }

    function changeLight(amount: number) {
        light = Math.max(0, light + amount);
    }
</script>

<Menubar.Root>
    <Menubar.Menu>
        <Menubar.Trigger>File</Menubar.Trigger>
        <Menubar.Content>
            <Menubar.Item>
                New Tab
                <Menubar.Shortcut>⌘T</Menubar.Shortcut>
            </Menubar.Item>
            <Menubar.Item>New Window</Menubar.Item>
            <Menubar.Separator />
            <Menubar.Item>Share</Menubar.Item>
            <Menubar.Separator />
            <Menubar.Item>Print</Menubar.Item>
        </Menubar.Content>
    </Menubar.Menu>
    <div class="grow"></div>
    <Button onclick={toggleMode} variant="outline" size="icon">
        <SunIcon
            class="h-[1.2rem] w-[1.2rem] scale-100 rotate-0 !transition-all dark:scale-0 dark:-rotate-90"
        />
        <MoonIcon
            class="absolute h-[1.2rem] w-[1.2rem] scale-0 rotate-90 !transition-all dark:scale-100 dark:rotate-0"
        />
        <span class="sr-only">Toggle theme</span>
    </Button>
</Menubar.Root>

<div class="flex flex-col items-center mx-2 my-4">
    <!-- health bar -->
    <div class="flex flex-row items-center w-full my-2 h-12">
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeHealth(-10)}
                class="h-full w-12">-10</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeHealth(-1)}
                class="h-full w-12">-1</Button
            >
        </ButtonGroup.Root>
        <div class="grow relative px-1 h-full">
            <Progress
                value={currentHealth}
                max={maxHealth}
                class="grow h-full **:data-[slot=progress-indicator]:bg-red-500"
            />
            <Text
                class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
            >
                Health:{currentHealth}/{maxHealth}
            </Text>
        </div>
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeHealth(1)}
                class="h-full  w-12">+1</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeHealth(10)}
                class="h-full w-12">+10</Button
            >
        </ButtonGroup.Root>
    </div>

    <!-- toughness bar -->
    <div class="flex flex-row items-center w-full my-2 h-12">
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeToughness(-10)}
                class="h-full w-12">-10</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeToughness(-1)}
                class="h-full w-12">-1</Button
            >
        </ButtonGroup.Root>
        <div class="grow h-full relative px-1">
            <Progress
                value={currentToughness}
                max={maxToughness}
                class="grow h-full **:data-[slot=progress-indicator]:bg-green-700"
            />
            <Text
                class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
            >
                Toughness:{currentToughness}/{maxToughness}
            </Text>
        </div>
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeToughness(1)}
                class="h-full w-12">+1</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeToughness(10)}
                class="h-full w-12">+10</Button
            >
        </ButtonGroup.Root>
    </div>

    <!-- aether bar -->
    <div class="flex flex-row items-center w-full my-2 h-12">
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeAether(-10)}
                class="h-full w-12">-10</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeAether(-1)}
                class="h-full w-12">-1</Button
            >
        </ButtonGroup.Root>
        <div class="grow h-16 relative px-1 h-full">
            <Progress
                value={currentAether}
                max={maxAether}
                class="grow h-full **:data-[slot=progress-indicator]:bg-blue-500"
            />
            <Text
                class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
            >
                Aether:{currentAether}/{maxAether}
            </Text>
        </div>
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeAether(1)}
                class="h-full w-12">+1</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeAether(10)}
                class="h-full w-12">+10</Button
            >
        </ButtonGroup.Root>
    </div>

    <!-- sanity bar -->
    <div class="flex flex-row items-center w-full my-2 h-12">
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeSanity(-10)}
                class="h-full w-12">-10</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeSanity(-1)}
                class="h-full w-12">-1</Button
            >
        </ButtonGroup.Root>
        <div class="grow h-full relative px-1">
            <Progress
                value={currentSanity}
                max={maxSanity}
                class="grow h-full **:data-[slot=progress-indicator]:bg-purple-400"
            />
            <Text
                class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
            >
                Sanity:{currentSanity}/{maxSanity}
            </Text>
        </div>
        <ButtonGroup.Root class="shrink px-1 h-full">
            <Button
                variant="secondary"
                onclick={() => changeSanity(1)}
                class="h-full w-12">+1</Button
            >
            <Button
                variant="secondary"
                onclick={() => changeSanity(10)}
                class="h-full w-12">+10</Button
            >
        </ButtonGroup.Root>
    </div>

    <div class="grid grid-cols-2 gap-2 h-50">
        <div class="flex flex-col items-start w-full grow">
            <div class="flex flex-row items-center w-full">
                <Button size="icon" onclick={() => changeExhaustion(-1)}>
                    <ChevronLeftIcon />
                </Button>
                <div class="mx-4">
                    <Text class="text-center">Exhaustion</Text>
                    <Text class="text-center" as="h4">{exhaustion}</Text>
                </div>
                <Button size="icon" onclick={() => changeExhaustion(1)}>
                    <ChevronRightIcon />
                </Button>
            </div>
            {#if exhaustionEffectActive}
            <Card.Root class="w-full mt-2">
                <Card.Content>
                    <p>{exhaustionEffectText}</p>
                </Card.Content>
            </Card.Root>
            {/if}
        </div>

        <div class="flex flex-col items-start w-full">
            <div class="flex flex-row items-center w-full">
                <Button size="icon" onclick={() => changeLight(-1)}>
                    <ChevronLeftIcon />
                </Button>
                <div class="grow">
                    <Text class="text-center">Light</Text>
                    <Text class="text-center" as="h4">{light}</Text>
                </div>
                <Button size="icon" onclick={() => changeLight(1)}>
                    <ChevronRightIcon />
                </Button>
            </div>
            
             <ButtonGroup.Root class="h-10 w-full">
                <Button
                    variant="secondary"
                    onclick={() => changeLight(10)}
                    class="h-full w-1/2">+10</Button
                >
                <Button
                    variant="secondary"
                    onclick={() => changeLight(20)}
                    class="h-full w-1/2">+20</Button
                >
            </ButtonGroup.Root>
            
            {#if light == 0}
            <Card.Root class="grow mt-2 mx-1">
                <Card.Content>
                    <p><b>Blinded</b>: Disadvantage non-Reason check, only basic attack, no targeting</p>
                    <br/>
                    <p><b>-1 sanity</b>/room</p>
                </Card.Content>
            </Card.Root>
            {/if}
        </div>
    </div>
</div>
