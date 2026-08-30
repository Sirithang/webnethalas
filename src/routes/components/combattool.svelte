<script lang="ts" module>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Card from "$lib/components/ui/card/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import Text from "$lib/components/ui/text/text.svelte";
    import InfoGauge from "./infogauge.svelte";
    import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
    import { popupStates } from '../state.svelte.js';
    import { PersistedState } from "runed";

    
    interface EnemyData {
        currentHealth: number,
        maxHealth: number
    }

    let currentEnemies = new PersistedState("enemiesData", [] as EnemyData[]);
    
    let currentCombatStage = new PersistedState("combatStage", 0);
    let stealthAttempted = new PersistedState("stealthAttempted", false);
    let stealthWon = new PersistedState("stealthWon", false);
    let initiativeWon = new PersistedState("initiativeWon", false);
    
    function addEnemy() {
        currentEnemies.current.push({maxHealth: 10, currentHealth: 10} as EnemyData)
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
        currentCombatStage.current = 0;
        currentEnemies.current = [];
    }
    
    
</script>

<script lang="ts">
let {
        xpGranted
    } = $props();
</script>

<div class="h-[55vh]">
    <Card.Root class="h-1/3">
        <Card.Content class="flex flex-col items-center h-full">
            <ScrollArea class="w-full h-full">
                <div class="flex flex-col items-center">
                {#each currentEnemies.current as e}
                <InfoGauge 
                    currentValue={e.currentHealth} 
                    maxValue={e.maxHealth} 
                    label="Health" 
                    height = {3}                          
                    onchangefunc={(change:number) => changeEnemyHealth(e, change)}
                        onmaxchangedfunc={(newVal:number) => {changeEnemyMaxHealth(e, newVal)}}/>
                {/each}
                <Button onclick={addEnemy}>New Enemy</Button>
                </div>
            </ScrollArea>
        </Card.Content>
    </Card.Root>
    <Card.Root>
        <Card.Content class="h-2/3">
            <ScrollArea class="w-full">
                <div class="h-fit">
                    {#if currentCombatStage.current == 0}
                        {#if !stealthAttempted.current}
                            <Text><b>Attempt Surprise:</b> Stealth vs Enemy Mind.</Text>
                            <div class="flex flex-row">
                                <Button onclick={() => { initiativeWon.current = true; stealthWon.current = true; currentCombatStage.current = 1; }}>Succeded</Button>
                                <Button onclick={() => { stealthAttempted.current = true; stealthWon.current = false; }}>Failed</Button>
                            </div>
                        {/if}
                        <Text><b>Initiative Check:</b> Perception {stealthAttempted.current && !stealthWon.current ? "(-20, stealth failed)" : "" } vs Enemy Mind.</Text>
                        <div class="flex flex-row">
                            <Button onclick={ () => { initiativeWon.current = true; currentCombatStage.current = 1; }}>Succeded</Button>
                            <Button onclick={ () => { initiativeWon.current = false; currentCombatStage.current = 1; }}>Failed</Button>
                        </div>
                    {:else if currentCombatStage.current == 1}
                        <div class="flex flex-row">
                            <Text class="grow">1st: {initiativeWon.current ? "Player" : "Enemies"}</Text>
                            {#if stealthWon.current}
                            <Text class="grow">|</Text>
                            <Text class="grow font-bold">Stealth Won:+20 on first attack</Text>
                            {/if}
                        </div>
                        
                        <Tabs.Root value="melee" class="w-full">
                        <Tabs.List class="flex w-full">
                            <Tabs.Trigger value="melee" >Melee</Tabs.Trigger>
                            <Tabs.Trigger value="magic">Magick</Tabs.Trigger>
                        </Tabs.List>
                        <Tabs.Content value="melee">
                            <Text>Melee → weapon skill vs (combat skill - weapon speed).</Text>
                            <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Win</b>: compute <Button variant="link" onclick={() => {popupStates.isDamageStepPopupShown = true}}>Damage</Button></Text>
                            <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Loose</b>: Defender roll on <Button variant="link" onclick={()=>{popupStates.isDefensivePopupShown = true;}}>defensive table</Button></Text>
                            <Text>&nbsp;&nbsp;&nbsp;&nbsp;<b>Both</b> Fail: Defender suffer 1 damage</Text>
                        </Tabs.Content>
                            <Tabs.Content value="magic"><Text>Magic → Spellward or Magic Resistance check</Text></Tabs.Content>
                        </Tabs.Root>
                        
                        <Button onclick={() => {currentCombatStage.current = 2;}}>End Fight</Button>
                    {:else}
                        <Text>Recover 1D4 Toughness</Text>
                        <div>
                            <Button onclick={() => { xpGranted(50); startNewFight();}}>Regular Fight Won(+50XP)</Button>
                            <Button onclick={() => { xpGranted(200); startNewFight(); }}>Overseer Defeated(+200XP)</Button>
                            <Button onclick={startNewFight}>New Fight</Button>
                        </div>
                    {/if}
                </div>
            </ScrollArea>
        </Card.Content>
    </Card.Root>
</div>