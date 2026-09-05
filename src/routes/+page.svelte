<script lang="ts">
    import * as Menubar from "$lib/components/ui/menubar/index.js";
    import { PersistedState } from "runed";
    import { Text } from "$lib/components/ui/text/index.js";
    import { Progress } from "$lib/components/ui/progress/index.js";
    import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
    import * as Card from "$lib/components/ui/card/index.js";

    import SunIcon from "@lucide/svelte/icons/sun";
    import MoonIcon from "@lucide/svelte/icons/moon";
    import ChevronRightIcon from "@lucide/svelte/icons/chevron-right";
    import ChevronLeftIcon from "@lucide/svelte/icons/chevron-left";

    import { toggleMode } from "mode-watcher";
    import { Button, buttonVariants } from "$lib/components/ui/button/index.js";
    import * as ButtonGroup from "$lib/components/ui/button-group/index.js";

    import * as Drawer from "$lib/components/ui/drawer/index.js";

    import InfoGauge from "./components/infogauge.svelte";

    import * as ExhaustionEffect from "$lib/data/exhaustion_effect.json";
    import CombatTool from "./components/combattool.svelte";
    import ExplorationRef from "./components/explorationref.svelte";
    import DefenseTable from "./components/defensetable.svelte";
    import DamagePhase from "./components/damagephase.svelte";
    import HitLocationTables from "./components/hitlocationtables.svelte";
    import CritsTable from "./components/critstable.svelte";
    import { popupStates } from "./state.svelte";
    import DifficultyTable from "./components/difficultytable.svelte";
    import ConditionsTable from "./components/conditionstable.svelte";
    import AboutWindow from "./components/about.svelte";
    import FumbleTable from "./components/fumbletable.svelte";
    import TrapsTable from "./components/trapstable.svelte";

    let maxHealth = new PersistedState("maxHealth", 15);
    let currentHealth = new PersistedState("currentHealth", 15);

    let maxToughness = new PersistedState("maxToughness", 15);
    let currentToughness = new PersistedState("currentToughness", 15);

    let maxAether = new PersistedState("maxAether", 15);
    let currentAether = new PersistedState("currentAether", 15);

    let maxSanity = new PersistedState("maxSanity", 15);
    let currentSanity = new PersistedState("currentSanity", 15);

    let exhaustion = new PersistedState("exhaustion", 0);
    let light = new PersistedState("light", 0);

    let exhaustionEffectActive = $state(false);
    let exhaustionEffectText = $state("");

    let currentExperience = new PersistedState("experience", 0);

    let currentTensionDie = new PersistedState("tensionDie", "d8");
    let currentLairDie = new PersistedState("lairDie", "d8");

    function changeHealth(amount: number) {
        currentHealth.current = Math.min(
            maxHealth.current,
            Math.max(0, currentHealth.current + amount),
        );
    }

    function changeToughness(amount: number) {
        currentToughness.current = Math.min(
            maxToughness.current,
            Math.max(0, currentToughness.current + amount),
        );
    }

    function changeAether(amount: number) {
        currentAether.current = Math.min(
            maxAether.current,
            Math.max(0, currentAether.current + amount),
        );
    }

    function changeSanity(amount: number) {
        currentSanity.current = Math.min(
            maxSanity.current,
            Math.max(0, currentSanity.current + amount),
        );
    }

    function changeExhaustion(amount: number) {
        exhaustion.current = Math.max(0, exhaustion.current + amount);

        for (var e of ExhaustionEffect.effects) {
            if (
                e.lowThreshold <= exhaustion.current &&
                e.highThreshold >= exhaustion.current
            ) {
                exhaustionEffectActive = true;
                exhaustionEffectText = e.effect;
                return;
            }
        }

        exhaustionEffectActive = false;
    }

    function changeLight(amount: number) {
        light.current = Math.max(0, light.current + amount);
    }

    function changeExperience(amount: number) {
        currentExperience.current = Math.max(
            0,
            currentExperience.current + amount,
        );

        if (currentExperience.current >= 1000)
            currentExperience.current -= 1000;
    }
</script>

