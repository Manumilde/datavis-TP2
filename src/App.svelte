<script>
  import * as d3 from "d3"
  import {onMount} from "svelte"
  // import bsasOverlay from './assets/circuTest.svg';

  /* Array donde guardaremos la data */
  let alumnos = []

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

  /* 5. Escala para diteliano */
  let colorDiteliano = d3.scaleOrdinal()
    .domain(["k", "m"])
    .range(["gold", "white"])

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
    <img src="/images/utdtLOGO.png" width="100" alt="anillos" />
    <h3 class="headline">
      <b>Tu Camino a Ditella</b>
      Distancia de Alumnos
    </h3>
    <p class="bajada">Comparando como llega cada persona a la Universidad</p>
  </div>

  <div class="glosario">

    <p>Amarillo: Caminando</p>
    <p>Rojo: Bondi</p>
    <p>Verde: Transporte Propio</p>
    <p>Azul Oscuro: Subte</p>

  </div>

  
  <!-- Conedor de las entidades -->
  <div class="container">
    <!-- Iteramos la data para visualizar c/ entidad -->
    {#each alumnos as dep}
      <div class="person-container">
        <div class="medal"
          style="background-color: {colorDiteliano(dep.Diteliano)}"
          ></div>
        <div
          class="person"
          style="border-width: 2px; 
        background-color:{colorTransporte(dep.Venis)}; 
        width: {2 * radioAltura(dep.tiempoN)}px; 
        height: {2 * radioAltura(dep.tiempoN)}px; 
        border-color: {colorUbicacion(dep.Vivis)}"
        ></div>
        <p class="name">
          <b>{dep.nombre}</b>
          <br />
          {dep.distName}
        </p>

      </div>
    {/each}
  </div>
</main>

<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    margin-bottom: 80px;
  }
  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 100px;
  }
  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
  }

    
  .person {
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    background-color: pink;
  }
  .medal {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: gold;
    margin: 5px 0;
  }
  /* .bsas-overlay-container {
    position: relative;
  }

  .bsas-overlay {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: red; /* Temporary for debugging */
  /* }  */
  .name {
    font-size: 14px;
    color: rgb(65, 65, 65);
    font-weight: normal;
    text-align: center;
    margin-top: 5px;
  }
  

</style>
