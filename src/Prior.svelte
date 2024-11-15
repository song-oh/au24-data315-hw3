<script>
    import { onMount, createEventDispatcher } from "svelte";
    import noUiSlider from "nouislider";
    import "nouislider/dist/nouislider.css";

    let minValue = 0;
    let maxValue = 10;
    let sliderElement;

    const dispatch = createEventDispatcher();

    onMount(() => {
        noUiSlider.create(sliderElement, {
            start: [minValue, maxValue],
            connect: true,
            range: {
                min: 0,
                max: 10,
            },
            step: 0.1,
            tooltips: [true, true],
            format: {
                to: (value) => value.toFixed(1),
                from: (value) => parseFloat(value),
            },
        });

        sliderElement.noUiSlider.on("update", (values) => {
            minValue = parseFloat(values[0]);
            maxValue = parseFloat(values[1]);
            dispatch("update", { minValue, maxValue });
        });
    });
</script>

<div class="slider-container">
    <div bind:this={sliderElement} id="interval-slider"></div>
    <div class="value-display">
        Your prior belief is: {minValue} - {maxValue} kg
    </div>
</div>

<style>
    .slider-container {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    #interval-slider {
        width: 80%;
        margin-top: 50px;
    }

    .value-display {
        margin-top: 10px;
        font-size: 1.2rem;
        color: #333;
    }
</style>
