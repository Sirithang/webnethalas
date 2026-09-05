<script lang="ts" module>
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Table from "$lib/components/ui/table/index.js";

    import { popupStates } from "../state.svelte.js";
    import * as PlayerFumbles from "$lib/data/fumble_player.json";
    import * as NPCFumbles from "$lib/data/fumble_npc.json";
</script>

<script lang="ts">
</script>

<Dialog.Root bind:open={popupStates.isFumblePopupShown}>
    <Dialog.Content class="grid-cols-[100%]">
        <Dialog.Header>
            <Dialog.Title>Fumble Table</Dialog.Title>
        </Dialog.Header>
        <Tabs.Root value="player">
            <Tabs.List class="w-full">
                <Tabs.Trigger value="player">Player</Tabs.Trigger>
                <Tabs.Trigger value="npc-monster">NPC & Monster</Tabs.Trigger>
            </Tabs.List>
            <Tabs.Content value="player" class="w-full">
                <Table.Root class="w-full">
                    <Table.Header class="w-full">
                        <Table.Row>
                            <Table.Head class="w-0.1">d10</Table.Head>
                            <Table.Head class="w-0.1">Result</Table.Head>
                        </Table.Row>
                    </Table.Header>
                    <Table.Body class="w-full">
                        {#each PlayerFumbles.fumbles as m, i}
                            <Table.Row>
                                <Table.Cell>{i + 1}</Table.Cell>
                                <Table.Cell class="text-wrap whitespace-normal"
                                    >{m}</Table.Cell
                                >
                            </Table.Row>
                        {/each}
                    </Table.Body>
                </Table.Root>
            </Tabs.Content>
            <Tabs.Content value="npc-monster">
                <Table.Root class="w-full">
                    <Table.Header class="w-full">
                        <Table.Row>
                            <Table.Head class="w-0.1">d10</Table.Head>
                            <Table.Head class="w-0.1">Result</Table.Head>
                        </Table.Row>
                    </Table.Header>
                    <Table.Body class="w-full">
                        {#each NPCFumbles.fumbles as m, i}
                            <Table.Row>
                                <Table.Cell>{i + 1}</Table.Cell>
                                <Table.Cell class="text-wrap whitespace-normal"
                                    >{m}</Table.Cell
                                >
                            </Table.Row>
                        {/each}
                    </Table.Body>
                </Table.Root>
            </Tabs.Content>
        </Tabs.Root>
    </Dialog.Content>
</Dialog.Root>
