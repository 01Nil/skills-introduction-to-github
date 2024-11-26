let time = 0;
let wave = [];
let running = true;  // Variable para controlar si el código está corriendo o no
let slider;
let button;
let bluePoints = [];  // Array para almacenar los puntos azules

function setup() {
  createCanvas(600, 400);
  slider = createSlider(1, 10, 1);
  
  // Crear el botón para empezar y detener la animación
  button = createButton('Start / Stop');
  button.position(10, 10);  // Coloca el botón en la posición deseada
  button.mousePressed(toggle);  // Asocia la función toggle al botón
}

function draw() {
  background(0);
  translate(220, 200);
  
  let x = 0;
  let y = 0;
  let prevX = 0;
  let prevY = 0;
  
  // Solo dibuja si el código está "corriendo"
  if (running) {
    for (let i = 0; i < slider.value(); i++) {
      prevX = x;
      prevY = y;
      
      let n = i * 2 + 1;
      let radius = 75 * (4 / (n * PI));
      x += radius * cos(n * time);
      y += radius * sin(n * time);
      
      stroke(255, 100);
      noFill();
      
      ellipse(prevX, prevY, radius * 2);
      stroke(255);
      line(prevX, prevY, x, y);
    }
  
    // Línea desde el último punto hacia el gráfico
    wave.unshift(y);
    
    // Guardar el punto azul en el array
    bluePoints.push({x, y});
    
    // Limitar el número de puntos azules almacenados
    if (bluePoints.length > 200) {
      bluePoints.shift();  // Elimina el primer (más antiguo) punto del array
    }
    
    // Dibuja la línea continua de puntos azules
    stroke(0, 0, 255); // Azul
    noFill();
    beginShape();
    for (let i = 0; i < bluePoints.length; i++) {
      vertex(bluePoints[i].x, bluePoints[i].y); // Conectar cada punto en una línea
    }
    endShape();
    
    time += 0.05;
    if (wave.length > 200) {
      wave.pop();
    }
  }
}

function toggle() {
  // Cambia el valor de la variable running entre true y false
  running = !running;
}
