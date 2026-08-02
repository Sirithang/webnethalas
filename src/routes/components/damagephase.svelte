<script lang="ts" module>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import Text from "$lib/components/ui/text/text.svelte";

    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Table from "$lib/components/ui/table/index.js";
    
    import { popupStates } from '../state.svelte.js';

</script>

<Dialog.Root bind:open={popupStates.isDamageStepPopupShown} >
    <Dialog.Content class="grid-cols-[100%]">
        <Dialog.Header>
            <Dialog.Title>Damage Phase</Dialog.Title>
        </Dialog.Header>
        
        <Text>1. Roll on <Button variant="link" onclick={() => {popupStates.isHitLocationPopupShown = true}}>Hit Location Tables</Button></Text>
        <Text>2. Roll all dices (weapon + abilities + bonuses) and <b>TAKE ONLY THE HIGHEST</b></Text>
        <Text>3. Reduce Toughness 1st, then Health. Armor on hit location reduce damage (if not ignored)</Text>
    </Dialog.Content>
</Dialog.Root>