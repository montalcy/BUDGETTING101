<script setup>
    import { Pie } from 'vue-chartjs'
    import {
      Chart as ChartJS,
      Tooltip,
      Legend,
      ArcElement
    } from 'chart.js'
    
    ChartJS.register(Tooltip, Legend, ArcElement)
    
    const props = defineProps({
      data: Object,
      options: Object
    })
    </script>
    
    <template>
      <Pie :data="data" :options="options" />
    </template>
    