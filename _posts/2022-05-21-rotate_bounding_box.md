---
title: 旋轉目標檢測方法
categories: Computer
image: 
---


## 常見的角度回歸方法



![definition]({{ "assets/img/rotate_bounding_box_fig1.PNG" | relative_url }})

- opencv定義法: $$\theta$$ 是指框與x軸所形成的銳角，將此邊定義為w，$$\theta$$ 定義域 [-90, 0)

- 長邊定義法: $$\theta$$ 是指長邊h與x軸所形成的夾角，定義域[-90, 90)

- 有序四邊形定義法，最左邊的點為起始點，採取逆時針排列( [x1,y1,x2,y2,x3,y3,x4,y4] )

![example]({{ "assets/img/rotate_bounding_box_fig2.PNG" | relative_url }})

(a) 在90度定義中，由於角度的週期性(POA) + 邊的可交換性(EoE) ，加上w跟h的縮放，讓回歸問題變得很複雜

(b) 定義了邊，所以只剩下POA

(c) 會有點跟點回歸不是最小路徑的問題(理同:POA)


## 把角度當成分類問題

- one-hot label encoding

定義 $$w = AR/C_{\theta}$$ , AR跟 $$C_{\theta}$$ 分別為角度全域(180度)跟種類個數

舉例來說: 

w = 1， 是指將180度分成180類，每類長度為1

對於0.5的單位來說，我們預測會造成最大精度的誤差為w/2 = 0.5

平均誤差為 (0, w/2)的均勻分配之cdf = w/4 = 0.25。

以一個比例為1:9的矩形來計算，將此矩形位移0.25度,0.5度後來計算與原來矩形IOU，分別下降0.02跟0.05，對最後結果的影響不大。

但隨著比例增大加上物件類別的增加，結果也隨之變差。

此外類別不能表現出實際損失1度跟169度差距很大，在分類上的距離卻是一樣的


因此這邊提出另一個方式: Circular Smooth Label for Angular Classification

### Circular Smooth Label for Angular Classification (CSL)



<div class="cmath"> $$ 

CSL(x) =
      \begin{align}
        &g(x), \theta - r < x < \theta + r \\
        &0, otherwise     \\\end {align}
$$</div>




其中r 是半徑, g(x)是window function. 

滿足以下幾點性質:

- 週期性
- 對稱性
- Max(g) = 1
- 單調性

ex: gaussian, pulse, triangle, rectangular function


![different-coding]({{ "/assets/img/rotate_bounding_box_fig4.PNG" | relative_url }})


<div>
  src
  <ul>
    <li> https://www.cvmart.net/community/detail/2862 </li>
    <li> https://arxiv.org/pdf/2003.05597.pdf </li>
  </ul>
</div>


<script id="MathJax-script"  src="{{site.baseurl}}/js/math/math.js"></script>
<script id="MathJax-script1"  src="{{site.baseurl}}/js/math/MathJax.js"></script>
<script id="MathJax-script2"  src="{{site.baseurl}}/js/math/MathMenu.js"></script>
<script id="MathJax-script3"  src="{{site.baseurl}}/js/math/MathZoom.js"></script>

<style>
body > div > div:not(#MathJax_Font_Test) {
    min-width: 960px;
}

img {
	max-width: 100%;
}



header ul li{
    box-sizing: border-box;
    padding: 0;
}
</style>

