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
- 用觀測值去更新模型之參數(後驗)
- 反覆迭代運行




$$\dot{x} 表示下一個時刻的X值 $$

<div class="cmath">

$$\dot{x} = Ax + Bu \longrightarrow (1)$$ , u為控制項, A&B皆為映射
$$y = Cx\longrightarrow (2)$$ 將空間由模型估計映射到觀測空間

$$\hat{\dot{x}} = A\hat{x} + Bu + K(y-\hat{y}) \longrightarrow (3) $$, K(.)的部分為後驗資訊

combine (3) - (1) & (2) 與 (2) hat
$$\dot{e_{obs}} = (A - KC)e_{obs}$$ 此為ODE型態, 
=>
解為: $$e_{obs}(t) = e^{(A-KC)t}e_{obs}(0)$$

A-KC < 0, $$e_{obs} \to 0, as \ t \to \infty$$
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
        <li>Linear Kalman Filter 筆記: https://blog.techbridge.cc/2021/04/11/kalman-filter-%E7%AD%86%E8%A8%98/</li>
		<li>How a Kalman filter works, in pictures: https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/</li>
		
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
