<template>
    <widget-period id="jvm_non_heap_memory" title="非堆内存使用情况">
    </widget-period>
</template>
<style>
</style>
<script>
    import WidgetPeriod from '../components/WidgetPeriod.vue';
    import Monitor from '../common/monitor.api';
    import Tools from '../common/tools';

    export default{
        components: {
            WidgetPeriod
        },
        methods: {
            getDataApi(){
                return Monitor.getJVMMemory;
            },
            getInitOption(){
                return {
                    tooltip: {
                        trigger: 'axis',
                        formatter: function (params, ticket, callback) {
                            return Tools.formatter(params, 'MB');
                        }
                    },
                    grid: {
                        top: '15%', left: '5%', right: '5%', bottom: '5%', containLabel: true
                    },
                    legend: {
                        top: 14,
                        data: ['已提交', '已使用']
                    },
                    xAxis: [{
                        type: 'category',
                        boundaryGap: false,
                        data: []
                    }],
                    yAxis: [{
                        name: '容量（MB）',
                        type: 'value'
                    }],
                    series: [{
                        name: '已提交', type: 'line', data: []
                    }, {
                        name: '已使用', type: 'line', data: []
                    }]
                };
            },
            // 数据转换
            toItemData(item) {
                let xAxisData = Tools.dateToHHmm(item.date), data1 = 0, data2 = 0, obj = item.ifcJVMMemory;

                data1 = parseFloat(Tools.byteToMB(obj.nonHeapMemoryUsage.committed).toFixed(2));
                data2 = parseFloat(Tools.byteToMB(obj.nonHeapMemoryUsage.used).toFixed(2));

                let seriesData = Array.of(data1, data2);
                return {xAxisData, seriesData}
            }
        }
    }
</script>  