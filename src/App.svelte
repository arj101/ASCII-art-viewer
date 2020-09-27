<script>

	import Button from './Button.svelte';
	import Centre from './Centre.svelte';
	import IconButton from './IconButton.svelte';

	let text;
	let text_elem;
	let isFullscreen = document.fullscreenElement ? true : false;

	let file_opened = false;

	function openFile() {
		const input = document.createElement('input');
		input.setAttribute('type', 'file');
		input.setAttribute('accept', '.txt');
		input.onchange = async () => {
			let file = input.files[0];

			let fr = new FileReader();
			fr.onload = () => {
				file_opened = true;
				text = fr.result;

				let fontSize = 10;

			    setTimeout(() => {
				let textBound = text_elem.getBoundingClientRect();

				console.log(textBound)
				console.log(text_elem.style)

				if (textBound.height > textBound.width) {
					while (textBound.height > window.innerHeight - window.innerHeight * 0.01) {
						fontSize *= 0.8;
						text_elem.style.fontSize = `${fontSize}px`;
				        textBound = text_elem.getBoundingClientRect();
					}
				} else {
					while (textBound.width > window.innerWidth - window.innerWidth * 0.01) {
						fontSize *= 0.8;
						text_elem.style.fontSize = `${fontSize}px`;
				        textBound = text_elem.getBoundingClientRect();
					}
				}
			}, 500);
			}

			fr.readAsText(file)
		}
		setTimeout(() => {
			input.click();
		}, 200);
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
			<pre bind:this={text_elem}>{text}</pre>
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
		color: white;
		width: fit-content;
		height: fit-content;
		font-family: 'Fira Code';
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
</style>
