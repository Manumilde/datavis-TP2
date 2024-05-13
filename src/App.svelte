<script>
  import * as d3 from "d3"
  import {onMount} from "svelte"
  import {scaleLinear} from "d3-scale";
  import AxisX from "./components/AxisX.svelte";
  import Tooltip from "./components/Tooltip.svelte";
  

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
  <div class="header1">
    <div class="header">
      <div class="logo">
        <img src="/images/UTDTlog.svg" width="100" alt="logo" />
      </div>
      <h3 class="headline" >
        <b style="color:beige">Tu Camino a Ditella</b>
        <p class="bajada">Comparando como llega cada persona a la Universidad</p>
      </h3>
      
    </div>
  </div>
  

  <div class="parte-b">
    <div class="parte-b-l">
      <div class="glosario">
        <h3 class="reftitle" style="color:beige">Referencias</h3>
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
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <path class="mancha-overlay" d="M0 14.3036C0 18.0171 3.33276 20.843 6.99635 20.2358L14.8388 18.9361C16.5184 18.6577 18.2413 18.8128 19.8442 19.3866L30.2479 23.1109C33.5236 24.2835 37.174 23.6584 39.8728 21.4627L40.3061 21.1102C42.3291 19.4643 44.9805 18.8061 47.5384 19.3148L50.6981 19.9432C54.4771 20.6947 58 17.8035 58 13.9504V13.9504C58 10.2569 54.7483 7.40786 51.0868 7.89331L46.3844 8.51678C44.5005 8.76655 42.5845 8.47436 40.8607 7.67446L38.0757 6.3821C36.9055 5.83907 35.995 4.8587 35.5398 3.65161V3.65161C34.0782 -0.224184 28.7766 -0.675647 26.6807 2.8972L24.3239 6.91468C22.783 9.5414 19.7477 10.9041 16.7608 10.3101L7.18609 8.40589C3.46698 7.66625 0 10.5116 0 14.3036V14.3036Z" 
              fill="#EDBC72"
              transform="translate(5,13),scale(.5)"/>
          </svg>
          <p>Diteliano</p>
        </div>
        <div class="glosario-ref">
          <svg class="circulo-glos-svg">
            <path class="anillo-overlay" d="M6.48845 3.23147C-13.2925 -12.7078 26.3684 62.9578 31.4886 34.2315" stroke="wheat" 
              stroke-linecap="round"
              transform="scale(.9)"></path>
          </svg>
          <p>Vive en Provincia de BA</p>
        </div>
        
    
      </div>
      
    </div>
    
  
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="graf-container"
      on:mouseleave={()=>{
        hoveredData=null;
      }}>
    
      <svg class="graf" >
  
        <!-- <AxisX {height} {xScale} {width} {margin} {yScale}/> -->
      
        <g id="sol">
          <circle class="utdt" cx="0" cy="0" r="30" fill="yellow" 
          fill-opacity="1"/>
          <path d="M10.3352 1L6.38247 9.09016L1 20.1066M10.3352 1L1 20.1066M10.3352 1H17.9091M1 20.1066V22H7.86932M17.9091 1L7.86932 22M17.9091 1H24.9545M7.86932 22H15.267M24.9545 1H32V2.89344M24.9545 1L15.267 22M15.267 22H22.6648M32 2.89344L26.7857 13.5656L22.6648 22M32 2.89344L22.6648 22" 
          stroke="#F79400"
          fill-opacity="0"
          stroke-opacity=".4"
          transform=translate(-5,0)/>
        </g>
      
       
        

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

        <!-- texto-graf -->

      <text class="texto-graf" x="80%" y="1.7%" fill="beige" font-size="11">Más de 10km</text>
      <text class="texto-graf" x="41%" y="1.7%" fill="beige" font-size="11">5km a 10km </text>
      <text class="texto-graf" x="13%" y="1.7%" fill="beige" font-size="11">1km a 5km </text>


        {#each alumnos as al}

        

            
    <g class="planet"
        >
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
      

      <!-- ----manchas--- -->

      <path class="mancha-overlay" d="M0 14.3036C0 18.0171 3.33276 20.843 6.99635 20.2358L14.8388 18.9361C16.5184 18.6577 18.2413 18.8128 19.8442 19.3866L30.2479 23.1109C33.5236 24.2835 37.174 23.6584 39.8728 21.4627L40.3061 21.1102C42.3291 19.4643 44.9805 18.8061 47.5384 19.3148L50.6981 19.9432C54.4771 20.6947 58 17.8035 58 13.9504V13.9504C58 10.2569 54.7483 7.40786 51.0868 7.89331L46.3844 8.51678C44.5005 8.76655 42.5845 8.47436 40.8607 7.67446L38.0757 6.3821C36.9055 5.83907 35.995 4.8587 35.5398 3.65161V3.65161C34.0782 -0.224184 28.7766 -0.675647 26.6807 2.8972L24.3239 6.91468C22.783 9.5414 19.7477 10.9041 16.7608 10.3101L7.18609 8.40589C3.46698 7.66625 0 10.5116 0 14.3036V14.3036Z" 
      fill="#EDBC72"
      fill-opacity={al.Diteliano == "k" ? hoveredData ? hoveredData && hoveredData == al ? "1" :".3" : "1" : "0"}
      transform="translate({ al.tiempoN == "30" ? 
          (hoveredData == al ? 
          (transvertx((al.ejeY),al.distancia)*38)  
          :(transvertx((al.ejeY),al.distancia)*38)+7) 
          : (hoveredData == al ? 
          (transvertx((al.ejeY),al.distancia)*38)-10  
          :(transvertx((al.ejeY),al.distancia)*38)+2)},

        {hoveredData && hoveredData == al ? 
          (transverty((al.ejeY),al.distancia)*40)-8
          :(transverty(al.ejeY,al.distancia)*40)-5}),
        scale({hoveredData && hoveredData == al ? (al.tiempoN/80):(al.tiempoN/160)})"
      />

      <!-- ----anillos--- -->

      <path class="anillo-overlay" d="M6.48845 3.23147C-13.2925 -12.7078 26.3684 62.9578 31.4886 34.2315" stroke="wheat" 
       stroke-linecap="round"
       
       transform="translate({ al.tiempoN == "90" ? 
        (hoveredData == al ?
          (transvertx((al.ejeY),al.distancia)*38)-32 
          :(transvertx((al.ejeY),al.distancia)*38)-12) 
        :(hoveredData == al ?
          (transvertx((al.ejeY),al.distancia)*38)-12 
          :(transvertx((al.ejeY),al.distancia)*38))},

       {al.tiempoN == "90" ?
        (hoveredData == al ? 
          (transverty((al.ejeY),al.distancia)*40)-40
          :(transverty((al.ejeY),al.distancia)*40)-18): 
        (hoveredData == al ?
          (transverty(al.ejeY,al.distancia)*40)-22
          :(transverty(al.ejeY,al.distancia)*40)-11)}),
       scale({hoveredData && hoveredData == al ? (al.tiempoN/40):(al.tiempoN/80)})"
       stroke-opacity={al.Vivis == "CABA" ? "0" : "1"}
       
       
       />

       
    </g>
           
        {/each}
    
      
      </svg>
        
      {#if hoveredData}
        <Tooltip data={hoveredData} {transvertx} {transverty}/>

        {/if}

      
      
  
    </div>
    
    <div class="cuerpo">
      <h3 style="color:beige">Tu camino a Ditella</h3>
      <p>En base a los datos recopilados de los alumnos de Visualizacion de Datos, discretizamos en el grafico la distancia a la que vive cada alumno, siendo el "sol" la Universidad. 
        El tamaño representa el tiempo aproximado en el que tardan en llegar y los colores varian dependiendo del medio de transporte que utilizan para el viaje. Si un planeta posee la mancha, significa que el alumno representado usa el "diteliano" para llegar.
        Si posee anillos, es porque vive en la Provincia de Buenos Aires, caso contrario, el alumno vive en CABA.
      </p>
      <h3 style="color:beige; text-align:center;">¡Intentá pasar el mouse sobre algún planeta para más info!</h3>
  
    </div>

    <div class="cuerpo2">
      <h3 style="color:beige">Autores</h3>
      <p>Manuel Milde</p>
      <p>Gonzalo García Vence</p>
      <p>Ezequiel Grinblat</p>
      
  
    </div>

  </div>

  
  


</main>

<style>
  :global(body) {
    background-color:#1B1B1E;
    background-image: url("/images/Desktop-2.png");
    
  }
  .header1 {
    display: flex;
    justify-content: center;

  }
  .header {
    display: flex;
    justify-content: center;
  
    flex-direction: column;
    width: auto;
    height: auto;
    margin-top: 70px;
    margin-bottom: 10px;
    background-color: #10100E;
    border-radius: 10px;
    
    
  }
  .headline {
    font-size: 48px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
    color: wheat;
    
  } 
  .logo {
    display: flex;
    align-content: center;
    align-self: left;
    justify-content: center;
    margin-top: 20px;
    scale: 80%;
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
    background-color: rgba(14, 14, 15, 0.347);
    border-radius: 10px;
    margin-bottom: 10%;
    
    
  }
  .parte-b-l {
    
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  /* ------------------------------------------------------glosario */ 
  .glosario {
    margin-top: 1px;
    margin-bottom: 10px;
    display: flex;
    flex-direction: row;
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#10100E;
    padding: 5px;
    justify-content: center;
    width: auto;
    fill: #10100E;

  }
  .reftitle {
    display: flex;
    flex-direction: row;
    margin-left: 2%;
    margin-right: 5px;
    align-items: center;
    background-color:#10100E;
  }
  .glosario-ref {
    display: flex;
    flex-direction: row;
    margin-left: 2%;
    margin-right: 5px;
    align-items: center;
    background-color:#10100E;
    
  }
  .circulo-glos-svg {
    width: 40px;
    height: 40px;
  }
  .circulo-glosario {
    stroke: "wheat" ;
    stroke-width: "1px";

  }

  /* ------------------------------------------------------cuerpo */
  .cuerpo {
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#10100E;
    margin-top: 10px;
    margin-bottom: 5px;
    padding: 25px;

  }
  .cuerpo2 {
    color: wheat;
    border-radius: 10px;
    font-family: Helvetica;
    background-color:#1B1B1E;
    margin-top: 10px;
    margin-bottom: 5px;
    padding: 25px;

  }

  /* ------------------------------------------------------Grafico */
  .graf-container {
    display: flex;
    flex-direction: column;
    height: 715px;
    width: 715px;
    background-color: #10100E;
    justify-content: center;
    
    align-content: center;
    border-radius: 10px;
    margin-top: 5px;
    margin-bottom: 5px;
    padding: 1%;
    align-items: flex-start;
    fill: #1B1B1E;
    margin-left: 21%;
    
  }
  .graf {
    position: relative;
    overflow: hidden;
    height: 510px;
    width: 510px;
    left: 0;
    top: 0;
    margin: auto;
    border-radius: 10px;
    background-color:#10100E;
    scale: 140%;
    margin-top: 10px;
    margin-bottom: 10px;
    padding: 10px;
    max-width: 100%;
    
  }
  .texto-graf {
    font-family: Helvetica;
    
  }
  .alumnos {
    transition:all 300ms ease;
  }
  .anillo-overlay {
    stroke-width: 1px;
    top: 30px;
    fill-opacity: 0;
    transition:transform 300ms ease;
    transition-property: transform;
    
  }
  .mancha-overlay {
    transition:transform 300ms ease;
    transition-property: transform;
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

</style>
