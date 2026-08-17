<script>
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import Text from "$lib/components/ui/text/text.svelte";

    import { popupStates } from "../state.svelte";

</script>
<Dialog.Root bind:open={popupStates.isAboutPopupShown}>
    <Dialog.Content class="grid-cols-[100%] h-[70%] content-start">
        <Dialog.Header>
            <Dialog.Title>Web Nethalas Tools</Dialog.Title>
        </Dialog.Header>
        
    </Dialog.Content>
</Dialog.Root>