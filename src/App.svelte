<script>

	let clickDuration = 0;
	let notClickDuration = 0;
	let measureClickDuration;
	let measureNotClickDuration;
	let inputs = [];
	let contents = [];
	let language = "English";
	let correction = 1;

	/**
	 * object나 array의 동등비교 해주는 함수
	 * @returns boolean값
	*/
	function equals(a,b) {
		return JSON.stringify(a) === JSON.stringify(b)
	}

	/**
	 * inputs의 모스 부호를 문자로 바꿔주는 함수
	*/
	function translate() {
		if (language == "English") {
			if (equals(inputs,[0,1])) {contents.push("A")}
				else if (equals(inputs,[1,0,0,0])) {contents.push("B")}
				else if (equals(inputs,[1,0,1,0])) {contents.push("C")}
				else if (equals(inputs,[1,0,0])) {contents.push("D")}
				else if (equals(inputs,[0])) {contents.push("E")}
				else if (equals(inputs,[0,0,1,0])) {contents.push("F")}
				else if (equals(inputs,[1,1,0])) {contents.push("G")}
				else if (equals(inputs,[0,0,0,0])) {contents.push("H")}
				else if (equals(inputs,[0,0])) {contents.push("I")}
				else if (equals(inputs,[0,1,1,1])) {contents.push("J")}
				else if (equals(inputs,[1,0,1])) {contents.push("K")}
				else if (equals(inputs,[0,1,0,0])) {contents.push("L")}
				else if (equals(inputs,[1,1])) {contents.push("M")}
				else if (equals(inputs,[1,0])) {contents.push("N")}
				else if (equals(inputs,[1,1,1])) {contents.push("O")}
				else if (equals(inputs,[0,1,1,0])) {contents.push("P")}
				else if (equals(inputs,[1,1,0,1])) {contents.push("Q")}
				else if (equals(inputs,[0,1,0])) {contents.push("R")}
				else if (equals(inputs,[0,0,0])) {contents.push("S")}
				else if (equals(inputs,[1])) {contents.push("T")}
				else if (equals(inputs,[0,0,1])) {contents.push("U")}
				else if (equals(inputs,[0,0,0,1])) {contents.push("V")}
				else if (equals(inputs,[0,1,1])) {contents.push("W")}
				else if (equals(inputs,[1,0,0,1])) {contents.push("X")}
				else if (equals(inputs,[1,0,1,1])) {contents.push("Y")}
				else if (equals(inputs,[1,1,0,0])) {contents.push("Z")}
				else {contents.push("?")}		
		}
	}

	/**
	 * 버튼을 클릭했을 때 실행되는 함수
	 */
	function downBtn() {
		clickDuration = 0;
		measureClickDuration = setInterval(() => {clickDuration++}, 10);
		clearInterval(measureNotClickDuration);

		if (notClickDuration >= 15 * correction) {
			translate();
			inputs = [];
			contents = contents;
		}
		document.getElementById("button").style.backgroundColor = "grey";
	}

	/**
	 * 버튼을 뗐을 때 실행되는 함수
	 */
	function upBtn() {
		notClickDuration = 0;
		clearInterval(measureClickDuration);
		measureNotClickDuration = setInterval(() => {notClickDuration++}, 10);

		if (clickDuration <= 10 * correction) {inputs.push(0)}

		if (11 * correction <= clickDuration) {inputs.push(1)}

		inputs = inputs;

		document.getElementById("button").style.backgroundColor = "white";
	}

	setInterval(() => {
		if (equals(inputs,[]) == false && notClickDuration >= 60 * correction) {
				translate();
				inputs = [];
				contents = contents;
				contents.push(" ");
				clearInterval(measureNotClickDuration);
				notClickDuration = 0;
			}
	}, 10)

	//키보드 입력 감지

	window.addEventListener("keydown", (e) => {
		if (e.key == " ") {
			downBtn();
		}
	})
	window.addEventListener("keyup", (e) => {
		if (e.key == " ") {
			upBtn();
		}
	})

</script>

<main>

	<head>
		<link rel="preconnect" href="https://fonts.googleapis.com">
		<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
		<link href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@100;300;400;500;700;800;900&display=swap" rel="stylesheet">
	</head>

	<h1 class="title">Morse Practice</h1>

	<div on:mousedown={downBtn} on:mouseup={upBtn} on:touchstart={downBtn} on:touchend={upBtn} class="button" id="button">
		<h1 class="clickText">Click!</h1>
	</div>
	<div class="letters">
		<div class="textTextBox">
			<p class="textText">TEXT</p>
		</div>
		{#each contents as letter}
			{#if letter != " "}
				<div class="letter">
					<p>{letter}</p>
				</div>
			{/if}
			{#if letter == " "}
			<div class= blank></div>
			{/if}
		{/each}
	</div>

	<div class="debug">
		<h3>디버그</h3>
		<p>내용: {contents}</p>
		<p>언어: {language}</p>
		<p>버튼 누른 시간: {clickDuration}</p>
		<p>버튼 안 누른 시간: {notClickDuration}</p>
		<p>입력: {inputs}</p>
	</div>

</main>

<style>
	main{
		text-align: center;
	}
	.title{
		font-size: 50px;
		font-family: "M PLUS Rounded 1c", sans-serif;
  		font-weight: 800;
  		font-style: normal;
	}
	.debug{
		background-color: aqua;
		display: solid;
	}
	.letter{
		font-size: 30px;
		height: 30px;
		transform: translate(0,-100%);
		font-family: "M PLUS Rounded 1c", sans-serif;
  		font-weight: 400;
  		font-style: normal;
	}
	.letters{
		display: flex;
		flex-wrap: wrap;
		width: 100% - 10px;
		background-color: rgb(165, 255, 171);
		padding-top: 30px;
		padding-bottom: 20px;
		padding-left: 10px;
		margin-top: 50px;
		margin-left: 50px;
		margin-right: 50px;
		min-height: 150px;
		border: solid;
		border-radius: 3px;
		border-color: black;
		position: relative;
	}
	.blank{
		width: 10px;
		height: 3px;
	}
	.button {
		position: relative;
		background-image: linear-gradient(to right bottom, rgb(255, 145, 255, 80%), rgb(106, 186, 255, 80%));
		width: 300px;
		height: 60px;
		text-align: center;
		color: white;
		left: 50%;
		transform: translate(-50%,0);
		border-radius: 3px;
		border: solid;
		border-color: black;
		margin-top: 50px;
		user-select: none;
		cursor: pointer;
		background-color: white;
	}
	.clickText{
		position: relative;
		top: 55%;
		transform: translate(0,-100%);
		font-size: 35px;
		font-family: "M PLUS Rounded 1c", sans-serif;
  		font-weight: 600;
  		font-style: normal;
	}
	.textTextBox{
		position: absolute;
		bottom: 175px;
		background-color: rgb(163, 255, 255);
		height: 50px;
		width: 150px;
		border: solid;
		border-radius: 3px;
		border-color: black;
	}
	.textText{
		position: relative;
		top: 30%;
		transform: translate(0,-100%);
		font-size: 40px;
		font-family: "M PLUS Rounded 1c", sans-serif;
  		font-weight: 600;
  		font-style: normal;
	}

</style>