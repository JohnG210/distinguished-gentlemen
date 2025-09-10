<script>
    // Example blog post usage: [CHART:rankComparison:[{"name":"Dicks","fonte_rank":1,"fonte_change":0,"site_rank":2,"site_change":0},{"name":"Kyle","fonte_rank":2,"fonte_change":0,"site_rank":12,"site_change":0},{"name":"Tyler","fonte_rank":3,"fonte_change":0,"site_rank":1,"site_change":0},{"name":"Greg","fonte_rank":4,"fonte_change":0,"site_rank":4,"site_change":0},{"name":"Paul","fonte_rank":5,"fonte_change":0,"site_rank":10,"site_change":0},{"name":"Ryan","fonte_rank":6,"fonte_change":0,"site_rank":6,"site_change":0},{"name":"Fonte","fonte_rank":7,"fonte_change":0,"site_rank":5,"site_change":0},{"name":"John","fonte_rank":8,"fonte_change":0,"site_rank":3,"site_change":0},{"name":"Allan","fonte_rank":9,"fonte_change":0,"site_rank":9,"site_change":0},{"name":"Mike","fonte_rank":10,"fonte_change":0,"site_rank":7,"site_change":0},{"name":"Melanson","fonte_rank":11,"fonte_change":0,"site_rank":11,"site_change":0},{"name":"Mulch","fonte_rank":12,"fonte_change":0,"site_rank":4,"site_change":0}]]
    import { onMount } from 'svelte';
    import Chart from 'chart.js/auto';
    import ChartDataLabels from 'chartjs-plugin-datalabels'; // Import the datalabels plugin

    Chart.register(ChartDataLabels); // Register the datalabels plugin

    export let personData; // New prop for the list of person objects

    let chartData = {}; // Internal reactive variable for Chart.js

    // Reactive statement to transform personData into chartData for Chart.js
    $: if (personData) {
        const labels = personData.map(p => p.name);
        const fonteRanks = personData.map(p => p.fonte_rank);
        const siteRanks = personData.map(p => p.site_rank);

        chartData = {
            labels: labels,
            datasets: [
                {
                    label: 'Fonte Rank',
                    data: fonteRanks,
                    backgroundColor: 'rgba(75, 192, 192, 0.6)'
                },
                {
                    label: 'Site Rank',
                    data: siteRanks,
                    backgroundColor: 'rgba(153, 102, 255, 0.6)'
                }
            ]
        };
    }

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
            title: { display: true, text: 'Rank Comparison: Fonte vs. Site' },
            datalabels: {
                align: 'end',
                anchor: 'end',
                formatter: (value, context) => {
                    const person = personData[context.dataIndex];
                    let change = 0;
                    if (context.dataset.label === 'Fonte Rank') {
                        change = person.fonte_change;
                    } else if (context.dataset.label === 'Site Rank') {
                        change = person.site_change;
                    }
                    if (change === 0) {
                        return value;
                    } else if (change > 0) {
                        return `${value} (+${change})`;
                    } else {
                        return `${value} (${change})`;
                    }
                }
            }
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
