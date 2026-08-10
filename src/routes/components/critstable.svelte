<script lang="ts" module>
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Table from "$lib/components/ui/table/index.js";

    import { popupStates } from "../state.svelte.js";
    
    import * as CritSuccessFailureData from "$lib/data/crit_success_failure.json"
</script>

<Dialog.Root bind:open={popupStates.isCritsPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Crits Effects</Dialog.Title>
        </Dialog.Header>

        <Tabs.Root value={CritSuccessFailureData.entries[0].skill} orientation="vertical">
            <Tabs.List class="w-0.2">
                {#each CritSuccessFailureData.entries as t}
                    <Tabs.Trigger value={t.skill}>{t.skill}</Tabs.Trigger>
                {/each}
            </Tabs.List>
            {#each CritSuccessFailureData.entries as t}
                <Tabs.Content value={t.skill} class="w-full">
                    <Table.Root class="w-full">
                        <Table.Header class="w-full">
                            <Table.Row>
                                <Table.Head>Success</Table.Head>
                                <Table.Head>Failure</Table.Head>
                            </Table.Row>
                        </Table.Header>
                        <Table.Body class="w-full">
                            <Table.Row>
                                <Table.Cell class="text-wrap whitespace-normal">{t.success}</Table.Cell>
                                <Table.Cell class="text-wrap whitespace-normal">{t.failure}</Table.Cell>
                            </Table.Row>
                        </Table.Body>
                    </Table.Root>
                </Tabs.Content>
            {/each}
        </Tabs.Root>
    </Dialog.Content>
</Dialog.Root>