<Menubar.Root>
    <div class="flex justify-center my-2">
        <Button
            value="crits"
            onclick={() => {
                popupStates.isCritsPopupShown = true;
            }}>Crits</Button
        >
        <Button
            value="condition"
            onclick={() => {
                popupStates.isConditionPopupShown = true;
            }}>Conditions</Button
        >
    </div>
    <Menubar.Menu>
        <Menubar.Trigger>Tables</Menubar.Trigger>
        <Menubar.Content>
            <Menubar.Item
                onSelect={() => {
                    popupStates.isHitLocationPopupShown = true;
                }}>Hit Locations</Menubar.Item
            >
            <Menubar.Item
                onSelect={() => {
                    popupStates.isDefensivePopupShown = true;
                }}>Defense Tables</Menubar.Item
            >
            <Menubar.Item
                onSelect={() => {
                    popupStates.isFumblePopupShown = true;
                }}>Fumbles</Menubar.Item
            >
            <Menubar.Item
                onSelect={() => {
                    popupStates.isTrapsPopupShown = true;
                }}>Traps</Menubar.Item
            >
        </Menubar.Content>
    </Menubar.Menu>
    <div class="grow"></div>
    <Button
        onclick={() => {
            popupStates.isAboutPopupShown = true;
        }}
        variant="outline"
        size="icon">?</Button
    >
    <Button onclick={toggleMode} variant="outline" size="icon">
        <SunIcon
            class="h-[1.2rem] w-[1.2rem] scale-100 rotate-0 transition-all! dark:scale-0 dark:-rotate-90"
        />
        <MoonIcon
            class="absolute h-[1.2rem] w-[1.2rem] scale-0 rotate-90 transition-all! dark:scale-100 dark:rotate-0"
        />
        <span class="sr-only">Toggle theme</span>
    </Button>
</Menubar.Root>

