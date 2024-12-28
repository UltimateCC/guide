<script lang="ts">
    export let title: string;
    export let htmlId: string;
    export let expanded: boolean;
</script>

<div id={htmlId} class="tuto-item" class:inactive={!expanded}>
    <div class="tuto-header">
        <h3>
            <button on:click={() => expanded = !expanded} aria-expanded="{expanded}">
                <span>{title}</span>
                <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 55.63 47.79">
                    <path d="M50.65.82H5.06C1.89.82-.07,4.28,1.56,7l22.79,37.99c1.58,2.64,5.41,2.64,7,0L54.15,7c1.63-2.72-.33-6.18-3.5-6.18Z"/>
                </svg>
            </button>
        </h3>
    </div>
    <h3 class="responsive-tuto-header">{title}</h3>
    <div class="tuto-content-container">
        <div class="tuto-content">
            <slot />
        </div>
    </div>
</div>