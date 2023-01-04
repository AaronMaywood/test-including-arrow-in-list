# 矢印の装飾を伴うリストのコーディング例

次のURLで示すリストを作成したい。
https://www.figma.com/file/R7dmUQoY7bGUfRIxsRC463/Responsive-Web-Page-%F0%9F%8E%89-(Community)?node-id=1%3A2&t=raCZMYYPlhNWrS0I-1

https://github.com/AaronMaywood/Website3/commit/2be65d64df898e9f5bdce6d43ea66e87ad2d0bea
では、次のようにli要素として矢印の装飾を行ったが、矢印は本来はliではない。
https://github.com/AaronMaywood/Website3/blob/2be65d64df898e9f5bdce6d43ea66e87ad2d0bea/index.html#L99-L111
```
<ul>
	<!-- caret（項目と項目の間の矢印） はリストの一部ではなく装飾であるため、li要素にしない方法があればなお良い -->
	<li>ソフトウェア品質向上</li>
	<li class="caret when-desktop"><img src="images/CaretCircleDoubleRight.svg" width="32" height="32" alt=""></li>
	<li class="caret when-mobile"><img src="images/CaretCircleDoubleRight_vert.svg" width="32" height="32" alt=""></li>
	<li>起こりうるエラーを予測</li>
	<li class="caret when-desktop"><img src="images/CaretCircleDoubleRight.svg" width="32" height="32" alt=""></li>
	<li class="caret when-mobile"><img src="images/CaretCircleDoubleRight_vert.svg" width="32" height="32" alt=""></li>
	<li>起こりうるリスクを防止</li>
	<li class="caret when-desktop"><img src="images/CaretCircleDoubleRight.svg" width="32" height="32" alt=""></li>
	<li class="caret when-mobile"><img src="images/CaretCircleDoubleRight_vert.svg" width="32" height="32" alt=""></li>
	<li>時間とコストを節約</li>
</ul>
```

HTMLのマークアップではliとせずに、figmaのデザインカンプのスタイルを保ったまま、リストの項目間に矢印を配置する方法はあるだろうか？


## リストのlist-styleとして矢印を挿入できるか？→矢印画像をlist-styleにはできるが、スタイルが異なってしまう。

```
<style>
body{
	background: black;
}

ul{
	list-style: url('images/CaretCircleDoubleRight.svg');
	color: white;
	font-size: 40px;
	display: flex;
	gap: 4rem;
	justify-content: center;
}

li::marker{
	/*
	::markerで使用できるプロパティはこちら
	https://developer.mozilla.org/ja/docs/Web/CSS/::marker
	*/
	width: 16px;		/* ここではwidth,heightは使用できない */
	font-size: 16px;	/* svg画像の大きさをコントロールするにはwidth,heightの代わりにfont-sizeを利用する */
	line-height: 1.5;
}

/* 最初の項目だけlist-styleなし */
li:first-child::marker{
	content: "";
	list-style:none;	/* これは無効、この代わりにcontent: ""; を使用する */
}
</style>
<ul>
	<li>起こりうるエラーを予測</li>
	<li>起こりうるリスクを防止</li>
	<li>時間とコストを節約</li>
</ul>
```

### ::beforeや::afterが使えないか？→スタイルが異なってしまう

```
body{
}
.contents-2nd ul {
	color: white;
	font-size: 40px;
	display: flex;
	gap: 4rem;
	justify-content: center;
}

.contents-2nd ul li::before {
	content: "";
	display: inline-block; /* width,heightが利用できるようにする */
	width: 1.5rem;
	height: 1.5rem;
/*
	vertical-align:bottom;
*/
	background: #F84646 url('../images/CaretCircleDoubleRight.svg');
	background-size: 100%;
	background-repeat: no-repeat;
}

.contents-2nd ul li::marker{
/*
背景で代用する方法
https://qumeru.com/magazine/134
https://stackoverflow.com/questions/7775594/css-list-style-image-size
*/
	font-size: 10px;	/* svg画像の大きさをコントロールするにはwidth,heightの代わりにfont-sizeを利用する→万能ではなく、思い通りにならない */
}

/* 最初の項目だけlist-styleなし */
.contents-2nd ul li:first-child::marker{
	content: "";
	list-style:none;	/* これは無効、この代わりにcontent: ""; を使用する */
}

/***********************************/

.contents-2nd ul li::after {
/*
display: list-item;    https://developer.mozilla.org/ja/docs/Web/CSS/display
この方法は、::afterがliの子要素となるために有効でない

liの兄弟を追加する疑似要素はないか？
https://developer.mozilla.org/ja/docs/Web/CSS/Pseudo-elements
→なし！
*/
}
```

## 結論

