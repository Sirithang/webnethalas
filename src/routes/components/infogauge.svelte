<script lang="ts" module>
    import { Button } from "$lib/components/ui/button/index.js";
    import * as ButtonGroup from "$lib/components/ui/button-group/index.js";
    import { Progress } from "$lib/components/ui/progress/index.js";
    import { Text } from "$lib/components/ui/text/index.js";
</script>

<script lang="ts">
    let {
        label,
        currentValue,
        maxValue,
        onchangefunc,
        barcolor,
     } = $props();
</script>


<div class="flex flex-row items-center w-full my-2 h-12">
    <ButtonGroup.Root class="shrink px-1 h-full">
        <Button
            variant="secondary"
            onclick={() => {onchangefunc(-10)}}
            class="h-full w-12">-10</Button
        >
        <Button
            variant="secondary"
            onclick={() => {onchangefunc(-1)}}
            class="h-full w-12">-1</Button
        >
    </ButtonGroup.Root>
    <div class="grow relative px-1 h-full">
        <Progress
            value={currentValue}
            max={maxValue}
            class="grow h-full **:data-[slot=progress-indicator]:{barcolor}"
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
            onclick={() => {onchangefunc(1)}}
            class="h-full  w-12">+1</Button
        >
        <Button
            variant="secondary"
            onclick={() => {onchangefunc(+10)}}
            class="h-full w-12">+10</Button
        >
    </ButtonGroup.Root>
</div>
