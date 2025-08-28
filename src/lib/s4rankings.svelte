<script>
    import { onMount } from 'svelte';
    import Chart from 'chart.js/auto'; // Using 'chart.js/auto' for simplicity, or 'chart.js' with explicit register

    export let chartData = {
        labels: ['Dicks', 'Kyle', 'Tyler', 'Greg', 'Paul', 'Ryan', 'Fonte', 'John', 'Allan', 'Mike', 'Melanson', 'Mulch'],
        datasets: [
            {
                label: 'Fonte Ranks',
                data: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
                backgroundColor: 'rgba(54, 162, 235, 0.5)',
                borderColor: 'rgba(54, 162, 235, 1)',
                borderWidth: 1
            },
            {
                label: 'Site Ranks',
                data: [1, 9, 3, 2, 3, 7, 5, 4, 11, 8, 12, 6],
                backgroundColor: 'rgba(255, 99, 132, 0.5)',
                borderColor: 'rgba(255, 99, 132, 1)',
                borderWidth: 1
            }
        ]
    };

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