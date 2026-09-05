<script lang="ts">
    import Button from "$lib/components/ui/button/button.svelte";
    import ScrollArea from "$lib/components/ui/scroll-area/scroll-area.svelte";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import Text from "$lib/components/ui/text/text.svelte";
    import { popupStates } from "../state.svelte";
</script>

<div class="h-full p-5">
    <Tabs.Root value="door-container" class="h-full">
        <Tabs.List class="w-full">
            <Tabs.Trigger value="door-container">Door/Container</Tabs.Trigger>
            <Tabs.Trigger value="explore">Explore</Tabs.Trigger>
        </Tabs.List>
        <Tabs.Content value="door-container" class="w-full h-full">
            <ScrollArea class="h-full">
                <div class="my-6">
                    <Text>Before interacting with a door or container:</Text>
                    <Text><b>1. Check For Trap</b></Text>
                    <Text class="ml-5"
                        >Perception vs <Button
                            variant="link"
                            onclick={() => {
                                popupStates.isDifficultyPopupShown = true;
                            }}>Random Difficulty</Button
                        ></Text
                    >
                    <Text class="ml-10"
                        ><b>PASS</b>: Roll d10 → 7+ trapped, roll on <Button
                            variant="link"
                            onclick={() => {
                                popupStates.isTrapsPopupShown = true;
                            }}>Trap Table</Button
                        ></Text
                    >
                    <Text class="ml-10"
                        ><b>FAIL</b>: if interact, as above but force trigger</Text
                    >
                    <Text><b>2. Check If Locked : Roll D20</b></Text>
                    <Text class="ml-5">Door → 12+ Locked</Text>
                    <Text class="ml-5">Container → 10+ Locked</Text>
                    <br />
                    <Text><b>Disarm Trap : Need Thieves' Tools</b></Text>
                    <Text class="ml-5"
                        >Thievery vs <Button
                            variant="link"
                            onclick={() => {
                                popupStates.isDifficultyPopupShown = true;
                            }}>Random Difficulty</Button
                        ></Text
                    >
                    <Text class="ml-10"><b>PASS</b>: Disarmed</Text>
                    <Text class="ml-10"><b>FAIL</b>: Trigger Trap</Text>
                    <Text><b>Bypass Trap (Environmental) or Trigger It</b></Text
                    >
                    <Text class="ml-5"
                        >Skill check for that trap w/ +20. If Triggered,
                        disarmed. On <b>Fail</b> suffer trap</Text
                    >
                    <br />
                    <Text><b>Locked Door/Container</b></Text>
                    <Text class="ml-5"
                        >Require Lockpick: Thievery vs <Button
                            variant="link"
                            onclick={() => {
                                popupStates.isDifficultyPopupShown = true;
                            }}>Random Difficulty</Button
                        ></Text
                    >
                    <Text class="ml-10"
                        ><b>FAIL</b>: Lockpick break, difficulty reduced one
                        step</Text
                    >
                    <Text class="ml-5"
                        >Break it : Athletic Check. <b
                            >CREATE NOISE : Tension Dice Check</b
                        ></Text
                    >
                    <Text class="ml-10"
                        ><b>FAIL</b>: Add +20 to your skill for each next
                        attempt
                    </Text>
                </div>
            </ScrollArea>
        </Tabs.Content>
        <Tabs.Content value="explore" class="w-full h-full">
            <ScrollArea class="h-full">
                <Text><b>1.</b> Roll D100 (p.97) :</Text>
                <Text class="ml-5">1-25 → Corridor</Text>
                <Text class="ml-5">26-100 → room</Text>
                <Text><b>2.</b> Roll Usage Die :</Text>
                <Text class="ml-5"
                    >Overseer Lair Die is not Lair not found yet</Text
                >
                <Text class="ml-5">Exit Lair Die otherwise</Text>
                <Text
                    ><b>3.</b> Roll Tension Die. Roll on Growing Darkness p.120 when
                    exhausted</Text
                >
                <Text class="ml-5">Reduce Lightsource by 1</Text>
                <Text><b>4.</b> Roll Combat Die D20</Text>
                <Text class="ml-5">Room → 10+ = Combat</Text>
                <Text class="ml-5">Corridor → 15+ = Combat</Text>
                <Text class="ml-5">Otherwise, Roll Event (p.108)</Text>
            </ScrollArea>
        </Tabs.Content>
    </Tabs.Root>
</div>
