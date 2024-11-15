<script>
    import Prior from "./Prior.svelte";
    import Data from "./Data.svelte";
    import Posterior from "./Posterior.svelte";

    let priorMin = 0;
    let priorMax = 10;

    const handleSliderUpdate = (event) => {
        priorMin = event.detail.minValue;
        priorMax = event.detail.maxValue;
    };
</script>

<div class="content">
    <section class="section-intro">
        <h1>What Do "Prior" and "Posterior" Mean?</h1>
        <p>
            Imagine you're playing a fun guessing game. In front of you is a box
            filled with fruit, and your job is to guess its weight. What could
            it be? Is it light, like tangerines, or heavy, like pineapples?
            Before you pick it up or weigh it, you take a guess. This guess is
            similar to something statisticians call a "prior belief" — an
            initial assumption about something you don’t know yet.
        </p>
    </section>

    <section class="section-prior">
        <h2>Part I. Setting the Prior</h2>
        <p>
            Let’s set our guess! Using the slider, you can adjust your belief
            about the weight. If you move the slider all the way to the edges,
            say 0–10 kg, that's a very vague or "uninformative" prior. It
            doesn’t assume much about the weight. But if you move the slider to
            a narrower range, like 4–5 kg, that’s a more "informative" prior,
            assuming more about the weight from the start. These ranges
            represent your uncertainty — you don’t know exactly, but you have a
            rough idea. In Bayesian analysis, we call this our
            <strong>prior</strong>.
        </p>
        <div class="visualization">
            <h3 class="section-header">Adjust Your Guess</h3>
            <Prior on:update={handleSliderUpdate} />
            <p class="short-text">
                As you adjust the slider, notice how a wider range captures more
                possibilities but with less confidence, while a narrower range
                shows more certainty. This helps us understand how much “weight”
                our prior belief carries.
            </p>
        </div>
    </section>

    <section class="section-data">
        <h2>Part II. Updating Beliefs</h2>
        <p>
            Alright, your prior belief seems like somewhere between {priorMin} -
            {priorMax} kg. Now, let’s gather some clues! Click the 'See Data' button
            below to reveal some weight measurements from similar boxes. You can
            choose to stop midway or view all available data points. These new data
            points will help us shape our belief. You might notice that the data
            clusters around a specific weight, perhaps around 7 kg. This new information
            allows us to refine our guess — a process known as
            <strong>updating</strong>.
        </p>
        <div class="visualization">
            <h3 class="section-header">Observe the Data</h3>
            <Data />
            <p class="short-text">
                Are you enjoying seeing how adding more measurements helps you
                narrow down your guess? With more data, you get a clearer
                picture, and your initial wide range of possibilities becomes
                more focused around a more specific value.
            </p>
        </div>
    </section>

    <section class="section-posterior">
        <h2>Part III. Building the Posterior Belief</h2>
        <p>
            Now comes the fun part — the <strong>posterior</strong>! The
            posterior represents our updated understanding of the weight,
            blending the initial guess we made (the prior) with the new clues
            we've gathered (the data). In the visualization below, you'll see
            the prior distribution that you specified and the data distribution
            you've observed (for convenience, I've approximated the prior curve
            as a normal distribution based on the range you chose). The
            posterior distribution reflects how our belief evolves by
            integrating both the prior knowledge and the new evidence.
        </p>
        <div class="visualization">
            <h3 class="section-header">Form the Posterior</h3>
            <Posterior minValue={priorMin} maxValue={priorMax} />
            <p class="short-text">
                Watch how the prior curve merges with the data to form the
                posterior, which often peaks near the average of the data
                points. Use the sliders above to experiment with different
                values for the data's mean and standard deviation, and see how
                everything fits together!
            </p>
        </div>
    </section>

    <section class="section-wrapup">
        <h2>A Closing Thought for You</h2>
        <p>
            Priors are like a valuable starting point in Bayesian analysis,
            especially when we have some domain knowledge or limited data. It’s
            often best to use <strong>weakly informative</strong> priors — ones that
            give us some direction without being overly restrictive. This approach
            allows the data to speak for itself and also guides us toward more plausible
            conclusions. The balance between initial beliefs and observed data is
            a powerful tool for understanding uncertainty across a wide range of
            situations!
        </p>
    </section>
</div>

<style>
    :global(body) {
        font-family: "Roboto", sans-serif;
        background: linear-gradient(145deg, #ffffff 0%, #3498db 100%);
        color: #fff;
        margin: 0;
        padding: 0;
    }

    .content {
        max-width: 1000px;
        margin: 50px auto;
        background-color: #fff;
        border-radius: 20px;
        padding: 40px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        transition:
            transform 0.5s ease,
            box-shadow 0.5s ease;
    }

    .content:hover {
        transform: translateY(-10px);
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
    }

    section {
        padding: 30px 20px;
        margin-bottom: 30px;
        background-color: #f8f8f8;
        border-radius: 10px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        transition: background-color 0.3s ease;
    }

    section:hover {
        background-color: #0a0a0a;
    }

    h1 {
        font-size: 2.6rem;
        font-weight: bold;
        color: #2c3e50;
        margin-bottom: 15px;
        text-align: center;
        animation: fadeInUp 1.2s ease-out;
    }

    h2 {
        font-size: 2rem;
        color: #34495e;
        font-weight: 600;
        margin-top: 25px;
        text-align: center;
        animation: fadeInUp 1.5s ease-out;
    }

    .visualization {
        background-color: #ffffff;
        padding: 30px;
        border-radius: 12px;
        margin-top: 30px;
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
        transition:
            transform 0.3s ease,
            box-shadow 0.3s ease;
    }

    .visualization:hover {
        transform: scale(1.03);
        box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
    }

    .section-header {
        font-size: 1.6rem;
        color: #3498db;
        text-align: center;
        margin-bottom: 10px;
    }

    p {
        font-size: 1.2rem;
        color: #fbf9f9;
        text-align: justify;
        margin-left: 20px;
        margin-right: 20px;
        line-height: 1.6;
    }

    .short-text {
        font-size: 1.2rem;
        color: #7f8c8d;
        text-align: justify;
        margin-top: 20px;
    }

    @keyframes fadeInUp {
        from {
            opacity: 0;
            transform: translateY(50px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
