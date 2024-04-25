<script>
  import * as d3 from "d3"
  import {onMount} from "svelte"
  import {scaleLinear} from "d3-scale";
  import AxisX from "./components/AxisX.svelte";
  import Tooltip from "./components/Tooltip.svelte";
  

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
    .range(["#ed334e", "#45C4B0", "#0D08F9", "#242F36"])

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

  //----------Funciones-para-radio---------

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
  
  //-----------Hovered---------------------

  let hoveredData;
  $: console.log(hoveredData);

  onMount(() => {
    d3.csv("./data/respuestasvis2.csv", d3.autoType).then(data => {
      console.log(data)

      /* Actualizamos dominio con la data de distancia */
      let minMaxEdad = d3.extent(data, d => d.distancia)
      grosor = grosor.domain(minMaxEdad)

      /* Actualizamos dominio y rango con la data de tiempoN */
      let minMaxAltura = d3.extent(data, d => d.tiempoN)
      radioAltura = radioAltura.domain(minMaxAltura).range([25, 50])
      

      alumnos = data

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
        <h3>Referencias</h3>
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <circle class="circulo-glosario"
            r="10"
            fill="#0D08F9"
            cx="50%"
            cy="50%" />
          </svg>
          
          <p>Caminando</p>
        </div>
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <circle class="circulo-glosario"
            r="10"
            fill="#ed334e"
            cx="50%"
            cy="50%" />
          </svg>
          <p>Bondi</p>
        </div>
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <circle class="circulo-glosario"
            r="10"
            fill="#45C4B0"
            cx="50%"
            cy="50%" />
          </svg>
          <p>Transporte Propio</p>
        </div>
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <circle class="circulo-glosario"
            r="10"
            fill="#242F36"
            cx="50%"
            cy="50%" />
          </svg>
          <p>Subte</p>
        </div>
        
    
      </div>
      
    </div>
    
  
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="graf-container"
      on:mouseleave={()=>{
        hoveredData=null;
      }}>
    
      <svg class="graf" {width} {height}>
  
        <!-- <AxisX {height} {xScale} {width} {margin} {yScale}/> -->
       
        <circle class="utdt" cx="0" cy="0" r="30" fill="yellow" overflow="visible"/>

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

        

            
    <g>
       <!-- svelte-ignore a11y-mouse-events-have-key-events -->
       <circle class ="alumnos" cx={(transvertx((al.ejeY),al.distancia)*38)+13} 
       cy={(transverty(al.ejeY,al.distancia)*40)} 
       r={hoveredData && hoveredData == al ? (al.tiempoN/2):(al.tiempoN/4)}
       opacity={hoveredData ? hoveredData == al ? "1" : ".3" : "1"}
       fill = {colorTransporte(al.Venis)}
       stroke = "wheat" 
       stroke-width= "1px"
       on:mouseover={()=>{
         hoveredData = al;
       }}
       />

      <path class="mancha-overlay" d="M0 14.3036C0 18.0171 3.33276 20.843 6.99635 20.2358L14.8388 18.9361C16.5184 18.6577 18.2413 18.8128 19.8442 19.3866L30.2479 23.1109C33.5236 24.2835 37.174 23.6584 39.8728 21.4627L40.3061 21.1102C42.3291 19.4643 44.9805 18.8061 47.5384 19.3148L50.6981 19.9432C54.4771 20.6947 58 17.8035 58 13.9504V13.9504C58 10.2569 54.7483 7.40786 51.0868 7.89331L46.3844 8.51678C44.5005 8.76655 42.5845 8.47436 40.8607 7.67446L38.0757 6.3821C36.9055 5.83907 35.995 4.8587 35.5398 3.65161V3.65161C34.0782 -0.224184 28.7766 -0.675647 26.6807 2.8972L24.3239 6.91468C22.783 9.5414 19.7477 10.9041 16.7608 10.3101L7.18609 8.40589C3.46698 7.66625 0 10.5116 0 14.3036V14.3036Z" 
      fill="#EDBC72"
      fill-opacity={al.Diteliano == "k" ? hoveredData ? hoveredData == al ? "1" :".3" : "1" : "0"}
      transform="translate({ al.tiempoN == "30" ? (transvertx((al.ejeY),al.distancia)*38)+7 : (transvertx((al.ejeY),al.distancia)*38)+2}, 
        {al.tiempoN == "30" ? (transverty((al.ejeY),al.distancia)*40)-2: (transverty(al.ejeY,al.distancia)*40)-2}),
        scale({hoveredData && hoveredData == al ? (al.tiempoN/80):(al.tiempoN/160)})"
      />

      <path class="anillo-overlay" d="M6.48845 3.23147C-13.2925 -12.7078 26.3684 62.9578 31.4886 34.2315" stroke="wheat" 
       stroke-linecap="round"
       transform="translate({ al.tiempoN == "90" ? (transvertx((al.ejeY),al.distancia)*38)-12 : (transvertx((al.ejeY),al.distancia)*38)}, 
       {al.tiempoN == "90" ? (transverty((al.ejeY),al.distancia)*40)-18: (transverty(al.ejeY,al.distancia)*40)-11}),
       scale({hoveredData && hoveredData == al ? (al.tiempoN/40):(al.tiempoN/80)})"
       stroke-opacity={al.Vivis == "CABA" ? "0" : "1"}
       
       
       />

       
    </g>
           
        {/each}
    
        <!-- {#each alumnos as al} ----esta es la version que anda---
        <circle class ="alumnos" cx={xScale(al.distancia)-12} 
                cy={yScale(al.ejeY*3.4)} 
                r={al.tiempoN/4}
                fill = {colorTransporte(al.Venis)}
                stroke = "black" />
    
        {/each} -->
        
      </svg>
        {#if hoveredData}
        <Tooltip data={hoveredData} {transvertx} {transverty}/>

        {/if}

      
      
  
    </div>
    
    <div class="cuerpo">
      <h3>Tu camino a Ditella</h3>
      <p>En base a los datos recopilados de los alumnos de Visualizacion de Datos, discretizamos en el grafico la distancia a la que vive cada alumno, siendo el "sol" la Universidad. 
        El tamaño representa el tiempo aproximado en el que tardan en llegar y los colores varian dependiendo del medio de transporte que utilizan para el viaje.
      </p>
  
    </div>

  </div>

  
  


</main>

<style>
  :global(body) {
    background-color:#1B1B1E;
    background-image: url("/images/Desktop-2.png");
    
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
    flex-direction: column;
    justify-content: space-between;

  }
  .parte-b-l {
    
    display: flex;
    flex-direction: column;
  }

  /* glosario */

  .glosario {
    margin-top: 20px;
    margin-bottom: 80px;
    display: flex;
    flex-direction: row;
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#10100E;
    padding: 25px;
    justify-content: space-between;

    
  }
  .glosario-ref {
    display: flex;
    flex-direction: row;
  }
  .circulo-glos-svg {
    width: 40px;
    height: 40px;
  }
  .circulo-glosario {
    stroke: "wheat" ;
    stroke-width: "1px";

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
    flex-direction: column;
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
    align-items: flex-start;
    
    
  }
  .ref {
    color: wheat;
    
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
  .alumnos {
    transition:all 300ms ease;
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
    stroke-width: 1px;
  
  }
  .anillo-overlay {
    position: absolute;
    width: auto;
    height: auto;
    stroke-width: 1px;
    opacity: 1;
    z-index: 4;
    top: 30px;
    fill-opacity: 0;
    transition:all 300ms ease;
    
  }
  .mancha-overlay {
    position: absolute;
    width: auto;
    height: auto;
    transition:all 300ms ease;
    background-size: cover;
    background-position: center;
    opacity: 1;
    z-index: 3;
    top: 30px;
  }
  

</style>
