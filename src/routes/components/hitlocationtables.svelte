<script lang="ts" module>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Table from "$lib/components/ui/table/index.js";

    import { popupStates } from "../state.svelte.js";
    
    import * as HitLocationTables from "$lib/data/hitlocations.json"
</script>

<Dialog.Root bind:open={popupStates.isHitLocationPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Hit Locations</Dialog.Title>
        </Dialog.Header>

        <Tabs.Root value={HitLocationTables.tables[0].targetType} orientation="vertical">
            <Tabs.List class="w-0.2">
                {#each HitLocationTables.tables as t}
                    <Tabs.Trigger value={t.targetType}>{t.targetType}</Tabs.Trigger>
                {/each}
            </Tabs.List>
            {#each HitLocationTables.tables as t}
                <Tabs.Content value={t.targetType} class="w-full">
                    <Table.Root class="w-full">
                        <Table.Header class="w-full">
                            <Table.Row>
                                <Table.Head class="w-13">d20</Table.Head>
                                <Table.Head>Location</Table.Head>
                            </Table.Row>
                        </Table.Header>
                        <Table.Body class="w-full">
                            {#each t.table as entry}
                                <Table.Row>
                                    <Table.Cell class="text-wrap whitespace-normal">{entry.roll}</Table.Cell>
                                    <Table.Cell class="text-wrap whitespace-normal">{entry.location}</Table.Cell>
                                </Table.Row>
                            {/each}
                        </Table.Body>
                    </Table.Root>
                </Tabs.Content>
            {/each}
        </Tabs.Root>
    </Dialog.Content>
</Dialog.Root>
