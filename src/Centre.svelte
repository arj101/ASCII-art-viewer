
<div>
    <slot></slot>
</div>
<style>
    div {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        margin: none;
        border: none;
        padding: none;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 0;
    }
    slot {
      z-index: 2;
    }
</style>
