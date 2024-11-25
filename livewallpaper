float time = 0;
ArrayList<MatrixColumn> matrixRain;
ArrayList<Affirmation> affirmations;
PFont matrixFont;
int currentAffirmation = 0;
color amberColor = color(255, 191, 0);  // Rich amber color
PFont boldFont;
float lastTime = 0;
float deltaTime = 0;
float affirmationAlpha = 255;
boolean isTransitioning = false;

void setup() {
  size(1080, 2340, P3D);  // Added P3D for proper 3D rotation
  
  // Try different system fonts for the best bold appearance
  String[] fontNames = {
    "Roboto-Bold",
    "SansSerif-Bold",
    "Dialog-Bold",
    "Verdana-Bold",
    "DejaVuSans-Bold",
    "SourceSansPro-Bold"
  };
  
  boolean fontFound = false;
  for (String fontName : fontNames) {
    try {
      boldFont = createFont(fontName, 32);  // Increased size for better visibility
      fontFound = true;
      break;
    } catch (Exception e) {
      continue;
    }
  }
  
  if (!fontFound) {
    // If no preferred fonts found, create a bolder look with the default font
    boldFont = createFont("SansSerif", 32);
  }
  
  // Initialize matrix rain
  matrixRain = new ArrayList<MatrixColumn>();
  for (int i = 0; i < width/35; i++) {  // Increased spacing between columns
    matrixRain.add(new MatrixColumn(i * 35));
  }
  
  // Initialize affirmations
  affirmations = new ArrayList<Affirmation>();
  String[] messages = {
   "I am more than my physical body",
    "Energy flows where attention goes",
    "My potential is boundless",
    "I shape my reality",
    "I am resilient, strong, and calm",
    "Each moment is a chance to transform",
    "I focus on what elevates me",
    "I am a creator, a seeker, a force of nature"
  };
  
  for (String msg : messages) {
    affirmations.add(new Affirmation(msg));
  }
}

void draw() {
  // Calculate delta time for smooth animations regardless of frame rate
  float currentTime = millis();
  deltaTime = (currentTime - lastTime) / 1000.0;
  lastTime = currentTime;
  time += deltaTime * 0.3;  // Changed from 0.4 for slightly slower rotation
  
  background(0);
  
  // Optimize matrix rain
  hint(DISABLE_DEPTH_TEST);
  int visibleColumns = 0;
  for (MatrixColumn col : matrixRain) {
    if (col.isVisible()) {
      col.update();
      col.display();
      visibleColumns++;
      if (visibleColumns > 25) break;  // Limit maximum visible columns
    }
  }
  
  // Optimize DNA rendering
  hint(ENABLE_DEPTH_TEST);
  pushMatrix();
  translate(width/2, height/2);
  drawDNA();
  popMatrix();
  
  // Smooth affirmation transitions
  if (frameCount % 300 == 0) { // Every 5 seconds
    isTransitioning = true;
  }
  
  Affirmation currentAff = affirmations.get(currentAffirmation);
  
  if (isTransitioning) {
    currentAff.alpha -= 8; // Slower fade out (changed from 10)
    
    if (currentAff.alpha <= 0) {
      currentAffirmation = (currentAffirmation + 1) % affirmations.size();
      currentAff = affirmations.get(currentAffirmation);
      currentAff.alpha = 0;
      isTransitioning = false;
    }
  } else if (currentAff.alpha < 255) {
    currentAff.alpha += 8; // Slower fade in (changed from 10)
  }
  
  hint(DISABLE_DEPTH_TEST);
  currentAff.display();
}

