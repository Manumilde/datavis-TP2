<script>
  import * as d3 from "d3"
  import {onMount} from "svelte"
  import {scaleLinear} from "d3-scale";
  import AxisX from "./components/AxisX.svelte";
  

  // import bsasOverlay from './assets/circuTest.svg';

  /* Array donde guardaremos la data */
  let alumnos = []

  let width = 510;
  let height = 510;

  const margin = {top: 20, right: 20, left: 0, bottom: 20};

  /* 1. Escala para edades */
  let grosor = d3.scaleLinear().range([5, 20])

  /* 2. Escala para ubicacion (Vivis) */
  let colorUbicacion = d3
    .scaleOrdinal()
    .domain(["CABA", "Provincia de Bs As"])
    .range(["#FFDE68", "#10100E"])

  /* 3. Escala para transporte */
  let colorTransporte = d3
    .scaleOrdinal()
    .domain(["Bondi", "Transporte Propio", "Caminando", "Subte"])
    .range(["#ed334e", "#45C4B0", "#fbb132", "#242F36"])

  /* 4. Escala para altura */
  let radioAltura = d3.scaleRadial()

  //Escala para posiciones
   $: xScale = scaleLinear()
    .domain([0,20])
    .range([0,width])
  
  import {max} from "d3-array";
  $: yScale = scaleLinear()
    .domain([0 , 50])
    .range([width,0]);

  /* 5. Escala para diteliano */
  let colorDiteliano = d3.scaleOrdinal()
    .domain(["k", "m"])
    .range(["gold", "white"])

  function transvertx(n,radio){

    const angle = ((Math.PI/(n)));
    const x = Math.cos(angle)*radio;
    // const y = radio * Math.sin(angle);

    let res = x ;

    return res;
    
  }
  function transverty(n,radio){

    let angle = (Math.PI/(n));
    // const x = radio * Math.cos(angle);
    const y = Math.sin(angle)*radio ;

    let res = y;

    return res;

  }
  

  onMount(() => {
    d3.csv("./data/respuestasvis2.csv", d3.autoType).then(data => {
      console.log(data)

      /* Actualizamos dominio con la data de distancia */
      let minMaxEdad = d3.extent(data, d => d.distancia)
      grosor = grosor.domain(minMaxEdad)

      /* Actualizamos dominio y rango con la data de tiempoN */
      let minMaxAltura = d3.extent(data, d => d.tiempoN)
      radioAltura = radioAltura.domain(minMaxAltura).range([25, 50])
      
      //------Angulos------------------//
      // let angles= (360/d.cantidadelementos); 
      // let ejeXcirc= cos(angles*d.distancia);
      // let ejeYcirc= sin(angles*d.distancia);

      alumnos = data
      
    // d3.selectAll(".person-container")
    //   .data(alumnos)
    //   .enter()
    //   .append("div") // Wrap the SVG in a separate div for positioning
    //     .classed("bsas-overlay-container", d => d.Vivis === "Provincia de Bs As")
    //     .append(() => {
    //       const svgContent = new DOMParser().parseFromString(bsasOverlay, "image/svg+xml");
    //       console.log(svgContent);
    //       return svgContent.documentElement;
    //     });
    
    // d3.selectAll(".person-container")
    //   .data(alumnos)
    //   .enter()
    //   .append("div") // Wrap the SVG in a separate div for positioning
    //     .classed("bsas-overlay-container", d => d.Vivis === "Provincia de Bs As")
    //     .append(() => {
    //       return document.querySelector("#bsas-overlay").cloneNode(true);
    //     });

      
    })
  })
</script>

