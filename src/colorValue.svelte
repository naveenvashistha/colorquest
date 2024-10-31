<script>
    import {callApi, finalColor} from './stores'; 
    $: if($finalColor && $callApi)
       {
            populateData();
            $callApi = false;
       }
    // $: if(red) handleChange(red);
    // $: if(green) handleChange(green);
    // $: if(blue) handleChange(blue);

    async function populateData()
    {
        let URL = `https://www.thecolorapi.com/id?rgb=${$finalColor[0]},${$finalColor[1]},${$finalColor[2]}`;
        let res = await fetch(URL);
        res = await res.json();
        console.log(res);
    }

    function handleChange(color)
    {
        if(color < 0) color = 0;
        else if(color > 255) color = 255;
    }
</script>

<div>
    <div>
        <span>RGB</span>
        <!-- <input type="text" bind:value={red} placeholder="0-255">
        <input type="text" bind:value={green} placeholder="0-255">
        <input type="text" bind:value={blue} placeholder="0-255"> -->
    </div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</div>
