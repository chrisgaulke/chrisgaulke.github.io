
<style>

.carousel {

    overflow: hidden; 
    height: 450px;

}


.carousel-content {
  position: absolute;
  bottom: 50%;
  z-index: 20;
  color: black;
  background-color: rgba(255, 255, 255, 0.4);
  width: 100%;
  height: 10%; 
}

@-webkit-keyframes zoom {
  from {
    -webkit-transform: scale(1, 1);
  }
  to {
    -webkit-transform: scale(1.25, 1.25);
  }
}

@keyframes zoom {
  from {
    transform: scale(1, 1);
  }
  to {
    transform: scale(1.25, 1.25);
  }
}

.carousel-inner .carousel-item > img {
  -webkit-animation: zoom 10s;
  animation: zoom 10s;
  margin-top: -10%;
  margin-bottom: -10%;
}

.card-img-top {
    height: 20vw;
    object-fit: cover;
}

</style> 



<div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="3"></li>
  </ol>
  <div class="carousel-inner">
        <div class="carousel-item active" data-interval="5000">
            <img src="sites/docs/images/infected_004-false_color.png" class="d-block mx-auto w-100" alt="...">
        </div>
        <div class="carousel-item" data-interval="5000">
            <img src="sites/docs/images/network2.png" class="d-block w-100 mx-auto"  alt="...">
        </div>
        <div class="carousel-item" data-interval="5000">
            <img src="sites/docs/images/reduced_circos_2.png" class="d-block w-100" alt="...">
       </div>
        <div class="carousel-item" data-interval="5000">
            <img src="sites/docs/images/Gut_2.png" class="d-block w-100" alt="...">
    </div>
  </div>
  <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

<hr /> 

<row> 

<div> 
<h3 style="text-align: center; font-style: oblique;">Dr Christopher Gaulke's research focuses on understanding the mechanisms through which the gut microbiome mediates the impacts of environmental exposures on host physiology</h3>
</div>
<hr />


<div class="card-group">

  <div class="card" style="max-width:100%; border:none;">
    <div class="card-body">
      <img class="card-img-top" src="sites/docs/images/infected_004-false_color.png" alt="hem-int" style="width:100%">
      <h4 class="card-title" style="text-align: center;">Host-Microbe Interactions</h4>
      <p class="card-text"> Understanding the mechanisms that underpin associations between the microbiome and health.</p>
    </div>
   
    <div class="card" style="background-color: #2C3E50; margin: 0px 15px; text-align: center; padding: 10px 0;">
      <a href="#" style="color: white;"><h4>Read More</h4></a>
    </div>
 
  </div>
  
 <div class="card" style="max-width:100%; border:none;">
    <div class="card-body">
      <img class="card-img-top" src="sites/docs/images/Gut_2.png" alt="Microbial Exposure Science" style="width:100%">
      <h4 class="card-title" style="text-align: center;">Microbial Exposure Science</h4>
      <p class="card-text"> Elucidating how environmental exposures impact host-microbiome interactions.</p>
    </div>
   
    <div class="card" style="background-color: #2C3E50; margin: 0px 15px; text-align: center; padding: 10px 0;">
        <a href="#" style="color: white;"><h4>Read More</h4></a>
    </div>
 
  </div>

  <div class="card" style="max-width:100%; border:none;">
   <div class="card-body">
      <img class="card-img-top" src="sites/docs/images/reduced_circos_2.png" alt="big data" style="width:100%">
      <h4 class="card-title" style="text-align: center;">Nutritional Microbiology</h4>
      <p class="card-text"> Quantifying the effects of dietary variation on microbiome operation.</p>
    </div>
    
    <div class="card" style="background-color: #2C3E50; margin: 0px 15px; text-align: center; padding: 10px 0;">
        <a href="#" style="color: white;"><h4>Read More</h4></a>
    </div>
  </div>
</div>

</row> 
