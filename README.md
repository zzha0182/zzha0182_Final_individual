# zzha0182_Final_individual
## The Instruction of Individual Work:
My personal work, the work presented a regular, tension animation. Based on perlin noise and randomness, some elements of the picture are created by changing their size, color and position.
## The Individual Code Chose:
Perlin noise
## The Animation Explanation:
The way I chose to animate was Perlin noise with randomness. Compared with click or time driven, Perlin noise can achieve a more natural, continuous and non-repetitive change effect, so that the image presents a soft sense of fluctuation.

My animation extension didn't change the structure of the team's drawing. Instead, it was a function wrapper that superimposed dynamic effects on top of the original drawing. These animations include:

Outer color block animation: The color block fluctuates continuously, creating a sense of rhythm like the rotation of the color wheel.
1. Dynamic color change of petals: simulate the "breathing" blooming of flowers through color change.
2. Green ring flicker: Saturation and brightness change based on noise, making it appear to glow.
3. Solar and Lunar disk zooming: Slightly zooming animations simulate the sun and moon breathing.
4. Blue circle pulsating: constantly changing color and size, conveying the visual sense of wavy water.
5. Egg ring light rotation and breathing: rotation and scaling to show its softness and instability.
frameCount * 0.01 was used as the driving variable for all animations to form a uniform animation rhythm for the whole figure.

## The Inspiration:

My inspiration comes from the exploration of ritual, circular structure and pattern order. Inspired by Hilma Af Klint, I hope to construct a visual rhythm that is both unified and rich through repetitive graphic language.

Each circle resembles a planet, some symbolising the sun and moon, others a dessert or a plate, embodying a metaphorical link between vision and taste. The background of the work is divided into black and white diagonals, forming a clear visual guidance and enhancing the overall composition tension.

We want these elements to be more than just static decoration, but to express a sense of life and rhythm through subsequent animation, making it a "breathing image".
![inspiration-static](inspiration.jpg)

## The Technical Explanation:
I used the noise() function from p5.js to generate smooth and continuous pseudo-random values that drive the shape in multiple dimensions, such as color, size, rotation, and so on.

Here are the main animation modules and their code descriptions:
This is drawAnimatedSectors
```
function drawAnimatedSectors(cx, cy, t) {
  const numBlocks = 36;
  const angleStep = TWO_PI / numBlocks;
  const radius = RADIUS;

  noStroke();
  for (let i = 0; i < numBlocks; i++) {
    // use Perlin noise generating colors
    const n = noise(t + i * 0.1);
    const r = map(n, 0, 1, 100, 255);
    const g = map(n, 0, 1, 50, 200);
    const b = map(n, 0, 1, 100, 255);
    fill(r, g, b, 180); 

    arc(
      cx, cy,
      radius * 2, radius * 2,
      i * angleStep,
      (i + 1) * angleStep,
      PIE
    );
  }
}
```
This is drawAnimatedPetals
```
function drawAnimatedPetals(cx, cy, t) {
  const numPetals = 14;
  const petalAngle = TWO_PI / numPetals;
  const baseLength = RADIUS * 1.2;
  const petalWidth = 12;
  const petalOffset = baseLength / 2;

  noStroke();
  for (let i = 0; i < numPetals; i++) {
  
    const n = noise(t + i * 0.3);
    const r = map(n, 0, 1, 150, 255);
    const g = map(n, 0, 1, 50, 180);
    const b = map(n, 0, 1, 100, 255);
    fill(r, g, b, 200); 

    const theta = i * petalAngle - PI / 2;

    push();
      translate(cx, cy);
      rotate(theta);
      ellipse(petalOffset, 0, baseLength, petalWidth);
    pop();
  }
}
```

This is drawAnimatedGreenRing
```
// Here I want to enhance the color contrast (HSB + S/B fluctuation) part2
function drawAnimatedGreenRing(cx, cy, t) {
  const layers = 4;
  const spacing = 14;

  colorMode(HSB, 360, 100, 100, 255);
  noFill();
  strokeWeight(3);

  for (let i = 0; i < layers; i++) {
    const n = noise(t + i * 0.5 + cx * 0.01);
    
    const h = map(n, 0, 1, 0, 360);
    
    const s = map(noise(t + i * 0.7), 0, 1, 50, 100);
   
    const b = map(noise(t + i * 0.9), 0, 1, 50, 100);
    stroke(h, s, b, 200); // alpha 200， The overlay effect can be even stronger

    const d = (RADIUS * 2) - 5 - i * spacing;
    ellipse(cx, cy, d, d);
  }

  // Return to RGB mode to avoid affecting other draws
  colorMode(RGB, 255);
}
```  

This is drawAnimatedSunMoonSize
```
function drawAnimatedSunMoonSize(cx, cy, t) {
  // A scaling factor of 0.8 to 1.2 is generated based on the noise
  const n = noise(t + cx * 0.02);
  const s = map(n, 0, 1, 0.8, 1.2);

  push();
    
    translate(cx, cy);
    scale(s);
    //  Passing (0,0) tells drawSunMoon to draw centered at its current position
    drawSunMoon(0, 0);
  pop();
}
```

This is drawAnimatedBlueRing and drawAnimatedEgg
```
function drawAnimatedBlueRing(cx, cy, t) {
  const layers = 3;
  const spacing = 22;

  colorMode(HSB, 360, 100, 100, 255);
  noFill();
  strokeWeight(5);

  for (let i = 0; i < layers; i++) {
    const n1 = noise(t + i * 0.2);
    const scale = map(n1, 0, 1, 0.9, 1.1);

    const n2 = noise(t + i * 0.5 + cx * 0.01);
    const h  = map(n2, 0, 1, 180, 240);
    stroke(h, 70, 90, 200);

    const baseD = (RADIUS * 2) - 5 - i * spacing;
    const d = baseD * scale;
    ellipse(cx, cy, d, d);
  }

  colorMode(RGB, 255);
}


function drawAnimatedEgg(cx, cy, t) {
  // rotation ±5°
  const angle = map(noise(t + cx * 0.02), 0, 1, -PI / 36, PI / 36);
  // scale 0.95–1.05
  const s = map(noise(t + cy * 0.02), 0, 1, 0.95, 1.05);

  push();
    translate(cx, cy);
    rotate(angle);
    scale(s);
    drawEgg(0, 0);  
  pop();
}
```