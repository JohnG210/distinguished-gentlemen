<script>
    // Example blog usage: [CHART:countdown:{"targetDateTime":"2025-09-04 08:15:00","title":"Time To The Regular Season","hideOnComplete":true}]
    import { onMount, onDestroy } from 'svelte';
    import moment from 'moment-timezone';

    export let targetDateTime; // e.g., "2025-12-25 00:00:00"
    export let title = "Countdown";
    export let hideOnComplete = false;

    let countdown = {
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0
    };

    let interval;
    let hasCompleted = false;

    // Function to calculate countdown
    function calculateCountdown() {
        const target = moment.tz(targetDateTime, "America/New_York");
        const now = moment();
        const diff = target.diff(now);

        if (diff <= 0) {
            countdown = { days: 0, hours: 0, minutes: 0, seconds: 0 };
            hasCompleted = true;
            clearInterval(interval);
            return;
        }

        const duration = moment.duration(diff);
        countdown.days = Math.floor(duration.asDays());
        countdown.hours = duration.hours();
        countdown.minutes = duration.minutes();
        countdown.seconds = duration.seconds();
        hasCompleted = false;
    }

    onMount(() => {
        calculateCountdown(); // Initial calculation
        interval = setInterval(calculateCountdown, 1000); // Update every second
    });

    onDestroy(() => {
        if (interval) {
            clearInterval(interval);
        }
    });

    // Reactive statement to re-calculate if targetDateTime changes
    $: targetDateTime, calculateCountdown();
</script>

{#if !(hideOnComplete && hasCompleted)}
<div class="countdown-container">
    <h2>{title}</h2>
    <div class="countdown-timer">
        <div class="time-segment">
            <span class="number">{countdown.days}</span>
            <span class="label">Days</span>
        </div>
        <div class="time-segment">
            <span class="number">{countdown.hours}</span>
            <span class="label">Hours</span>
        </div>
        <div class="time-segment">
            <span class="number">{countdown.minutes}</span>
            <span class="label">Minutes</span>
        </div>
        <div class="time-segment">
            <span class="number">{countdown.seconds}</span>
            <span class="label">Seconds</span>
        </div>
    </div>
</div>
{/if}

<style>
    .countdown-container {
        text-align: center;
        margin: 0px 0;
        padding: 00px;
        background-color: var(--f8f8f8); /* Adapted for theme */
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    h2 {
        color: var(--g333); /* Adapted for theme */
        margin-bottom: 15px;
        font-size: 1.8em;
    }

    .countdown-timer {
        display: flex;
        justify-content: center;
        gap: 20px;
    }

    .time-segment {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: var(--fff); /* Adapted for theme */
        padding: 15px 20px;
        border-radius: 6px;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
        min-width: 80px;
    }

    .number {
        font-size: 2.5em;
        font-weight: bold;
        color: var(--blueTwo); /* Adapted for theme */
    }

    .label {
        font-size: 0.9em;
        color: var(--g999); /* Adapted for theme */
        text-transform: uppercase;
        margin-top: 5px;
    }
</style>