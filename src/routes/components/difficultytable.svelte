<script lang="ts">
    import * as Table from "$lib/components/ui/table/index.js";
    import * as Dialog from "$lib/components/ui/dialog/index.js";    
    import { popupStates } from "../state.svelte";
</script>

<Dialog.Root bind:open={popupStates.isDifficultyPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Difficulty Modifier</Dialog.Title>
        </Dialog.Header>
        <Table.Root class="w-full">
            <Table.Header class="w-full">
                <Table.Row>
                    <Table.Head class="w-13">D8</Table.Head>
                    <Table.Head>Difficulty</Table.Head>
                    <Table.Head>Modifier</Table.Head>
                </Table.Row>
            </Table.Header>
            <Table.Body class="w-full">
                <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">1</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Child's Play</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">+30</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">2</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">effortless</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">+20</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">3</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Easy</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">+10</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">4-5</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Normal</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">+0</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">6</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Demanding</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">-10</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">7</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Hard</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">-20</Table.Cell>
                </Table.Row>
                 <Table.Row>
                    <Table.Cell class="text-wrap whitespace-normal">8</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">Impossible</Table.Cell>
                    <Table.Cell class="text-wrap whitespace-normal">-30</Table.Cell>
                </Table.Row>
            </Table.Body>
        </Table.Root>
    </Dialog.Content>
</Dialog.Root>