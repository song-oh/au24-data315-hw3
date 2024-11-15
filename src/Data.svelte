<script>
    import * as d3 from "d3";
    import { onMount } from "svelte";

    const margin = { top: 0, right: 50, bottom: 40, left: 50 };
    const width = 500;
    const height = 300;
    const chartW = width - margin.left - margin.right;
    const chartH = height - margin.top - margin.bottom;

    let circlesData = [];
    let interval;
    let animationRunning = false;
    let message = "";
    let xScale;
    let xAxis;
    let yScale;
    let yAxis;
    let marks;
    let bins;

    onMount(() => {
        initialRender();
    });

    function initialRender() {
        xScale = d3.scaleLinear().range([0, chartW]).domain([0, 10]);
        d3.select(xAxis)
            .call(d3.axisBottom(xScale).ticks(10).tickFormat(d3.format(".0f")))
            .selectAll(".tick text")
            .style("fill", "black");

        d3.select(xAxis)
            .append("text")
            .style("font-size", "14px")
            .style("fill", "black")
            .style("transform", `translate(${chartW / 2}px, 35px)`)
            .text("Bins");

        yScale = d3.scaleLinear().range([chartH, 0]).domain([0, 22]);
        d3.select(yAxis)
            .call(d3.axisLeft(yScale).ticks(6).tickFormat(d3.format(".0f")))
            .selectAll(".tick text")
            .style("fill", "black");

        d3.select(yAxis)
            .append("text")
            .style("font-size", "14px")
            .style("fill", "black")
            .style(
                "transform",
                `translate(${-margin.left / 2}px, ${chartH / 2}px) rotate(-90deg)`,
            )
            .text("Count");

        bins = Array.from({ length: 20 }, (_, i) => ({
            x0: i * 0.5,
            x1: (i + 1) * 0.5,
            count: 0,
        }));
        renderHistogram();
    }

    function renderHistogram() {
        d3.select(marks)
            .selectAll("rect")
            .data(bins)
            .enter()
            .append("rect")
            .style("fill", "steelblue")
            .attr("x", (d) => xScale(d.x0))
            .attr("width", (d) => xScale(d.x1) - xScale(d.x0) - 1)
            .attr("y", (d) => yScale(d.count))
            .attr("height", (d) => yScale(0) - yScale(d.count));
    }

    function animateCircles() {
        if (animationRunning) return;
        animationRunning = true;

        interval = setInterval(() => {
            if (circlesData.length >= 120) {
                stopAnimation();
                message = "You've observed all data available!";
                return;
            }

            const newValue = d3.randomNormal(7.0, 1.5)();

            const circle = {
                x: xScale(Math.floor(newValue / 0.5) * 0.5 + 0.25),
                y: -10,
                value: newValue.toFixed(1),
            };

            circlesData.push(circle);
            const binIndex = Math.floor(newValue * 2);
            bins[binIndex].count++;

            createCircle(circle, binIndex);
        }, 500);
    }

    function createCircle(circle, binIndex) {
        const circleSelection = d3
            .select(marks)
            .append("g")
            .attr("class", "circle-group");

        circleSelection
            .append("circle")
            .data([circle])
            .style("fill", "orange")
            .style("opacity", 0.7)
            .attr("cx", circle.x)
            .attr("cy", circle.y)
            .attr("r", 5)
            .transition()
            .duration(1500)
            .ease(d3.easeLinear)
            .attr("cy", yScale(bins[binIndex].count))
            .on("end", function () {
                updateHistogram(binIndex);
                d3.select(this).remove();
            });

        circleSelection
            .append("text")
            .text(circle.value)
            .attr("x", circle.x + 10)
            .attr("y", circle.y)
            .style("fill", "black")
            .style("font-size", "14px")
            .transition()
            .duration(1500)
            .ease(d3.easeLinear)
            .attr("y", yScale(bins[binIndex].count) - 5)
            .on("end", function () {
                d3.select(this).remove();
            });
    }

    function updateHistogram(binIndex) {
        d3.select(marks)
            .selectAll("rect")
            .data(bins)
            .transition()
            .duration(500)
            .attr("y", (d) => yScale(d.count))
            .attr("height", (d) => yScale(0) - yScale(d.count));
    }

    function stopAnimation() {
        clearInterval(interval);
        animationRunning = false;
    }

    function reset() {
        clearInterval(interval);
        animationRunning = false;
        circlesData = [];
        bins = Array.from({ length: 20 }, (_, i) => ({
            x0: i * 0.5,
            x1: (i + 1) * 0.5,
            count: 0,
        }));
        message = "";
        updateHistogram();
        d3.select(marks).selectAll(".circle-group").remove();
    }
</script>

<main>
    <svg {width} {height}>
        <g
            style="transform: translate({margin.left}px, {margin.top}px)"
            bind:this={marks}
        ></g>
        <g
            style="transform: translate({margin.left}px, {height -
                margin.bottom}px)"
            bind:this={xAxis}
        ></g>
        <g
            style="transform: translate({margin.left}px, {margin.top}px)"
            bind:this={yAxis}
        ></g>
    </svg>

    <p style="color: orange; font-size: 1.2rem; font-weight: bold;">
        {message}
    </p>
    <br />
    <button on:click={animateCircles}>See Data</button>
    <button on:click={stopAnimation}>Stop</button>
    <button on:click={reset}>Reset</button>
</main>

<style>
    button {
        border: none;
        padding: 0.5rem 2rem;
        color: slategrey;
        font-size: 1.2rem;
        border-radius: 1rem;
        transition: all 250ms;
        transform-origin: center;
        box-shadow:
            0px 3px 3px rgba(0, 0, 0, 0.25),
            inset 0px -2px 3px rgba(0, 0, 0, 0.25);
    }
    button:hover {
        cursor: pointer;
        transform: scale(0.975);
        box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.25);
    }
</style>
