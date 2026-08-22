<script>
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import ScrollArea from "$lib/components/ui/scroll-area/scroll-area.svelte";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import { popupStates } from "../state.svelte";

</script>
<Dialog.Root bind:open={popupStates.isAboutPopupShown}>
    <Dialog.Content class="h-[70%] flex">
        <Tabs.Root value="help" class="w-full mt-2">
            <Tabs.List class="w-full">
                <Tabs.Trigger value="help">Help</Tabs.Trigger>
                <Tabs.Trigger value="about">About</Tabs.Trigger>
            </Tabs.List>
            <Tabs.Content value="help" class="w-full h-full">
                <ul class="my-6 ms-6 list-disc [&>li]:mt-2">
                    <li>Click on a bar to set the max value</li>
                </ul>
            </Tabs.Content>
            <Tabs.Content value="about" class="w-full h-[calc(100%-var(--spacing)*6)]">
                <ScrollArea class="h-full">
                    <div class="text-justify m-1 h-full">
                        <p>This is a simple companion tool to the <Button variant="link" href="https://kernethalas.com/">Ker Nethalas</Button> Solo Dungeon Crawler Game.</p>
                        <br/>
                        <p>It is designed to be run on a phone in portrait mode sitting next to you as you play.</p>
                        <br/>
                        <p>This is not intended to be a digital character sheet, as I enjoy having to physically draw map, roll dices and lookup things on tables but instead try to speed up play by removing some friction I encounter while playing.</p>
                        <br/>
                        <p><b>You cannot play with just that tools and will need the Game Book!</b>. The intend is not to replace the book, just make playing with it a bit more streamlined.</p>
                    
                        <br/>
                        <h1 class="text-lg">Features</h1>
                        <ul class="my-6 ms-6 list-disc [&>li]:mt-2">
                            <li>Handle changing value (like Toughness, Aether etc.) to avoid constantly erasing the character sheet. Just track them on the app then commit their value to paper only when stopping playing to "save"</li>
                            <li>Contain a combat tool to track enemies health pools + combat flow references like fumbles/defensives tables to avoid having to constantly flip pages away from the enemy stat block</li>
                            <li>Contains an exploration references, especially how to detect trap/lock and handle them</li>
                            <li>A couple of common references like conditions</li>
                        </ul>
                    </div>
                </ScrollArea>
            </Tabs.Content>
        </Tabs.Root>
    </Dialog.Content>
</Dialog.Root>