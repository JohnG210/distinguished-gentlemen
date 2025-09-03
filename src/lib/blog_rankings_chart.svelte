<script>
    //Example blog post usage: [CHART:rankComparison:{"labels":["Dicks","Kyle","Tyler","Greg","Paul","Ryan","Fonte","John","Allan","Mike","Melanson","Mulch"],"fonteRanks":[1,2,3,4,5,6,7,8,9,10,11,12],"siteRanks":[1,9,3,2,3,7,5,4,11,8,12,6]}]
    import { onMount } from 'svelte';
    import Chart from 'chart.js/auto'; // Using 'chart.js/auto' for simplicity, or 'chart.js' with explicit register

    export let chartData;

    export let chartOptions = {
        indexAxis: 'y',
        responsive: true,
        maintainAspectRatio: false,
        scales: {
            x: { beginAtZero: true, title: { display: true, text: 'Rank' } },
            y: { title: { display: true, text: 'Individuals' } }
        },
        plugins: {
            legend: { position: 'top' },
            title: { display: true, text: 'Rank Comparison: Fonte vs. Site' }
        }
    };

    $: if (myChart && chartData) {
        myChart.data = chartData;
        myChart.update();
    }

    let chartCanvas;
    let myChart;

    onMount(() => {
        const ctx = chartCanvas.getContext('2d');
        myChart = new Chart(ctx, {
            type: 'bar',
            data: chartData,
            options: chartOptions
        });

        // Cleanup on component destroy
        return () => {
            if (myChart) {
                myChart.destroy();
            }
        };
    });
</script>

<style>
    .chart-container {
        position: relative;
        width: 100%;
        height: 380px; /* Adjust height as needed */
        margin: 20px auto; /* Center the chart and provide some margin */
    }
    canvas {
        display: block; /* Remove extra space below canvas */
    }
</style>

<div class="chart-container">
    <canvas bind:this={chartCanvas}></canvas>
</div>