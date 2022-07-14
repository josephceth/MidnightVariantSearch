<script>
    export let rarity;
    let fillColor;

    switch(rarity) {
        case "Common":
            fillColor = "#77ff47";
            break;
        case "Rare":
            fillColor = "#37bef8";
            break;
        case "Epic":
            fillColor = "#b235ed";
            break;
        case "Legendary":
            fillColor = "#f96300";
            break;
        case "Relic":
            fillColor = "#ED3535";
            break;
        default:
}
</script>

<svg width="47" height="54" viewBox="0 0 47 54" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M23.0102 0L3.42727e-07 13.3964L0 40.1891L15.2354 49.059L46.0204 31.1495V13.3964L23.0102 0ZM46.0204 35.0988L18.6284 51.0344L23.0102 53.5854L46.0204 40.1891V35.0988Z" fill="{fillColor}"></path></svg>