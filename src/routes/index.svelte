<script>
import {onMount} from 'svelte';
import { fade } from 'svelte/transition';

let variants = [];
let nameSearch = ""
let pageSize = 20;
let page = 1;
let sorter = -1;
let allVariants = [];


function GetVariants(page){
    return variants.slice((page-1)*pageSize, page*pageSize);
}

function GetVariantRarityAttribute(variant){
    return variant.attributes.find(atr => atr.trait_type == "Rarity Class").value
}

function SortByRank(){
    page = 1;
    sorter = -sorter;
    variants.sort((a,b) => a.rank < b.rank ? sorter : b.rank < a.rank ? -sorter : 0);
    allVariants = [...GetVariants(page)];
}

function SortByID(){
    page = 1;
    sorter = -sorter;
    variants.sort((a,b) => a.id < b.id ? sorter : b.id < a.id ? -sorter : 0);
    allVariants = [...GetVariants(page)];
}

function FilterByName(){
    if(nameSearch == ""){
        page = 1;
        allVariants = [...GetVariants(page)];
    }else{
        allVariants = variants.filter(variant => 
            variant.name.toLowerCase().includes(nameSearch.toLowerCase())
            ||
            variant.id.toString().includes(nameSearch)
        ).splice(0,50);
    }
}


onMount(async () =>{
    let response = await fetch("http://localhost:3000/src/json/yayaya.json.gz");
    let json = await response.json();
    variants = json;
    allVariants = GetVariants(page);

    let element = document.getElementById("gridVariant");
    element.addEventListener('scroll', () => {
        const {
            scrollTop,
            scrollHeight,
            clientHeight
        } = element;

        if (scrollTop + clientHeight >= scrollHeight - 200)  {
            page++;
            allVariants = [...allVariants, ...GetVariants(page)];
        }
    }, 
    {
        passive: true
    });
});
</script>


<div class="flex space-x-4">
    <input class="border" type="text" id="search" placeholder="Search" on:keyup="{FilterByName}" bind:value={nameSearch} />
    <button class="btn" on:click={SortByRank}>Sort By Rank</button>
    <button class="btn" on:click={SortByID}>Sort By ID</button>
</div>
<div id="gridVariant" class="col-span-4 mt-2 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3" style="height:80vh; overflow-y:scroll">
    {#each allVariants as variant}
    <div transition:fade class="mx-4 {GetVariantRarityAttribute(variant).toLowerCase()}Shadow p-2 m-2">
        <p class="font-medium text-center text-lg">{variant.name.toUpperCase()}</p>
        <div style="position:relative;">
            <p class="top-left rounded">#{variant.id}</p>
            <p class="bottom-center rounded">Rank: {variant.rank}</p>
            <img loading="lazy" alt="{variant.id}" src="{`https://midnightsociety.com/_next/image?url=%2Ffounders_pass_art%2F${variant.id}.jpg%3Fthumb%26res%3D146&w=1920&q=25`}"/>
        </div>
    </div>
    {/each}
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
        height: 225px;
        width: 225px;
        min-height: 292px;
        min-width: 292px;
        opacity: 1;
        transition: opacity 1200ms ease-out;
        background-color:gray;
      }
      
      ::-webkit-scrollbar{
        height: 12px;
        width: 12px;
    }
    ::-webkit-scrollbar-track{
        background: rgba(255, 255, 255, 0.1);
        border-radius: 1ex;
    }
    ::-webkit-scrollbar-thumb{
        background: var(--theme-color);
        border-radius: 1ex;
    }
    ::-webkit-scrollbar-corner{
        background: none;
    }
    
    .relicShadow {
        max-height: 335px;
        box-shadow: 0px 0px 8px 1px #ED3535;
    }
    
    .legendaryShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px rgb(249, 99, 0);
    }
    
    .epicShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px rgb(178, 53, 237);
    }
    
    .rareShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px rgb(55, 190, 248)
    }
    .commonShadow {
        max-height: 335px;
        box-shadow: 0px 0px 5px 1px rgb(119, 255, 71);
    }
    
      </style>