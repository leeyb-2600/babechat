<!DOCTYPE html>
<html>
<head>
<style>
  * { margin: 0; padding: 0; }
  
  body {
    background: transparent;
  }

  .container {
    position: relative;
    width: 500px;
    height: 500px;
  }

  .container img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .bg       { z-index: 1; }
  .weather  { z-index: 2; }
  .event    { z-index: 3; }
  .fart     { z-index: 4; }
  .character{ z-index: 5; }
</style>
</head>
<body>

<div class="container">
  <img class="bg"        src="https://itimg.kr/3934/WH/ST.webp">
  <img class="weather"   src="https://itimg.kr/3934/WX/WX.webp">
  <img class="event"     src="https://itimg.kr/3934/EVNT/OFF.webp">
  <img class="fart"      src="https://itimg.kr/3934/O2/FT.webp">
  <img class="character" src="https://itimg.kr/3934/PJ/BF1.webp">
</div>

</body>
</html>
