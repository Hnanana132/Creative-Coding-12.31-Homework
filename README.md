# Creative-Coding-12.31-Homework

作品灵感：Sierpiński Triangle

1. 取一个实心的三角形。（多数使用等边三角形）
   
2. 沿三边中点的连线，将它分成四个小三角形。
   
3. 去掉中间的那一个小三角形。
   
4. 对其余三个小三角形重复1。

作品目标：

Keywords：分形，花状

What to do：在画布上绘制三个不共线的点，构成三角形后会不断生成中心三角形的中位线，形成抽象的花状分形。

How to do：1. 设置画布并让使用者点击画布绘制三角形2. 产生第一个分形结构3. 让使用者再次点击画布

代码实现：
int numPoints = 3;

ArrayList<PVector[]> triangles = new ArrayList<PVector[]>();

ArrayList<PVector> midpoints = new ArrayList<PVector>();

void setup() {

  size(800, 600);
  
  frameRate(30);
  
  textAlign(CENTER, CENTER);
  
}

void draw() {

  background(255);

  for (PVector[] triangle : triangles) {
  
    drawTriangle(triangle);
    
  }

  drawMidpoints();
  
}

void drawTriangle(PVector[] triangle) {

  fill(255);  
  
  stroke(255, 255, 0); 
  
  strokeWeight(2);  
  
  beginShape();
  
  for (int i = 0; i < numPoints; i++) {
  
    if (triangle[i] != null) {
    
      vertex(triangle[i].x, triangle[i].y);
      
    }
    
  }
  
  endShape(CLOSE);
  
}

void drawMidpoints() {

  stroke(255, 255, 0);  
  
  noFill();  

  int numMidpoints = midpoints.size();
  
  for (int i = 0; i < numMidpoints - 2; i += 3) {
  
    PVector p1 = midpoints.get(i);
    
    PVector p2 = midpoints.get(i + 1);
    
    PVector p3 = midpoints.get(i + 2);

    line(p1.x, p1.y, p2.x, p2.y);
    
    line(p2.x, p2.y, p3.x, p3.y);
    
    line(p3.x, p3.y, p1.x, p1.y);

    // Add midpoints for the next iteration
    
    if (i + 3 < numMidpoints) {
    
      PVector nextMid1 = new PVector((p1.x + p2.x) / 2, (p1.y + p2.y) / 2);
      
      PVector nextMid2 = new PVector((p2.x + p3.x) / 2, (p2.y + p3.y) / 2);
      
      PVector nextMid3 = new PVector((p3.x + p1.x) / 2, (p3.y + p1.y) / 2);

      midpoints.add(nextMid1);
      
      midpoints.add(nextMid2);
      
      midpoints.add(nextMid3);
      
    }
    
  }
  
}

void mousePressed() {

  PVector mousePos = new PVector(mouseX, mouseY);

  if (triangles.size() == 0 || triangles.get(triangles.size() - 1)[2] != null) {
  
    triangles.add(new PVector[numPoints]);
    
    triangles.get(triangles.size() - 1)[0] = mousePos;
    
  } else {
  
    PVector[] currentTriangle = triangles.get(triangles.size() - 1);
    
    int lastIndex = -1;
    
    for (int i = 0; i < numPoints; i++) {
    
      if (currentTriangle[i] == null) {
      
        lastIndex = i;
        
        break;
        
      }
      
    }

    if (lastIndex != -1) {
    
      currentTriangle[lastIndex] = mousePos;

      if (lastIndex == numPoints - 1) {
      
        calculateMidpoints(currentTriangle);
        
      }
      
    }
    
  }

}

void calculateMidpoints(PVector[] triangle) {

  midpoints.clear();

  for (int i = 0; i < numPoints; i++) {
  
    int nextIndex = (i + 1) % numPoints;

    PVector mid = new PVector((triangle[i].x + triangle[nextIndex].x) / 2, (triangle[i].y + triangle[nextIndex].y) / 2);
    
    midpoints.add(mid);
    
  }

  for (int i = 0; i < numPoints; i++) {
  
    PVector mid = midpoints.get(i);
    
    PVector nextMid = midpoints.get((i + 1) % numPoints);

    midpoints.add(new PVector((mid.x + nextMid.x) / 2, (mid.y + nextMid.y) / 2));
    
  }
  
}
