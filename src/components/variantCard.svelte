<script>
    import { fade } from 'svelte/transition';
    import { selectedVariant } from '../stores/stores.js';
    export let variant;

    function GetVariantRarityAttribute(variant){
        return variant.attributes.find(atr => atr.trait_type == "Rarity Class").value
    }

    function SetSelectedVariant(){
        selectedVariant.set(variant);
        document.getElementsByClassName("modal-toggle")[0].click();
    }


</script>

<div on:click={SetSelectedVariant} transition:fade class="mx-4 {GetVariantRarityAttribute(variant).toLowerCase()}Shadow p-2 m-2">
    <p class="font-medium text-center text-lg">{variant.name.toUpperCase()}</p>
    <div style="position:relative;">
        <p class="top-left rounded">#{variant.id}</p>
        <p class="bottom-center rounded">Rank: {variant.rank}</p>
        <img loading="lazy" alt="{variant.id}" src="{`https://midnightsociety.com/_next/image?url=%2Ffounders_pass_art%2F${variant.id}.jpg%3Fthumb%26res%3D146&w=1920&q=25`}"/>
    </div>
</div>



<style>
    .top-left {
        position: absolute;
      top: 20px;
      left: 20px;
      background-color: black;
      color: white;
      padding-left: 20px;
      padding-right: 20px;
    }
    
    .bottom-center {
        position: absolute;
      bottom: 20px;
      margin-left: auto;
        margin-right: auto;
        left: 75px;
        right: 75px;
    text-align: center;
      background-color: black;
      color: white;
      padding-left: 20px;
      padding-right: 20px;
    }
    
    img {
        height: 250px;
        width: 250px;
        min-height: 225px;
        min-width: 225px;
        opacity: 1;
        transition: opacity 1200ms ease-out;
        background-color:gray;
        border: 1px solid black;
      }
    
    
    .relicShadow {
        max-height: 335px;
        box-shadow: 0px 0px 8px 1px #ED3535;
    }
    
    .legendaryShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px #f96300;
    }
    
    .epicShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px #b235ed;
    }
    
    .rareShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px #37bef8
    }
    .commonShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px #77ff47;
    }
    
      </style>