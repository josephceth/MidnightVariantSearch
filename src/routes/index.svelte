<script>
import {onMount} from 'svelte';
import { selectedVariant } from '../stores/stores.js';
import VariantCard from '../components/variantCard.svelte';
import RarityIcon from '../components/RarityIcon.svelte';
import IconBorder from '../components/IconBorder.svelte';
import MoonIcon from '../components/MoonIcon.svelte';

let variants = [];
let nameSearch = ""
let pageSize = 20;
let page = 1;
let sorter = -1;
let allVariants = [];

let modalVariant;
selectedVariant.subscribe(value => {
    modalVariant = value;
});


function GetVariants(page){
    return variants.slice((page-1)*pageSize, page*pageSize);
}

function GVA(variant, attribute){
        return variant.attributes.find(atr => atr.trait_type == attribute);
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
        console.log(allVariants);
    }
}

onMount(async () =>{
    let response =  await fetch(`./yayaya.json`, {
      headers : { 
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       }
    });
    let json = await response.json();
    variants = json;
    allVariants = GetVariants(page);
    console.table(allVariants);

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

<div id="gridVariant" class="col-span-4 mt-2 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-1 lg:grid-cols-2 xl:grid-cols-3" style="height:80vh; overflow-y:scroll">
     {#each allVariants as variant}
    <VariantCard variant={variant} />
    {/each}
</div>

<input type="checkbox" id="my-modal" class="modal-toggle" />
<div class="modal text-white">
  <div class="modal-box w-11/12 max-w-5xl">
    <a class="btn" href="https://opensea.io/assets/matic/0x89a4875c190565505b7891b700c2c6dc91816a47/{modalVariant.id}" target="_blank">View on OpenSea</a>

        <label for="my-modal" class="btn">Close</label>
    <div class="card compact side bg-base-100">
        <div class="flex-row items-center space-x-4 card-body">
            <div>
                <div class="avatar">
                    <div class="w-100 h-100">
                        <img loading="lazy" alt="{modalVariant.id}" src="{`https://midnightsociety.com/_next/image?url=%2Ffounders_pass_art%2F${modalVariant.id}.jpg%3Fthumb%26res%3D146&w=1920&q=100`}"/>
                    </div>
                </div>
            </div>
            <div>
                <h2 class="card-title uppercase {GVA(modalVariant, "Squad Role").rarity_class}">Squad: {GVA(modalVariant, "Squad Role").value}</h2> 
                <p class="text-white italic text-xl">{modalVariant.name}</p>
            </div>
        </div>
    </div>

    <div class="card compact side bg-base-100">
        <div class="flex-row items-center space-x-4 card-body">
            <div>
                <div class="avatar">
                    <div class="w-14 h-14">
                        {#key modalVariant}
                        <RarityIcon rarity={GVA(modalVariant, "Rarity Class").rarity_class}/>
                        {/key}
                    </div>
                </div>
            </div>
            <div>
                <h2 class="card-title uppercase {GVA(modalVariant, "Rarity Class").rarity_class}">Ranking</h2> 
                <p class="text-white text-lg">{modalVariant.rank}</p>
            </div>
        </div>
    </div>
    <div class=" grid grid-cols-1 md:grid-cols-2">
    {#each modalVariant.attributes as attribute (attribute)}
        <div class="card compact side bg-base-100">
            <div class="flex-row items-center space-x-0 card-body">
                <div>
                    <div class="avatar">
                        <div class="w-14 h-14">
                            <RarityIcon rarity={attribute.rarity_class}/>
                        </div>
                    </div>
                </div>
                <div>
                    <h2 class="card-title uppercase {attribute.rarity_class}">{attribute.trait_type}</h2> 
                    <p class="text-white text-lg">{attribute.value} <span class="opacity-50 text-sm">({parseFloat(attribute.rarity).toFixed(2)}% have this trait)</span></p>
                </div>
            </div>
        </div>
    {/each}
</div>
    
  </div>
</div>


<style>      
    .Relic {
        color: #ED3535;
    }
    
    .Legendary {
        color: #f96300;
    }
    
    .Epic {
        color: #b235ed;
    }
    
    .Rare {
        color: #37bef8
    }
    .Common {
        color: #77ff47;
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

.text-xl {
    font-size: 2.5rem;
}

</style>