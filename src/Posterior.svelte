<script>
    import { onMount } from "svelte";

    export let minValue = 0;
    export let maxValue = 10;

    let dataMean = 7;
    let dataStdDev = 1.5;
    let posterior = [];

    function prior(x) {
        const priorMean = (minValue + maxValue) / 2;
        const priorStdDev = (maxValue - minValue) / 4;
        const exponent = -((x - priorMean) ** 2) / (2 * priorStdDev ** 2);
        return (
            (1 / (priorStdDev * Math.sqrt(2 * Math.PI))) * Math.exp(exponent)
        );
    }

    function likelihood(x, mean, stddev) {
        const exponent = -((x - mean) ** 2) / (2 * stddev ** 2);
        return (1 / (stddev * Math.sqrt(2 * Math.PI))) * Math.exp(exponent);
    }

    function calculatePosterior() {
        const posteriorUnnormalized = [];
        let totalSum = 0;

        for (let x = minValue; x <= maxValue; x += 0.1) {
            const priorValue = prior(x);
            const likelihoodValue = likelihood(x, dataMean, dataStdDev);
            posteriorUnnormalized.push(priorValue * likelihoodValue);
            totalSum += priorValue * likelihoodValue * 0.1;
        }

        posterior = posteriorUnnormalized.map((value) => value / totalSum);
    }

    function updatePosterior() {
        calculatePosterior();
    }

    onMount(() => {
        calculatePosterior();
    });
</script>

<main>
    <div>
        <label>
            Mean of Observed Data:
            <input
                type="range"
                min="0"
                max="10"
                step="0.1"
                bind:value={dataMean}
                on:input={updatePosterior}
            />
            <span>{dataMean.toFixed(1)}</span>
        </label>
        <label>
            Standard Deviation of Observed Data:
            <input
                type="range"
                min="0.5"
                max="3"
                step="0.1"
                bind:value={dataStdDev}
                on:input={updatePosterior}
            />
            <span>{dataStdDev.toFixed(1)}</span>
        </label>
    </div>

    <svg width="600" height="500" viewBox="0 0 600 500">
        <g font-size="14" text-anchor="start">
            <rect
                x="20"
                y="20"
                width="15"
                height="15"
                fill="steelblue"
                fill-opacity="0.5"
            />
            <text x="40" y="32">Prior Distribution</text>
            <rect
                x="20"
                y="45"
                width="15"
                height="15"
                fill="slategrey"
                fill-opacity="0.5"
            />
            <text x="40" y="57">Observed Data (Likelihood)</text>
            <rect
                x="20"
                y="70"
                width="15"
                height="15"
                fill="orange"
                fill-opacity="0.5"
            />
            <text x="40" y="82">Posterior Distribution</text>
        </g>

        <g>
            {#each Array.from({ length: 101 }, (_, i) => i * 0.1) as x, index}
                <rect
                    x={index * 6}
                    y={450 - prior(x) * 600}
                    width="6"
                    height={prior(x) * 600}
                    fill="steelblue"
                    fill-opacity="0.5"
                    role="presentation"
                />
            {/each}

            {#each Array.from({ length: 101 }, (_, i) => i * 0.1) as x, index}
                <rect
                    x={index * 6}
                    y={450 - likelihood(x, dataMean, dataStdDev) * 600}
                    width="6"
                    height={likelihood(x, dataMean, dataStdDev) * 600}
                    fill="slategrey"
                    fill-opacity="0.5"
                    role="presentation"
                />
            {/each}

            {#each Array.from({ length: 101 }, (_, i) => i * 0.1) as x, index}
                <rect
                    x={index * 6}
                    y={450 - posterior[index] * 600}
                    width="6"
                    height={posterior[index] * 600}
                    fill="orange"
                    fill-opacity="0.5"
                    role="presentation"
                />
            {/each}
        </g>

        {#each Array.from({ length: 11 }, (_, i) => i) as tickIndex}
            <text
                x={tickIndex * 58 + 10}
                y="465"
                text-anchor="middle"
                font-size="12"
            >
                {tickIndex}
            </text>
        {/each}

        <text x="300" y="490" text-anchor="middle">Weight</text>
        <text
            x="20"
            y="250"
            text-anchor="middle"
            transform="rotate(-90 20,250)"
        >
            Probability Density
        </text>
    </svg>
</main>

<style>
    main {
        font-family: sans-serif;
        margin: 20px;
        text-align: center;
        color: #555;
    }

    svg {
        margin: 20px 0;
    }

    label {
        display: inline-block;
        margin: 10px;
        font-size: 14px;
    }
</style>
