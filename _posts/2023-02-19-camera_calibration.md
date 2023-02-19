---
title: 相機標定
categories: Computer
image: 
---

---
## 1. 一般成像


<br>

<div style="padding-bottom:10%">
	<figure>
		<img src="{{ 'assets/img/camera_calibration/camera_image_1.png' | relative_url }}" class="center">
		<figcaption align="center">Fig.1 - 成像過程 </figcaption>
	</figure>
</div>




	世界座標到像素座標轉換過程:

	世界座標 => 相機座標 => 圖像物理座標 => 圖像像素座標

	世界座標中一點P, 射向光軸上相機中心點C(紅線) 在黃色平面上(z=f)成像點'P, 該平面為CCD圖像, 原點為中心點,座標使用(x, y)表示, 
	投影到圖像像素座標系,且離散化,成像寬度W高度H, 座標用(u,v)表示, 原點在左上角。

	Note: fx, fy 差異來自sensor映射到圖像的關係

<div class="cmath">
$$
Z_c \begin{bmatrix}
x \\ y  \\ z
\end{bmatrix} = \begin{bmatrix}
f_x &&  0 && u_0 && 0 \\
0 && f_y && v_0 && 0 \\
0 && 0 && 1 && 0
\end{bmatrix}
\begin{bmatrix}
R && t \\ 0_{1x3} && 1
\end{bmatrix}
\begin{bmatrix}
X_W \\ Y_W \\ Z_W \\ 1
\end{bmatrix}

$$</div>

---

## 2. 魚眼 & 全景

在某些場景, 我們需要更大範圍的視角, 魚眼是透過多個鏡面折射增加FOV, 而全景則是利用鏡子來達到 360度的視野範圍


<div class="wrap">
	<div class="left" >
		<div style="height:80%">
			<figure style="padding-top:20%; padding-bottom:20%">
				<img src="{{ 'assets/img/camera_calibration/fisheye.png' | relative_url }}">
			</figure>
		</div>
		<div style="height:20%">
			<figcaption  align="center">Fig.2- 魚眼</figcaption>	
		</div>
	</div>

	<div class="right">
		<figure>
			<img src="{{ 'assets/img/camera_calibration/omnidirection.png' | relative_url }}">
			<figcaption  align="center">Fig.3 - 全景</figcaption>
		</figure>
	</div>
</div>


---

## 3. 校正


#### 3.1 校正摘要


<figure>
	<img src="{{ 'assets/img/camera_calibration/proof_unit_circle.png' | relative_url }}" class="center">
	<figcaption align="center">Fig.4 - 投影至單位圓 </figcaption>
</figure>


首先, Fig.4 說明Q'跟Q是相同的點, 會在經過F的平面成像。 這樣一來從P經過mirror(橘線)投影到線l 等價於將P投影至單位圓
上一點R', 再從N點對R'做stereographic projection。 mirror的幾何圖形可推廣至圓錐曲線<sub>(3)</sub>。

接著看一下unified projection model, 步驟如下:

1. 世界座標一點經過映射到單位圓上
2. 改變參考點到點 $$C_p = (0,0,\xi)$$ (新原點),  $$\xi$$為離心率, 依據不同圓椎曲面改變
3. 投影到正規化平面 (make z=1)
4. 鏡頭內參轉換 K => 圖像座標

其中 $$f_1 = f*s_x$$, $$f_2 = f*s_y$$, $$s_x, s_y$$ 表示sensor與pixel在x,y方向映射關係,
$$\eta$$ 為collineation 乘數,用$$\gamma_i = f_i \eta$$估計, 作用於normal plane與image plane之間。


---
**完整的成像函數**:

$$ G = P。D。H。W $$, 等號右側從左至右依序為 pin-hole model projection, distortion, mirror, extrinsic

distortion:

$$\rho = \sqrt{x^2+y^2}$$

- radial (鏡面扭曲):

	 $$L(\rho) = 1 + k_1 \rho^2 + k_2 \rho^4 + k5\rho^6 $$

- misalignment (sensor偏移) 

<div class="cmath">
$$
dx = \begin{bmatrix}
2k_3xy + k_4(\rho^2 + 2x^2) \\ 
k_3(\rho^2 + 2y^2) + 2k_4xy  
\end{bmatrix} 
$$
</div>


給定m個世界座標$$g_i$$對應圖像位置 $$e_i$$ => 內參校正過程是在最小化下列方程式

$$F(x) = \frac{1}{2}\sum_{i=1}^m[G(V,g_i) - e_i]^2$$


#### 3.2 求解

1. 初始化未知參數 $$k_i = \alpha = 0, \forall i = 1,2...,5 $$, $$\gamma_1 = \gamma_2$$, 並以這些初始值求解外參
2. (optional[for mirror]) 
	2.1 移除中心跟邊界的點
      2.2 移除跟重點範圍太遠的點
      2.3 將剩餘的點放入 list 中, 隨機sample用median估計mirror center 跟radius

3. 估計$$\gamma$$

p = (u, v)為圖像座標一點, 代入normal plane 反投影公式$$h^{-1}(m)$$

$$\gamma: p_c = \gamma m $$ =>


<div class="cmath">
$$
h^{-1}(m) = \begin{bmatrix}
u_c \\ 
v_c \\
g(u_c, v_c)  
\end{bmatrix} 
$$
</div>

其中 $$g(u_c, v_c) = \frac{\gamma}{2} - \frac{1}{2\gamma}(u^2_c + v^2_c)$$, 

Note: 轉換到m 要同乘 $$\gamma$$ 才是圖像座標

透過關聯世界座標與圖像座標來解未知參數, 由兩個限制式可得解:

1. 四點共線 (世界座標)
2. 所有點都在normal plane "N" 上

=> 得$$\gamma$$

最後透過每個校正圖的corner找外參, 剩餘的點用來reproject 計算error並透過最小化error優化參數

camodocal是用non-linear optimization - ceres solver 來對內參(k1~k6, $$\gamma1, \gamma2$$)做優化。




---------------------------------------------------------------------------------------------------
src:
- (1)一般成像: https://blog.csdn.net/weixin_44278406/article/details/112986651
- (2)魚眼: https://blog.csdn.net/u010128736/article/details/52864024
- (3)Catadioptric: https://repository.upenn.edu/cgi/viewcontent.cgi?article=1096&context=cis_papers
- (4)Camodocal: https://people.inf.ethz.ch/pomarc/pubs/HengIROS13.pdf
- (5)MEI: chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.robots.ox.ac.uk/~cmei/articles/single_viewpoint_calib_mei_07.pdf
- (6)Ceres: https://blog.csdn.net/wzheng92/article/details/80008380

<script id="MathJax-script"  src="{{site.baseurl}}/js/math.js"></script>
<script id="MathJax-script1"  src="{{site.baseurl}}/js/MathJax.js"></script>

<style>
body > div > div:not(#MathJax_Font_Test) {
    min-width: 960px;
}


.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

.wrap{
  width: 100%;
  display: flex;
  justify-content: space-between;
  height: 200;
}

.left{
  width: 50%;
}

.right{
  width: 50%
}
</style>