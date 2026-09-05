<script lang="ts" module>
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import * as Table from "$lib/components/ui/table/index.js";

    import { popupStates } from "../state.svelte.js";

    import * as Traps from "$lib/data/traps.json";
</script>

<Dialog.Root bind:open={popupStates.isTrapsPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Traps</Dialog.Title>
        </Dialog.Header>
        <Table.Root class="w-full">
            <Table.Header class="w-full">
                <Table.Row>
                    <Table.Head class="w-8">D10</Table.Head>
                    <Table.Head class="w-13">Skill</Table.Head>
                    <Table.Head>Effect</Table.Head>
                </Table.Row>
            </Table.Header>
            <Table.Body class="w-full">
                {#each Traps.traps as t}
                    <Table.Row>
                        <Table.Cell class="text-wrap whitespace-normal"
                            >{t.roll}</Table.Cell
                        >
                        <Table.Cell class="text-wrap whitespace-normal"
                            >{t.skill}</Table.Cell
                        >
                        <Table.Cell class="text-wrap whitespace-normal"
                            >{@html t.effect}</Table.Cell
                        >
                    </Table.Row>
                {/each}
            </Table.Body>
        </Table.Root>
    </Dialog.Content>
</Dialog.Root>
