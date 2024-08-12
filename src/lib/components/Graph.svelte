<script>
	import { onMount, onDestroy } from 'svelte';
	import { LineChart } from '@carbon/charts-svelte';
	import '@carbon/charts-svelte/styles.css';

	export let rawData = {};
	let chart;

	$: chartData = transformData(rawData);

	function transformData(data) {
		return data.months.flatMap((month) => [
			{ group: 'Green Score', date: month, value: data.green_scores[month] },
			{ group: 'Electricity', date: month, value: data.electricity[month] },
			{ group: 'Water', date: month, value: data.water[month] },
			{ group: 'LPG', date: month, value: data.lpg[month] },
			{ group: 'Recycle Points', date: month, value: data.recycle_points[month] }
		]);
	}

	const chartOptions = {
		title: 'Environmental Metrics Over Time',
		axes: {
			left: {
				mapsTo: 'value',
				title: 'Value',
				scaleType: 'linear',
				titleFontSize: 14,
				labelFontSize: 12,
				titleFontColor: '#5A6872',
				labelFontColor: '#5A6872'
			},
			bottom: {
				mapsTo: 'date',
				scaleType: 'labels',
				titleFontSize: 14,
				labelFontSize: 12,
				titleFontColor: '#5A6872',
				labelFontColor: '#5A6872'
			}
		},
		curve: 'curveMonotoneX',
		height: '400px',
		legend: {
			fontSize: 12,
			fontColor: '#5A6872'
		},
		color: {
			scale: {
				'Green Score': '#00A78F',
				Electricity: '#5A6872',
				Water: '#5596E6',
				LPG: '#FF7832',
				'Recycle Points': '#9B82F3'
			}
		},
		grid: {
			x: {
				enabled: true,
				color: '#E0E0E0'
			},
			y: {
				enabled: true,
				color: '#E0E0E0'
			}
		},
		titleFontSize: 18,
		titleFontColor: '#152935',
		tooltip: {
			fontSize: 12
		}
	};

	onMount(() => {
		if (chart) {
			chart.render();
		}
	});

	onDestroy(() => {
		if (chart) {
			chart.destroy();
		}
	});
</script>

<LineChart bind:chart data={chartData} options={chartOptions} />