<main>
  <div class="header">
    <div class="logo">
      <img src="/images/UTDTlog.svg" width="100" alt="logo" />
    </div>
    <h3 class="headline">
      <b>Tu Camino a Ditella</b>
      <p class="bajada">Comparando como llega cada persona a la Universidad</p>
    </h3>
    
  </div>

  <div class="parte-b">
    <div class="parte-b-l">
      <div class="glosario">

        <p>Amarillo: Caminando</p>
        <p>Rojo: Bondi</p>
        <p>Verde: Transporte Propio</p>
        <p>Azul Oscuro: Subte</p>
    
      </div>
      <div class="cuerpo">

        <p>Aclaracion</p>
    
      </div>
    </div>
    
  
    <div class="graf-container">
      <svg class="graf" {width} {height}>
  
        <AxisX {height} {xScale} {width} {margin} {yScale}/>
       
        <circle class="utdt" cx="0" cy="0" r="30" fill="yellow"/>

        <circle class="radial" cx="0" cy="0" r="60"/>
        <circle class="radial" cx="0" cy="0" r="200"/>
        <circle class="radial" cx="0" cy="0" r="400"/>
        
        <circle class="orbit" cx="0" cy="0" r="50"/>
        <circle class="orbit" cx="0" cy="0" r="86"/>
        <circle class="orbit" cx="0" cy="0" r="125"/>
        <circle class="orbit" cx="0" cy="0" r="165"/>
        <circle class="orbit" cx="0" cy="0" r="242"/>
        <circle class="orbit" cx="0" cy="0" r="280"/>
        <circle class="orbit" cx="0" cy="0" r="320"/>
        <circle class="orbit" cx="0" cy="0" r="480"/>

        {#each alumnos as al}
    
        <!-- <circle class="orbit" cx="0" cy="0" r={al.distancia*50}/> -->
    
        <circle class ="alumnos" cx={(transvertx((al.ejeY),al.distancia)*38)+13} 
                cy={(transverty(al.ejeY,al.distancia)*40)} 
                r={al.tiempoN/4}
                fill = {colorTransporte(al.Venis)}
                stroke = "wheat" />
    
                <!-- <div>
    
                  {#each transvert(al.cantidadelementos,al.distancia) as {x,y}}
                    <div class="small-circle" style="transform: translate({x}px, {y}px);">
                      <img src="./assets/circuTest.svg" alt=({x},{y}) class="minus">
                    </div>
                  {/each}
                </div> -->
    
        {/each}
    
        <!-- {#each alumnos as al} ----esta es la version que anda---
        <circle class ="alumnos" cx={xScale(al.distancia)-12} 
                cy={yScale(al.ejeY*3.4)} 
                r={al.tiempoN/4}
                fill = {colorTransporte(al.Venis)}
                stroke = "black" />
    
        {/each} -->
      </svg>
    
  
    </div>
    

  </div>

  
  


</main>

<style>
  :global(body) {
    background-color:#1B1B1E;
    
  } 
  .header {
    display: flex;
    justify-content: center;
  
    flex-direction: row;
    width: auto;
    height: 200px;
    margin-top: 50px;
    margin-bottom: 80px;
    background-color: #10100E;
    border-radius: 10px;
    justify-content: space-around;
    
  }
  .headline {
    font-size: 48px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 5%;
    color: wheat;
    
  } 
  .logo {
    display: flex;
    align-content: center;
    align-self: left;
    justify-content: center;
    margin-top: 5;
  }
  .bajada {
    font-size: 24px;
    color: wheat;
    font-weight: normal;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
    color: wheat;
  }
  .parte-b {
    display: flex;
    flex-direction: row;
    justify-content: space-between;

  }
  .parte-b-l {
    
    display: flex;
    flex-direction: column;
  }
  .glosario {
    margin-top: 20px;
    margin-bottom: 80px;
    display: flex;
    flex-direction: column;
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#10100E;
    padding: 25px;
    
  }
  .cuerpo {
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#10100E;
    margin-top: 50px;
    margin-bottom: 80px;
    padding: 25px;

  }
  .graf-container {
    display: flex;
    margin-left: 10%;
    flex: 2;
    background-color: #10100E;
    justify-content: right;
    align-items: right;
    align-content: right;
    border-radius: 10px;
    margin-top: 20px;
    margin-bottom: 80px;
    padding: 10%;
    
    
  }
  .graf {
    display:flex;

    margin: auto;
    border-radius: 10px;
    background-color:#10100E;
    scale: 165%;
    margin-top: 50px;
    margin-bottom: 80px;
    padding: 10px;
  }
  
  .radial {
    
    border-radius:50%;
    fill-opacity: 0%;
    stroke: wheat;
    stroke-dasharray: 3 4;
    background:rgba(0,0,0,0);
    color:white;
  }
  .orbit {
    
   
    fill-opacity: 0%;
    stroke-opacity: 20%;
    stroke: wheat;
    stroke-width: 2px;
  
  }
  

</style>
