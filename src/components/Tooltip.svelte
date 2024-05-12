<script>
    export let data;
    export let transvertx;
    export let transverty;
</script>

<div class = "tooltip" style="position: absolute;
    top: {(transverty(data.ejeY,data.distancia)*55)+500}px;
    left: {(transvertx((data.ejeY),data.distancia)*55)+13+400}px;">

    <h3>{data.nombre}</h3>
    <p>{data.Tiempo}</p>
</div>

<!-- <div class = "tooltip" style="position: absolute;
    top: {450+650}px;
    left: {(600)+650}px;">

    <h3>{data.nombre}</h3>
    <p>{data.Tiempo}</p>
</div> -->


<!-- Para coso fijo -->

<!-- <div class = "tooltip" style="position: fixed;
    bottom: 0;
    right: 0;">

    <h3>{data.nombre}</h3>
    <p>{data.Tiempo}</p>
</div> -->

<style>
    .tooltip {
        padding: 6px;
        
        background-color: rgba(0, 0, 0, .5);
        
        pointer-events: none;
        transition: top 300ms ease, left 300ms ease;
        border-radius: 10px;
        color: wheat;


    }
    .h3 {
        font-weight: 700;
        margin-bottom: 0.25rem;
    }
</style>