<script>
    export let src;
    export let alt;


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
            { opacity: 0, width: '200px', height: '200px',
            left: '-50px', top: '-50px'}
        ]
        button.appendChild(ripple);

        ripple.animate(rippleAnimation, 200);

        setTimeout(function(){
            if (button) {
              button.removeChild(ripple);
            }
        }, 300);

    }

</script>

<button on:mousedown={click} bind:this={button} on:click>
    <img src={src} alt={alt}>
</button>



<style>
	img {
		width: 50px;
		height: 50px;
        z-index: 1;
        user-select: none;
        -webkit-user-drag: none;
		-webkit-user-select: none;
	}
	button {
    z-index: 2;
        margin: none;
		padding: none;
		border: none;
		outline: none;
        user-select: none;
        position: absolute;
		width: 50px;
		height: 50px;
		border: none;
		cursor: pointer;
		box-shadow: 0px 2px 3px #55555580;
		padding: 10px;
		text-transform: uppercase;
		transition: all 100ms ease;
		border-radius: 50%;
		background-color: #fff;
		display: flex;
		align-items: center;
		justify-content: center;
        overflow: hidden;
		-webkit-user-select: none;

	}
	button:focus {
        background-color: #ddd;
	}
	button:hover {
		box-shadow: 0px 4px 3px #55555580;
	}
</style>