void drawDNA() {
  float helixRadius = 100;
  float helixHeight = 800;
  int steps = 60;  // Reduced steps for better performance

  pushMatrix();
  rotateY(time * 0.3);
  
  // Optimized DNA strands
  strokeWeight(5);  // Thicker strands
  for (int strand = 0; strand < 2; strand++) {
    beginShape();
    noFill();
    stroke(255, 191, 0, 255);
    for (int i = 0; i <= steps; i += 2) {  // Increased step size
      float angle = map(i, 0, steps, -PI, PI) + time + strand * PI;
      float y = map(i, 0, steps, -helixHeight / 2, helixHeight / 2);
      float x = cos(angle) * helixRadius;
      float z = sin(angle) * helixRadius;
      vertex(x, y, z);
    }
    endShape();
  }

  // Simplified base pairs
  stroke(255, 191, 0, 128);
  for (int i = 0; i <= steps; i += 6) {  // Reduced number of base pairs
    float angle = map(i, 0, steps, -PI, PI) + time;
    float y = map(i, 0, steps, -helixHeight / 2, helixHeight / 2);
    float x1 = cos(angle) * helixRadius;
    float z1 = sin(angle) * helixRadius;
    float x2 = cos(angle + PI) * helixRadius;
    float z2 = sin(angle + PI) * helixRadius;
    line(x1, y, z1, x2, y, z2);
  }
  
  popMatrix();
}


class MatrixColumn {
  float x, y;
  float speed;
  char[] chars;
  int len;
  
  MatrixColumn(float x) {
    this.x = x;
    reset();
    len = int(random(5, 12));
    chars = new char[len];
    for (int i = 0; i < len; i++) {
      chars[i] = getRandomChar();
    }
  }
  
  void reset() {
    y = random(-200, 0);
    speed = random(1, 3);
  }
  
  void update() {
    y += speed;
    if (y > height) reset();
    if (frameCount % 8 == 0) {
      for (int i = 0; i < len; i++) {
        if (random(1) < 0.05) chars[i] = getRandomChar();
      }
    }
  }
  
  void display() {
    textFont(boldFont);
    textAlign(CENTER);
    textSize(42);  // Slightly smaller for matrix characters
    
    // Bright leading character
    fill(255, 255, 200, 220);
    text(chars[0], x, y);
    text(chars[0], x+1, y);  // Make matrix characters bolder too
    
    // Optimize character rendering
    int visibleChars = min(len, 8);
    for (int i = 0; i < visibleChars; i++) {
      float alpha = map(i, 0, visibleChars, 150, 10);
      fill(255, 191, 0, alpha);
      text(chars[i], x, y - (i * 20));
      text(chars[i], x+1, y - (i * 20));  // Bold effect
    }
  }
  
  char getRandomChar() {
    return char(0x30A0 + int(random(96)));
  }
  
  boolean isVisible() {
    return (x >= 0 && x <= width && y <= height + 200);
  }
}

class Affirmation {
  String text;
  float y;
  float targetY;
  float alpha = 255;
  
  Affirmation(String text) {
    this.text = text;
    this.y = height * 0.85;
    this.targetY = this.y;
  }
  
  void display() {
    textFont(boldFont);
    textAlign(CENTER, CENTER);
    textSize(32);  // Increased size
    
    // Make text bolder by drawing it multiple times with slight offset
    // Floating animation
    y = lerp(y, targetY + sin(time * 1.5) * 10, 0.1);
    
    // Simplified glow effect with fade
    for(int i = 3; i > 0; i--) {
      float glowAlpha = map(i, 3, 0, 20, 60);
      glowAlpha *= (0.5 + 0.5 * sin(time * 1.5)) * (alpha / 255.0);
      float spread = i * 2;
      fill(255, 191, 0, glowAlpha);
      // Draw multiple times for bolder appearance
      text(text, width/2 + spread + 1, y);
      text(text, width/2 + spread - 1, y);
      text(text, width/2 + spread, y + 1);
      text(text, width/2 + spread, y - 1);
    }
    
    // Main text with fade - drawn multiple times for boldness
    fill(255, 191, 0, alpha);
    text(text, width/2 + 1, y);
    text(text, width/2 - 1, y);
    text(text, width/2, y + 1);
    text(text, width/2, y - 1);
    text(text, width/2, y);  // Center text
  }
}