いまのところ、次のように矢印もli要素にする方法しか思いついていない。
https://github.com/AaronMaywood/Website3/blob/2be65d64df898e9f5bdce6d43ea66e87ad2d0bea/index.html#L99-L111

# ロゴのクラウドレイアウトを適切にコーディングしたい

ロゴのクラウドとはこれのこと
https://www.figma.com/file/R7dmUQoY7bGUfRIxsRC463/Responsive-Web-Page-%F0%9F%8E%89-(Community)?node-id=1%3A2&t=raCZMYYPlhNWrS0I-1

## 最初の実装 - flexbox を使用

```
<ul>
	<li><img src="images/image 6.png" width="134" height="67" alt="BORUSAN"></li>
	<li class="ul-wrapper">
		<ul>
			<li><img src="images/image 5.png" width="122" height="61" alt="Istanbul Bilgi Universitesi"></li>
			<li><img src="images/Group.svg" width="123" height="37" alt="book my Show"></li>
		</ul>
	</li>
	<li class="ul-wrapper">
		<ul>
			<li><img src="images/image 2.png" width="132" height="66" alt="AKBANK"></li>
			<li><img src="images/image 3.png" width="132" height="66" alt="AKCANSA"></li>
			<li><img src="images/Frame 4.svg" width="167" height="52" alt="Tumunu Gor"></li>
		</ul>
	</li>
	<li class="ul-wrapper">
		<ul>
			<li><img src="images/image 4.png" width="118" height="59" alt="aktas"></li>
			<li><img src="images/OLA logo.svg" width="98" height="34" alt="OLA"></li>
		</ul>
	</li>
	<li><img src="images/Amazon Logo.svg" width="103" height="32" alt="Amazon"></li>
</ul>
CSSは以下の通り
.contents-5th > ul {
	margin-top: 48px;
	display: flex;
	gap: 24px;
	align-items: center;
	justify-content: center;
}

.contents-5th > ul > li.ul-wrapper {
	background: none;
	height: fit-content;
}

.contents-5th > ul > li > ul {
	display: flex;
	flex-direction: column;
	gap: 24px;
}
```

見た目だけであればこれでもよいが、次にセマンティクスを改良した版を示す

## 改良版 - grid を使用すれば一階層のリストにできる

flexboxを使用した実装では、ulの階層ではリストの意味が変わる（階層付きリストになる）ため、gridを使用し、ulが１階層になるようにした

https://github.com/AaronMaywood/Website3/blob/2be65d64df898e9f5bdce6d43ea66e87ad2d0bea/index.html#L134-L144
```
<ul>
	<li class="borusan"><img src="images/image 6.png" width="134" height="67" alt="BORUSAN"></li>
	<li class="istanbul"><img src="images/image 5.png" width="122" height="61" alt="Istanbul Bilgi Universitesi"></li>
	<li class="bookmy"><img src="images/Group.svg" width="123" height="37" alt="book my Show"></li>
	<li class="akbank"><img src="images/image 2.png" width="132" height="66" alt="AKBANK"></li>
	<li class="akcansa"><img src="images/image 3.png" width="132" height="66" alt="AKCANSA"></li>
	<li class="tumunu"><img src="images/Frame 4.svg" width="167" height="52" alt="Tumunu Gor"></li>
	<li class="aktas"><img src="images/image 4.png" width="118" height="59" alt="aktas"></li>
	<li class="ola"><img src="images/OLA logo.svg" width="98" height="34" alt="OLA"></li>
	<li class="amazon"><img src="images/Amazon Logo.svg" width="103" height="32" alt="Amazon"></li>
</ul>
```

https://github.com/AaronMaywood/Website3/blob/2be65d64df898e9f5bdce6d43ea66e87ad2d0bea/css/style.css#L287-L310
```
.contents-5th ul {
	margin-top: 48px;
	display: grid;
	grid-template-areas:
		".       .        akbank  .     .   " 
		".       istanbul akbank  aktas .   " 
		"borusan istanbul akcansa aktas amazon" 
		"borusan bookmy   akcansa ola   amazon" 
		".       bookmy   tumunu  ola   .   " 
		".       .        tumunu  .     .   ";
	grid-template-columns: 170px 170px 170px 170px 170px;
	gap: 24px;
	justify-content: center;
}

.contents-5th ul .borusan { grid-area: borusan; }
.contents-5th ul .istanbul { grid-area: istanbul; }
.contents-5th ul .bookmy { grid-area: bookmy; }
.contents-5th ul .akbank { grid-area: akbank; }
.contents-5th ul .akcansa { grid-area: akcansa; }
.contents-5th ul .tumunu { grid-area: tumunu; }
.contents-5th ul .aktas { grid-area: aktas; }
.contents-5th ul .ola { grid-area: ola; }
.contents-5th ul .amazon { grid-area: amazon; }
```
