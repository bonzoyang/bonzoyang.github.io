---
layout: post
title:  "維度詛咒(Curse of Dimensionality)"
categories: data-mining
published: true
date: 2020-03-06T22:35:28+08:00
---



### 引言
多數的data mining問題來自資料有許多的feature，導致資料的樣本點散落在一個超高維度的資料空間(data space)，這種超高維度資料所引起的問題被稱為「維度詛咒(curse of dimensionality)」。雖然高維度空間在data mining中是相當常見的，然而高維空間的種種特性對人類來說卻相當違反直覺，畢竟我們只有$$2$$或$$3$$維這種低維度空間的認知。

### 超立方體
高維空間有什麼特性呢？一個單位體在高維空間理所當然會比在低維空間的表面積還大上許多。之所以比表面積，是因為體積在不同維度時是不同單位的，故無法比較。而表面積在$$1$$維以上的空間都一樣是長度平方。如果我們能視覺化一個高維空間中的超立方體(hypercube)，它看起來會像刺蝟這樣，如：
![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.2.png' | absolute_url }}){: .align-center height="30%" width="30%"}

<u>Figure 2.2.</u> 高維超立方體觀念上長得像刺蝟
{: style="text-align: center;"}
隨著維度的數目越大，邊緣(刺)的長度會比本體中間長上許多。超高維度資料有4個特性能作為準則，用以解釋input data做mining後的結果。

### 超高維度資料**4個**重要的特性(準則)
1. 假設密度固定的條件之下，越高維度的空間，所需要的資料「點」數越多，意味著data set所需的資料筆數越多。例如：在$$1$$維空間中假設$$n$$筆資料(即 $$n$$ 個點)所對應的密度是我們所接受的，若要在$$k$$維空間中維持相同的資訊密度，那我們的資料集將爆炸性地成長到 $$n^k$$ 筆資料。若整數$$1—100$$代表$$1$$維空間的$$100$$個資料點，若將空間提升到$$5$$維並且維持相同的資料密度，意味著我們需要$$100^5 = 10^{10}$$ 相異資料點；即使是真實世界中最大的資料集也難逃維度詛咒，正因為大資料集的高維度，代表它們有著相對低的資料密度，低密度使得它難以得到令人滿意的mining結果。

2. 若我們需要「包覆」高維空間中某個「比例」的資料點，那所需的「半徑」會越大。例如：我們可以用下列公式來代表要包覆這些資料點的超立方體所需要的**<font color='orange'>半徑</font>**有多長

	$$e(p)=p^{1/d}$$
	{: style="text-align: center;"}

	其中 $$p$$ 代表所要包覆的資料點比例，$$d$$ 是維度的數量。假如我們要包覆**單位**超立方體中$$10%$$的資料點($$p = 0.1$$)，則在$$2$$維空間的情況下，我們將需要半徑為 $$e_2(0.1) = 0.32$$ 的超立方體，$$3$$維空間則需要半徑為 $$e_3(0.1) = 0.46$$ 的超立方體，到了$$10$$維空間則需要半徑長達 $$e_{10}(0.1) = 0.8$$ 的超立方體才能包覆$$10\%$$資料點，Figure 2.3 為其概念圖。
	![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.3.png' | absolute_url }}){: .align-center height="80%" width="80%"}

	<u>Figure 2.3.</u> 包覆 $$10\%$$ 資料點在$$1$$$維、$$2$$4維、$$3$$維空間所需的超立方體半徑長。
	{: style="text-align: center;"}
	這也表示你必須捕捉某個頂點點周圍相當大的範圍，才能包覆高維空間中一小部分的資料點。

3. 高維空間中幾乎每個樣本點到邊緣(某個軸)的距離都會比自己到另一個樣本點的距離更短。假設有 $$n$$ 個樣本點，高維空間中兩點之距離的期望值 $$D$$：	
	$$D(d, n) = 1/2\left(\dfrac{1}{n}\right)^{1/d}$$
	{: style="text-align: center;"}

	假設$$2$$維空間中散佈著$$10000$$個點，則點之間距離的期望值$$D(2, 10000) = 0.005$$，到了$10$維空間則為$$D(10, 10000) = 0.4$$。請留意，到邊緣(軸)的最大距離發生在資料點distribution的中心，並且到每一邊(軸)的normalized距離是$$0.5$$。
	
4. **<font color='orange'>幾乎每個點都是outlier</font>**。隨著input資料的維度增加，預測出來的結果與自己對應的分類(或分群)中心的距離也會增加。例如，當 $$d = 10$$ 時，預測點到自己的對應的分類中心點距離為$$3.1$$個標準差，$$d = 20$$ 時，則為$$4.4$$個標準差。從這個角度來看，對每個新的預測點看起來像是一開始分好的群的outlier。Figure 2.2 就是在描述這個概念，預測點大多落在「刺蝟」的邊緣，就是遠離中心部分。

### 結論
當在高維空間中處理有限數量的樣本時，上述的「維數詛咒」特性通常會產生嚴重的後果。從特性(1)、(2)，我們看到了對高維樣本進行局部估計的困難；(隨著維度上升)我們需要越來越多樣本點，才有辦法滿足data mining所要求的資料密度。特性(3)、(4)指出了預測某個新input點會落在哪裡的難度，因為平均而言，因為平均而言，任何新預測結果會離群的邊緣比較近，且離中心部分的training example比較遠。
![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.4.png' | absolute_url }}){: .align-center height="80%" width="80%"}

<u>Figure 2.4.</u> 隨著維度增長，「距離」的意義也隨之改變。
{: style="text-align: center;"}

一群學生最近(2003年)做出了一個有趣的實驗，說明了理解維度詛咒的概念於data mining工作的重要性。他們為不同的$$n$$維空間隨機生成$$500$$個資料點，維度的數目$$n$$在$$2$$到$$50$$之間。然後，他們在每個空間中測量任兩個點之間的距離，併計算出參數 $$P$$：

$$P_n = \dfrac{ \text{MAX-DIST}_n - \text{MIN-DIST}_n}{\text{MIN-DIST}_n}$$
{: style="text-align: center;"}

其中 $$n$$ 是維度的數量，而 $$\text{MAX-DIST}$$ 和 $$\text{MIN-DIST}$$ 分別是給定空間中的最大距離和最小距離，結果如Figure 2.4所示。此圖有趣的是，在維度較高的情況下，參數$$P_n$$非常接近$$0$$。這意味著在高維空間中，最大和最小距離變得如此接近。換句話說，在這些高維空間中，任兩點之間的距離實際上並沒有沒有差異。這個實驗證明了，傳統上對data mining至關重要的「**<font color='orange'>距離</font>**」跟「**<font color='orange'>密度</font>**」其意義在高維空間中將發生改變！當data set的維度增加，資料將會快速地變稀疏，這表示這些資料點都會變成(高維)資料空間中的outlier，因此我們要重新檢視、評估距離(distance)、相似度(similarity)、分佈(data distrubution)、期望值(mean)、標準差(standard deviation)...等，是否仍適用傳統統計學上的定義。

### 來源
[Mehmed Kantardzic. 2003. Principles of Measurement Systems. 2nd ed., 36-38. Wiley-IEEE Press](https://books.google.com.tw/books?id=ivm4DwAAQBAJ&pg=PA36)

### 後記
韓家煒、Micheline Kamber、裴健合著的Jiawei H., Micheline K., Jian P. 1991. Data Mining: Concepts and Techniques
3rd ed. 有關維度詛咒的地方寫得不好，完全沒有點出精髓。

### 原文
Most data-mining problems arise because there are large amounts of samples with different types of features. Besides, these samples are very often high dimensional, which means they have extremely large number of measurable features. This additional dimension of large data sets causes the problem known in data-mining terminology as "the curse of dimensionality." The "curse of dimensionality" is produced because of the geometry of high-dimensional spaces, and these kinds of data spaces are typical for data-mining problems. The properties of high-dimensional spaces often appear counter-intuitive because our experience with the physical world is in a low-dimensional space, such as a space with two or three dimensions. Conceptually, objects in high-dimensional spaces have a larger surface area for a given volume than objects in low-dimensional spaces. For example, a high-dimensional hypercube, if it could be visualized, would look like a porcupine, as in Figure 2.2. 

![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.2.png' | absolute_url }}){: .align-center height="30%" width="30%"}

<u>Figure 2.2.</u> High-dimensional data looks conceptually like a porcupine.
{: style="text-align: center;"}

As the dimensionality grows larger, the edges grow longer relative to the size of the central part of the hypercube. Four important properties of high-dimensional data are often the guidelines in the interpretation of input data and data-mining results:
	
1. The size of a data set yielding the same density of data points in an $$n$$-dimensional space increases exponentially with dimensions. For example, if a one-dimensional ($$1$$D) sample containing $$n$$ data points has a satisfactory level of density, then to achieve the same density of points in $$k$$$ dimensions, we need $$n^k$$ data points. If integers $$1—100$$ are values of $$1$$D samples, where the domain of the dimension is $$[0, 100]$$, then to obtain the same density of samples in a five-dimensional space, we will need $$100^5 = 10^{10}$$ different samples. This is true even for the largest real-world data sets; because of their large dimensionality, the density of samples is still relatively low and, very often, unsatisfactory for data-mining purposes.

2. A larger radius is needed to enclose a fraction of the data points in a high-dimensional space. For a given fraction of samples, it is possible to determine the edge length e of the hypercube using the formula

	$$e(p)=p^{1/d}$$
	{: style="text-align: center;"}

	where $$p$$ is the prespecified fraction of samples and $$d$$ is the number of dimensions. For example, if one wishes to enclose $$10\%$$ of the samples ($$p = 0.1$$), the corresponding edge for a two-dimensional ($$2$$D) space will be $$e_2(0.1) = 0.32$$, for a three-dimensional space $$e_3(0.1) = 0.46$$, and for a 10-dimensional space $$e_{10}(0.1) = 0.8$$. Graphical interpretation of these edges is given in Figure 2.3. This shows that a very large neighborhood is required to capture even a small portion of the data in a high-dimensional space.

	![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.3.png' | absolute_url }}){: .align-center height="80%" width="80%"}

	<u>Figure 2.3.</u> Regions enclose $$10\%$$ of the samples for one-, two-, and three-dimensional space.
	{: style="text-align: center;"}


3. Almost every point is closer to an edge than to another sample point in a high-dimensional space. For a sample of size $$n$$, the expected distance $$D$$ between data points in a $$d$$-dimensional space is
	
	$$D(d, n) = 1/2\left(\dfrac{1}{n}\right)^{1/d}$$
	{: style="text-align: center;"}

	For example, for a $$2$$D space with $$10,000$$ points, the expected distance is $$D(2, 10000) = 0.005$$ and for a 10-dimensional space with the same number of sample points $$D(10, 10000) = 0.4$$. Keep in mind that the maximum distance from any point to the edge occurs at the center of the distribution and it is $$0.5$$ for normalized values of all dimensions.


4. Almost every point is an outlier. As the dimension of the input space increases, the distance between the prediction point and the center of the classified points increases. For example, when $$d = 10$$, the expected value of the prediction point is $$3.1$$ standard deviations away from the center of the data belonging to one class. When $$d = 20$$, the distance is $$4.4$$ standard deviations. From this standpoint, the prediction of every new point looks like an outlier of the initially classified data. This is illustrated conceptually in Figure 2.2, where predicted points are mostly in the edges of the porcupine, far from the central part.


These rules of the "curse of dimensionality" most often have serious consequences when dealing with a finite number of samples in a high-dimensional space. From properties (1) and (2), we see the difficulty in making local estimates for high-dimensional samples; we need more and more samples to establish the required data density for performing planned mining activities. Properties (3) and (4) indicate the difficulty of predicting a response at a given point, since any new point will on average be closer to an edge than to the training examples in the central part.

![image-center]({{ '/assets/images/2020-03-06-cursee-of-dimensionality/figure2.4.png' | absolute_url }}){: .align-center height="80%" width="80%"}

<u>Figure 2.4.</u> With large number of dimensions, the concept of a distance changes its meaning.
{: style="text-align: center;"}
	

One interesting experiment, performed recently by a group of students, shows the importance of understanding curse of dimensionality concepts for data-mining tasks. They generated randomly 500 points for different $$n$$-dimensional spaces. The number of dimensions was between 2 and 50. Then, they measured, in each space, all distances between any pair of points and calculated the parameter $$P$$:

$$P_n = \dfrac{ \text{MAX-DIST}_n - \text{MIN-DIST}_n}{\text{MIN-DIST}_n}$$
{: style="text-align: center;"}

where $$n$$ is the number of dimensions and $$\text{MAX-DIST}$$ and $$\text{MIN-DIST}$$ are maximum and minimum distances in the given space, respectively. The results are presented in Figure 2.4. What is interesting from the graph is that with high number of dimensions, parameter $$P_n$$ is reaching the value of $$0$$. That means maximum and minimum distances becoming so close in these spaces; in other words, there are no differences in distances between any two points in these large-dimensional spaces. It is an experimental confirmation that traditional definitions of density and distance between points, which are critical for many data-mining tasks, change its meaning! When dimensionality of a data set increases, data becomes increasingly sparse with mostly outliers in the space that it occupies. Therefore we have to revisit and reevaluate traditional concepts from statistics: distance, similarity, data distribution, mean, standard deviation, etc.






