<script lang="ts" module>
    import { Button, buttonVariants } from "$lib/components/ui/button/index.js";
    import * as ButtonGroup from "$lib/components/ui/button-group/index.js";
    import { Progress } from "$lib/components/ui/progress/index.js";
    import { Text } from "$lib/components/ui/text/index.js";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import Input from "$lib/components/ui/input/input.svelte";
</script>

<script lang="ts">
    let {
        label,
        currentValue,
        maxValue,
        onchangefunc,
        onmaxchangedfunc,
        classAddition = "",
        height = 12,
        maxEditable = true,
    } = $props();

    let editDialogOpen = $state(false);
</script>

<div class={["flex flex-row items-center w-full my-2", "h-" + height]}>
    <ButtonGroup.Root class="shrink px-1 h-full">
        <Button
            variant="secondary"
            onclick={() => {
                onchangefunc(-10);
            }}
            class="h-full w-12">-10</Button
        >
        <Button
            variant="secondary"
            onclick={() => {
                onchangefunc(-1);
            }}
            class="h-full w-12">-1</Button
        >
    </ButtonGroup.Root>
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <div
        class="grow relative px-1 h-full"
        role="button"
        tabindex="0"
        onclick={() => {
            if (maxEditable) editDialogOpen = true;
        }}
    >
        <Progress
            value={currentValue}
            max={maxValue}
            class={["grow h-full", classAddition]}
        />
        <Text
            class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10"
        >
            {label}:{currentValue}/{maxValue}
        </Text>
    </div>
    <ButtonGroup.Root class="shrink px-1 h-full">
        <Button
            variant="secondary"
            onclick={() => {
                onchangefunc(1);
            }}
            class="h-full  w-12">+1</Button
        >
        <Button
            variant="secondary"
            onclick={() => {
                onchangefunc(+10);
            }}
            class="h-full w-12">+10</Button
        >
    </ButtonGroup.Root>
</div>

<Dialog.Root bind:open={editDialogOpen}>
    <Dialog.Content class="sm:max-w-[425px]">
        <Dialog.Description>
            <Text>Edit max {label}</Text>
            <Input
                bind:value={maxValue}
                type="number"
                onchange={() => {
                    onmaxchangedfunc(maxValue);
                    editDialogOpen = false;
                }}
            />
        </Dialog.Description>
        <Dialog.Footer class="flex flex-row">
            <Dialog.Close
                type="button"
                class={buttonVariants({ variant: "outline" })}
            >
                Cancel
            </Dialog.Close>
        </Dialog.Footer>
    </Dialog.Content>
</Dialog.Root>
