---
title: Kalman Filter
categories: Computer
image: 
---

---

應用場景:

1. 沒辦法直接測量到自己想量的變數, 只能量到間接變數
    ex: 無法測量火箭噴射口內溫度, 只能測量噴射口外
2. 有多種sensor, 但都有誤差

概念:
- 用模型假設欲觀測之變數(先驗)
- 有多個sensor採取加權(也可以是uniform)平均給出一個估計值 
- 用觀測值去更新模型之參數(後驗)
- 反覆迭代運行


舉例來說:

GPS 定位經過訊號不穩的地方(隧道)，我們基於k-1時刻的位置X經過映射F, 加上外力u(加速度)與其相對應的映射B, 以及無法估計的隨機noise w(一般用常態估計 N(0, Q)) 得到k時刻的估計值 $$ x_k $$, 
加上當下GPS給出的位置做出更好的預測, GPS給出的位置則用H作用在$$ x_k $$加上noise $$ v_k $$(一般用常態估計 N(0, R)) 得到最終的估計值, 這個"H"可以說是根據sensor的反饋對預測作出修正。

Note: Common practice is to conservatively set Q and R slightly larger than the expected values to get robustness.

k 表示時間

<div class="cmath">

$$x_k = Fx_{k-1} + Bu_{k-1} + w_{k-1} \longrightarrow (1)  $$  
$$z_k = Hx_k + v_k\longrightarrow (2)$$ 將空間由模型估計映射到觀測空間

$$\hat{y_k} = K(z_k-\hat{z_k}) \longrightarrow (3) $$, 模型預測誤差

Consider the multiple of the distributions of $$z_k $$ and $$ \hat(z_k)$$ and serve the result of final distribution as our posterior model.

The update estimate of $$ x_k$$ is $$\hat{x_k} + K_k \hat{y_k} $$ which is exactly the addition of the multiple of residual and $$ K_k $$ here called Kalman gain. 

How beautiful it is ?

</div>


證明可由gaussion pdf相乘推導出新的mu, sigma


<figure>
    <img src="{{ 'assets/img/kalman/20230712_2.png' | relative_url }}">
</figure>


<figure>
    <img src="{{ 'assets/img/kalman/20230712_1.png' | relative_url }}">
</figure>


-----------------------------------------------------------------------
<div>
src:
    <ul>
		<li>How a Kalman filter works, in pictures: https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/</li>
        <li>https://www.intechopen.com/chapters/63164</li>
		
    </ul>
</div>



<script id="MathJax-script"  src="{{site.baseurl}}/js/math/math.js"></script>
<script id="MathJax-script1"  src="{{site.baseurl}}/js/math/MathJax.js"></script>
<script id="MathJax-script2"  src="{{site.baseurl}}/js/math/MathMenu.js"></script>
<script id="MathJax-script3"  src="{{site.baseurl}}/js/math/MathZoom.js"></script>
<script> 
		var elements = document.getElementsByClassName('MathJax');

		for (var i = 0; i < elements.length; i++) {
		var element = elements[i];
		element.style.fontSize = "100%";
		}
</script>
<style>
