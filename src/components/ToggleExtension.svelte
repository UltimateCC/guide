<script lang="ts">
    export let title: string;
    export let expanded: boolean;

    let content: HTMLDivElement;

    function updateHeight(newExpanded: boolean) {
        if (content) {
            const contentHeight = `${content.scrollHeight}px`;

            if (!newExpanded) content.style.height = contentHeight;

            // Before uncollapsing the content, set its height to the actual height to have a smooth transition
            if (!newExpanded) content.style.height = contentHeight;

            // Wait until the next frame to apply the height change
            setTimeout(() => {
                if (newExpanded) {
                    content.style.height = contentHeight;
    
                    setTimeout(() => {
                        content.style.height = 'auto'; // Set height to auto to allow for dynamic content
                    }, 500);
                } else {
                    content.style.height = '0';
                }
            }, 0);
        }
    }

    $: updateHeight(expanded);
</script>

<div class="toggle-captions-visible">
    <button class="text-btn" on:click={() => expanded = !expanded}>
        <h4>{title}</h4>
    </button>
    <div id="toggle-captions-visible-content" bind:this={content}>
        <slot />
    </div>    
</div>