<div class="flex flex-col items-center mx-2">
    <div class="w-full">
        <!-- health bar -->
        <InfoGauge
            label="Health"
            height={10}
            currentValue={currentHealth.current}
            maxValue={maxHealth.current}
            onchangefunc={changeHealth}
            onmaxchangedfunc={(newVal: number) => {
                maxHealth.current = newVal;
                if (maxHealth.current < currentHealth.current) {
                    currentHealth.current = maxHealth.current;
                }
            }}
            classAddition="**:data-[slot=progress-indicator]:bg-red-500"
        />
        <!-- toughness bar -->
        <InfoGauge
            label="Toughness"
            height={10}
            currentValue={currentToughness.current}
            maxValue={maxToughness.current}
            onchangefunc={changeToughness}
            onmaxchangedfunc={(newVal: number) => {
                maxToughness.current = newVal;
                if (maxToughness.current < currentToughness.current) {
                    currentToughness.current = maxToughness.current;
                }
            }}
            classAddition="**:data-[slot=progress-indicator]:bg-green-700"
        />
        <!-- aether bar -->
        <InfoGauge
            label="Aether"
            height={10}
            currentValue={currentAether.current}
            maxValue={maxAether.current}
            onchangefunc={changeAether}
            onmaxchangedfunc={(newVal: number) => {
                maxAether.current = newVal;
                if (maxAether.current < currentAether.current) {
                    currentAether.current = maxAether.current;
                }
            }}
            classAddition="**:data-[slot=progress-indicator]:bg-blue-500"
        />
        <!-- sanity bar -->
        <InfoGauge
            label="Sanity"
            height={10}
            currentValue={currentSanity.current}
            maxValue={maxSanity.current}
            onchangefunc={changeSanity}
            onmaxchangedfunc={(newVal: number) => {
                maxSanity.current = newVal;
                if (maxSanity.current < currentSanity.current) {
                    currentSanity.current = maxSanity.current;
                }
            }}
            classAddition="**:data-[slot=progress-indicator]:bg-purple-400"
        />
    </div>

    <div class="flex flex-row justify-between w-full">
        <div class="border-2 w-full m-1">
            <Text class="text-center w-full text-xs">Tension Die</Text>
            <ToggleGroup.Root
                type="single"
                bind:value={currentTensionDie.current}
                class="w-full justify-center"
            >
                <ToggleGroup.Item value="d10" aria-label="Toggle bold"
                    >(D10)</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d8" aria-label="Toggle bold"
                    >D8</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d6" aria-label="Toggle bold"
                    >D6</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d4" aria-label="Toggle bold"
                    >D4</ToggleGroup.Item
                >
            </ToggleGroup.Root>
        </div>
        <div class="border-2 w-full m-1">
            <Text class="text-center text-xs">Lair (d10) - Domain (d8)</Text>
            <ToggleGroup.Root
                type="single"
                bind:value={currentLairDie.current}
                class="w-full justify-center"
            >
                <ToggleGroup.Item value="d10" aria-label="Toggle bold"
                    >D10</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d8" aria-label="Toggle bold"
                    >D8</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d6" aria-label="Toggle bold"
                    >D6</ToggleGroup.Item
                >
                <ToggleGroup.Item value="d4" aria-label="Toggle bold"
                    >D4</ToggleGroup.Item
                >
            </ToggleGroup.Root>
        </div>
    </div>

    <div class="grid grid-cols-2 gap-2 h-50">
        <div class="flex flex-col items-start w-full grow">
            <div class="flex flex-row items-center w-full">
                <Button size="icon" onclick={() => changeExhaustion(-1)}>
                    <ChevronLeftIcon />
                </Button>
                <div class="mx-4">
                    <Text class="text-center">Exhaustion</Text>
                    <Text class="text-center" as="h4">{exhaustion.current}</Text
                    >
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
                    <Text class="text-center" as="h4">{light.current}</Text>
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

            {#if light.current == 0}
                <Card.Root class="grow mt-2 w-full">
                    <Card.Content>
                        <p><b>Blinded</b></p>
                        <br />
                        <p><b>-1 sanity</b>/room</p>
                    </Card.Content>
                </Card.Root>
            {/if}
        </div>
    </div>

    <div class="w-full flex flex-col gap-1">
        <!-- <div class="grow relative px-1 h-6 w-full" role="button" tabindex=0>
            <Progress
                value={currentExperience.current}
                max={1000}
                class="grow h-full"
            />
            <Text
                class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
            >
                Exp:{currentExperience.current}/1000
            </Text>
        </div> -->
        <InfoGauge
            label="Experience"
            height={10}
            currentValue={currentExperience.current}
            maxValue={1000}
            onchangefunc={changeExperience}
            onmaxchangedfunc={() => {}}
            maxEditable={false}
            classAddition="**:data-[slot=progress-indicator]:bg-yellow-600"
        />
        <div class="grid w-full grid-flow-row grid-cols-2">
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(10);
                }}
            >
                Door/Container Unlocked
            </Button>
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(10);
                }}
            >
                Trap Disarmed
            </Button>
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(10);
                }}
            >
                Lore Book Found
            </Button>
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(50);
                }}
            >
                New Domain Entered
            </Button>
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(50);
                }}
            >
                Regular Combat Won
            </Button>
            <Button
                class="flex"
                onclick={() => {
                    changeExperience(200);
                }}
            >
                Overseer Defeated
            </Button>
        </div>
    </div>

    <Drawer.Root modal={false}>
        <Drawer.Trigger
            class={[
                buttonVariants({ variant: "outline" }),
                "fixed bottom-0 left-5 w-[40%] h-10",
            ]}>Combat</Drawer.Trigger
        >
        <Drawer.Content>
            <CombatTool
                xpGranted={(amount: number) => {
                    changeExperience(amount);
                }}
            ></CombatTool>
        </Drawer.Content>
    </Drawer.Root>

    <Drawer.Root modal={false}>
        <Drawer.Trigger
            class={[
                buttonVariants({ variant: "outline" }),
                "fixed bottom-0 right-5 w-[40%] h-10",
            ]}>Exploration</Drawer.Trigger
        >
        <Drawer.Content class="h-[90%]">
            <ExplorationRef></ExplorationRef>
        </Drawer.Content>
    </Drawer.Root>

    <DefenseTable />
    <DamagePhase />
    <HitLocationTables />
    <CritsTable />
    <DifficultyTable />
    <ConditionsTable />
    <FumbleTable />
    <TrapsTable />
    <AboutWindow />
</div>
