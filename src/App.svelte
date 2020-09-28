<script>

	import Button from './Button.svelte';
	import Centre from './Centre.svelte';
	import IconButton from './IconButton.svelte';

	let text;
	let text_elem;
	let isFullscreen = document.fullscreenElement ? true : false;

	let lines;

	let file_opened = false;
	let fontSize = 10;


	function openFile() {
		const input = document.createElement('input');
		input.setAttribute('type', 'file');
		input.setAttribute('accept', '.txt');
		input.onchange = async () => {
			let file = input.files[0];

			let fr = new FileReader();
			fr.onload = async () => {
				file_opened = true;

				text = fr.result;

			    setTimeout(() => {


				let textBound = text_elem.getBoundingClientRect();

				if (textBound.height > textBound.width) {
					resizeHeight()
				} else {
					resizeWidth()
				}
			}, 300);
			}

			fr.readAsText(file)
		}
		setTimeout(() => {
			input.click();
		}, 200);
	}

	function resizeHeight() {
		let textBound= text_elem.getBoundingClientRect();
		fontSize *= 0.7;
		text_elem.style.fontSize = `${fontSize}px`;
		textBound = text_elem.getBoundingClientRect();
		if (textBound.height > window.innerHeight - window.innerHeight * 0.02) {
			requestAnimationFrame(resizeHeight);
		}
	}

	function resizeWidth() {
		let textBound = text_elem.getBoundingClientRect();
		fontSize *= 0.7;
		text_elem.style.fontSize = `${fontSize}px`;
		textBound = text_elem.getBoundingClientRect();
		if (textBound.width > window.innerWidth - window.innerWidth * 0.02) {
			requestAnimationFrame(resizeWidth)
		}
	}

	function toggleFullscreen() {
		setTimeout(() => {
			if (isFullscreen) {
				document.exitFullscreen();
			} else {
				document.body.requestFullscreen();
			}
			isFullscreen = !isFullscreen;
		}, 300);
	}


</script>

<div class='wrapper'>
	{#if !file_opened}
		<Centre>
			<Button on:click={openFile}>Open</Button>
		</Centre>
	{:else}
		<Centre>
			<code><pre id='lines' bind:this={text_elem}>{text}</pre></code>
		</Centre>
	{/if}

	{#if isFullscreen}
		<div class='fullscreen'>
			<IconButton src='./fullscreen_exit.svg' on:click={toggleFullscreen} alt=''/>
		</div>
	{:else}
	    <div class='fullscreen'>
			<IconButton src='./fullscreen.svg' on:click={toggleFullscreen} alt=''/>
		</div>
	{/if}
</div>

<style>
	:global(.ripple) {
		position: absolute;
		width: 0px;
		height: 0px;
		border-radius: 50%;
		background-color: #74747480;
		transition: 200ms ease cubic-bezier(0.4, 0, 0.2, 1);
		z-index: 0;
	}
	pre {
		font-size: 10px;
		color: rgb(245, 245, 245);
		width: fit-content;
		height: fit-content;
	    font-family: 'Ubuntu Mono', monospace;
		overflow: visible;
	}
	.wrapper {
		background-color: #212121;
		margin: none;
		padding: none;
		border: none;
		outline: none;
		width: 100%;
		height: 100%;
		z-index: 0;
	}
	:global(body) {
		background-color: #212121;
		margin: none;
		padding: none;
		border: none;
		outline: none;
	}
	.fullscreen {
		position: absolute;
		top: 1px;
		left: 1px;
		margin: none;
		padding: none;
		border: none;
		outline: none;
		width: fit-content;
		height: fit-content;
	}
	#lines {
		display: flex;
		align-items: start;
		justify-content: start;
		flex-direction: column;
	}
</style>
