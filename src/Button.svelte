<script>
	let button;

	function click(event) {
		let ripple = document.createElement('div');
		ripple.classList.add('ripple');
		let buttonBound = button.getBoundingClientRect();
		ripple.style.left = `${event.clientX - buttonBound.left}px`;
		ripple.style.top = `${event.clientY - buttonBound.top}px`;

		const rippleAnimation = [
			{ opacity: 1, width: '0px', height: '0px'},
			{ opacity: 1},
			{},
			{ opacity: 0, width: '800px', height: '800px',
			left: '-350px', top: '-350px'}
		]
		button.appendChild(ripple);

		ripple.animate(rippleAnimation, 500);

		setTimeout(() => {
			button.removeChild(ripple)
		}, 500);

	}
</script>


<button bind:this={button} on:mousedown={click} on:click>
	<slot></slot>
</button>

<style>
	slot {
		-webkit-user-select: none;
	}
	button {
		z-index: 2;
		user-select: none;
		position: relative;
		cursor: pointer;
		border: 2px solid #424242;
		padding: 30px;
		padding-left: 60px;
		padding-right: 60px;
		text-transform: uppercase;
		transition: all 100ms ease;
		font-size: 1.8rem;
		border-radius: 0;
		background-color: transparent;
		overflow: hidden;
		color: white;
		-webkit-user-select: none;
	}
	button:active {
		background-color: transparent;
	}
</style>
