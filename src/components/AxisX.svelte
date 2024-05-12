<script>
    export let height;
    export let width;
    export let xScale;
    export let margin;
    export let yScale;

    let xTicks = [1, 5, 10, 20];
    let orbit = [1, 3, 7, 12];
</script>

<g>
    {#each xTicks as tick}
        <text 
        x= {width -width +  margin.left}
        y={yScale(height)}
        dx="2"
        dy="10"
        style="font-size: small; "
        >{tick}km</text>
        
    
    {/each}

</g>