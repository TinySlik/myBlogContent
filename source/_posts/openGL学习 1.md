---
	title: openGL学习 1
---

今天开始把我以前学习的openGL的皮毛梳理一下，挖一挖深.
# 矩阵旋转原理
x' = r* (cos(alpha) * cos(beta) - sin(alpha) * sin(betha))
   = x* cos (alpha) - y * sin(alpha);
   
y' = x* sin(alpha) + y * cos(alpha);



x'     [cos(alpha) - sin(alpha)]    [x]
   =          					  * 
y'     [sin(alpha) + cos(alpha)]    [y]


