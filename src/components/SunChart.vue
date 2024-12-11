<template>
  <div id="chart" class="chart-container"></div>
</template>

<script setup>
import { onMounted, watchEffect } from 'vue';
import * as echarts from 'echarts';

const props = defineProps({
  weather: {
    type: Object,
    required: true,
  },
});

const convertToHours = (time) => {
  if (time != null && !isNaN(time)) {
    const date = new Date(time * 1000);
    return date.getHours() + date.getMinutes() / 60;
  }
  console.log("InvalidDate", time)
  return 0
};

const generateParabola = (sunrise, sunset) => {
  const data = [];
  const timeDiff = sunset - sunrise;
  const maxHeightY = (timeDiff < 2) ? 0.1 :
                   (timeDiff >= 2 && timeDiff < 3) ? 0.2 :
                   (timeDiff >= 3 && timeDiff < 4) ? 0.3 :
                   (timeDiff >= 4 && timeDiff < 5) ? 0.4 :
                   (timeDiff >= 5 && timeDiff < 6) ? 0.5 :
                   (timeDiff >= 6 && timeDiff < 7) ? 0.6 :
                   (timeDiff >= 7 && timeDiff < 8) ? 0.7 :
                   (timeDiff >= 8 && timeDiff < 9) ? 0.8 :
                   (timeDiff >= 9 && timeDiff < 10) ? 0.9 : 1;

  const maxHeightX = (sunset + sunrise)/2;

  data.push([sunrise, 0], [maxHeightX, maxHeightY], [sunset, 0]);

  return data;
};

const initChart = () => {
  const chartDom = document.getElementById('chart');
  if (!chartDom) {
    console.log('Chart container not found');
    return;
  }

  const myChart = echarts.init(chartDom);

  const sunrise = convertToHours(props.weather.sys.sunrise);
  const sunset = convertToHours(props.weather.sys.sunset);

  const option = {
    xAxis: {
      type: 'value',
      min: 0,
      max: 24,
      interval: 4,
      data: [0, 4, 8, 12, 16, 20, 24],
    },
    yAxis: {
      type: 'value',
      show: false,
      min: 0,
      max: 2,
      interval: 0.1,
    },
    series: [
      {
        type: 'line',
        data: generateParabola(sunrise, sunset),
        smooth: true,
        lineStyle: {
          color: '#f7a456',
        },
        symbol: 'circle',
        symbolSize: 1,
      },
    ],
    grid: {
      left: '10%',
      right: '10%',
      top: '0%',
      bottom: '18%',
    },
  };

  myChart.setOption(option);
};

onMounted(() => {
  initChart();
});

watchEffect(() => {
  if (props.weather && props.weather.sys) {
    initChart();
  }
});
</script>

<style scoped>
.chart-container {
  grid-area: chart;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center; 
  justify-content: center;
}
</style>