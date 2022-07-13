<script>
    import { fade } from 'svelte/transition';
    export let variant;
</script>

<div transition:fade class="mx-4 {GetVariantRarityAttribute(variant).toLowerCase()}Shadow p-2 m-2">
    <p class="font-medium text-center text-lg">{variant.name.toUpperCase()}</p>
    <div style="position:relative;">
        <p class="top-left rounded">#{variant.id}</p>
        <p class="bottom-center rounded">Rank: {variant.rank}</p>
        <img loading="lazy" alt="{variant.id}" src="{`https://midnightsociety.com/_next/image?url=%2Ffounders_pass_art%2F${variant.id}.jpg%3Fthumb%26res%3D146&w=1920&q=25`}"/>
    </div>
</div>