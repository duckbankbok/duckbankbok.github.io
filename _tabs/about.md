---
title: About
icon: fas fa-info-circle
order: 4
---
<style>
@import url('https://fonts.googleapis.com/css2?family=Merriweather&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Merriweather&family=Nanum+Gothic:wght@700&display=swap');
.zoom a img {
	-webkit-transform: scale(1);
	transform: scale(1);
	-webkit-transition: .3s ease-in-out;
	transition: .3s ease-in-out;
}
.zoom a img:hover {
	-webkit-transform: scale(1.3);
	transform: scale(1.3);
}
.zoom-container {
	background: rgb(0,27,84);
	-webkit-transition: .3s ease-in-out;
    transition: .3s ease-in-out;
}
.zoom-container:hover {
	background: rgb(68,193,196);
	-webkit-transition: .3s ease-in-out;
    transition: .3s ease-in-out;
}

/* 리셋 CSS */
* {margin:0;padding:0;box-sizing:border-box;}
ul, li {list-style:none;}

.slidebox {max-width:700px;margin:0 auto;position:relative;}
.slidebox .slidelist {position:relative;white-space:nowrap;font-size:0;overflow:hidden;}
.slidebox .slidelist .slideitem {position:relative;display:inline-block;vertical-align:top;width:100%;transition:all 1s;}
.slidebox .slidelist .slideitem > a {display:block;width:auto;position:relative;}
.slidebox .slidelist .slideitem > a img {max-width:100%;}
.slidebox .slidelist .textbox {position:relative;z-index:1;left:50%;transform:translate(-50%,-50%);line-height:1.6;text-align:center;}
.slidebox .slidelist .textbox h6 {font-size:1rem;color:#000000;transform:translateY(30px);transition:all .5s;}

.slidebox .slide-control [class*="control"] label {position:absolute;z-index:10;top:55%;transform:translateY(-50%);padding:20px;border-radius:50%;cursor:pointer;}
.slidebox .slide-control [class*="control"] label.prev {left:20px;background:#333 url('../assets/img/button/left-arrow.png') center center / 50% no-repeat;}
.slidebox .slide-control [class*="control"] label.next {right:20px;background:#333 url('../assets/img/button/right-arrow.png') center center / 50% no-repeat;}

[name="slide"] {display:none;}
#slide01:checked ~ .slidelist .slideitem {left:0;}
#slide02:checked ~ .slidelist .slideitem {left:-100%;}

/* input에 체크되면 텍스트 효과 */
input[id="slide01"]:checked ~ .slidebox li:nth-child(1) .textbox h6 {opacity:1;transform:translateY(0);transition-delay:.2s;}
input[id="slide02"]:checked ~ .slidebox li:nth-child(2) .textbox h6 {opacity:1;transform:translateY(0);transition-delay:.2s;}

.slide-control [class*="control"] {display:none;}
#slide01:checked ~ .slide-control .control01 {display:block;}
#slide02:checked ~ .slide-control .control02 {display:block;}
</style>

<div class="container" style="text-align: center;">
	<div class="zoom-container" style="display: inline-block; position: relative; width: 206px; height: 206px; border-radius: 50%;">
		<div class="zoom" style="display: inline-block; position: relative; width: 200px; height: 200px; overflow: hidden; border-radius: 50%; margin-top: 3px;">
			<a href="https://github.com/duckbankbok" target="_blank"><img src="../assets/img/profile.jpg" style="display: block; width: auto; height: 100%; margin-top: auto; margin-bottom: 0;" /></a>
		</div>
	</div>
    <div class="divider"></div>
    <div style="display: inline-block; background-color: rgb(68,193,196); height: 1px; width: 160px;"></div>
    <h5 style="margin-top: 0; margin-bottom: 0.5rem; font-family: 'Merriweather', 'Nanum Gothic', serif;" >복영규 (Younggyu Bok)</h5>
    <h6 style="mmargin: 0;">Master's student in UNIST (Department of Industrial Engineering)</h6>
</div>

##### Research interests

Big data, Optimization, Data mining

##### Education

`2016-2022, UNIST`
Bachelor of Science (Management Engineering)  
`2022-Now, UNIST`
Master's Student (Industrial Engineering)

##### Web Project

<div class="slidebox" style="justify-content: center;">
	<input type="radio" name="slide" id="slide01" checked>
	<input type="radio" name="slide" id="slide02">
	<ul class="slidelist">
		<li class="slideitem">
			<h6 class="textbox" style="font-family: 'Merriweather'; margin: 0.5rem 0 0 0;">Dog classifier</h6>
			<a href="https://duckbankbok.github.io/dog-classifier/">
				<img src="../assets/img/projects/websites/dog_classifier.png" style="margin: 0;">
			</a>
		</li>
		<li class="slideitem">
			<h6 class="textbox" style="font-family: 'Merriweather'; margin: 0.5rem 0 0 0;">Lottery</h6>
			<a href="https://github.com/duckbankbok/lottery">
				<img src="../assets/img/projects/websites/lottery.png" style="margin: 0;">
			</a>
		</li>
	</ul>
	<div class="slide-control">
		<div class="control01">
			<label for="slide02" class="prev"></label>
			<label for="slide02" class="next"></label>
		</div>
		<div class="control02">
			<label for="slide01" class="prev"></label>
			<label for="slide01" class="next"></label>
		</div>
	</div>
</div>