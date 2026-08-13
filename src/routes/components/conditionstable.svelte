<script lang="ts" module>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Table from "$lib/components/ui/table/index.js";

    import { popupStates } from "../state.svelte.js";
    
    import * as ConiditionTables from "$lib/data/conditions.json"
</script>

<Dialog.Root bind:open={popupStates.isConditionPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Conditions</Dialog.Title>
        </Dialog.Header>
        <Table.Root class="w-full">
            <Table.Header class="w-full">
                <Table.Row>
                    <Table.Head class="w-13">Name</Table.Head>
                    <Table.Head>Effects</Table.Head>
                    <Table.Head>Save</Table.Head>
                </Table.Row>
            </Table.Header>
            <Table.Body class="w-full">
                {#each ConiditionTables.conditions as c}
                    <Table.Row>
                        <Table.Cell class="text-wrap whitespace-normal">{c.name}</Table.Cell>
                        <Table.Cell class="text-wrap whitespace-normal">{@html c.effect}</Table.Cell>
                        <Table.Cell class="text-wrap whitespace-normal">{@html c.save}</Table.Cell>
                    </Table.Row>
                {/each}
            </Table.Body>
        </Table.Root>
    </Dialog.Content>
</Dialog.Root>