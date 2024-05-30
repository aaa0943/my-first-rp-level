let particles = [];
let collisionOccurred = false;

function setup() {
  createCanvas(800, 800);
  // 초기 입자 생성: 서로 반대 방향에서 중앙으로 이동
  particles.push(new Particle(width / 4, height / 2, 2, 0, 'red'));
  particles.push(new Particle(3 * width / 4, height / 2, -2, 0, 'blue'));
}

function draw() {
  background(0);
  for (let particle of particles) {
    particle.update();
    particle.display();
  }
  
  // 충돌 감지 및 처리
  if (!collisionOccurred && dist(particles[0].x, particles[0].y, particles[1].x, particles[1].y) < 10) {
    collisionOccurred = true;
    createCollisionParticles();
  }
}

function createCollisionParticles() {
  particles = [];
  for (let i = 0; i < 10; i++) {
    let angle = random(TWO_PI);
    let speed = random(1, 3);
    particles.push(new Particle(width / 2, height / 2, speed * cos(angle), speed * sin(angle), 'white'));
  }
}

class Particle {
  constructor(x, y, vx, vy, color) {
    this.x = x;
    this.y = y;
    this.vx = vx;
    this.vy = vy;
    this.color = color;
  }

  update() {
    this.x += this.vx;
    this.y += this.vy;

    // 화면 경계에서 반사
    if (this.x < 0 || this.x > width) this.vx *= -1;
    if (this.y < 0 || this.y > height) this.vy *= -1;
  }

  display() {
    noStroke();
    fill(this.color);
    ellipse(this.x, this.y, 10, 10);
  }
}
