<script lang="ts" module>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Card from "$lib/components/ui/card/index.js";
    import Text from "$lib/components/ui/text/text.svelte";
    import InfoGauge from "./infogauge.svelte";
    import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";

    
    interface EnemyData {
        currentHealth: number,
        maxHealth: number
    }

    let currentEnemies: EnemyData[] = $state([]);
    
    let currentCombatStage = $state(0);
    let stealthAttempted = $state(false);
    let stealthWon = $state(false);
    let initiativeWon = $state(false);
    
    function addEnemy() {
        currentEnemies.push({maxHealth: 10, currentHealth: 10} as EnemyData)
    }
    
    function changeEnemyHealth(enemy: EnemyData, amount: number) {
        enemy.currentHealth = Math.max(0, Math.min(enemy.maxHealth, enemy.currentHealth + amount));
    }
    
    function changeEnemyMaxHealth(enemy: EnemyData, newMax: number) {
        enemy.maxHealth = newMax;
        if(newMax < enemy.currentHealth) {
            enemy.currentHealth = newMax;
        }
    }
    
    function startNewFight() {
        currentCombatStage = 0;
        currentEnemies = [];
    }
</script>

<script lang="ts">

</script>

<div class="h-[50vh] grid grid-rows-2">
    <Card.Root class="h-full">
        <Card.Content class="flex flex-col items-center h-full">
        {#each currentEnemies as e}
                <InfoGauge 
                    currentValue={e.currentHealth} 
                    maxValue={e.maxHealth} 
                    label="Health" 
                    onchangefunc={(change) => changeEnemyHealth(e, change)}
                    onmaxchangedfunc={(newVal) => {changeEnemyMaxHealth(e, newVal)}}/>
        {/each}
            <Button onclick={addEnemy}>New Enemy</Button>
        </Card.Content>
    </Card.Root>
    <Card.Root>
        <Card.Content class="h-[25vh]">
            <ScrollArea class="h-[160px] w-full">
                <div class="h-fit">
                    {#if currentCombatStage == 0}
                        {#if !stealthAttempted}
                            <Text><b>Attempt Surprise:</b> Stealth vs Enemy Mind.</Text>
                            <div class="flex flex-row">
                                <Button onclick={() => { initiativeWon = true; stealthWon = true; currentCombatStage = 1; }}>Succeded</Button>
                                <Button onclick={() => { stealthAttempted = true; stealthWon = false; }}>Failed</Button>
                            </div>
                        {/if}
                        <Text><b>Initiative Check:</b> Perception {stealthAttempted && !stealthWon ? "(-20, stealth failed)" : "" } vs Enemy Mind.</Text>
                        <div class="flex flex-row">
                            <Button onclick={ () => { initiativeWon = true; currentCombatStage = 1; }}>Succeded</Button>
                            <Button onclick={ () => { initiativeWon = false; currentCombatStage = 1; }}>Failed</Button>
                        </div>
                    {:else if currentCombatStage == 1}
                        <Text>Initiative Winner: {initiativeWon ? "Player" : "Enemies"}</Text>
                        {#if stealthWon}
                        <Text>Stealth Won: +20 on first attack</Text>
                        {/if}
                        <Text>Melee → weapon skill vs combat skill.</Text>
                        <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Win</b>: compute damage</Text>
                        <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Loose</b>: Defender roll on defensive table</Text>
                        <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Both</b> Fail: Defender suffer 1 damage</Text>
                        <Text>Magic → Spellward or Magic Resistance check</Text>
                        <Button onclick={() => {currentCombatStage = 2;}}>End Fight</Button>
                    {:else}
                        <Text>Recover 1D4 Toughness</Text>
                        <Button onclick={startNewFight}>New Fight</Button>
                    {/if}
                </div>
            </ScrollArea>
        </Card.Content>
    </Card.Root>
</div>