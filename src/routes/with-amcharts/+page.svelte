<script lang="ts">
    // amCharts imports
    import * as am5 from "@amcharts/amcharts5";
    import * as am5xy from "@amcharts/amcharts5/xy";
    import am5themes_Animated from "@amcharts/amcharts5/themes/Animated";

    //Importing Flexmonster Connector for amCharts:
    import "flexmonster/lib/flexmonster.amcharts";
    import { Flexmonster } from "svelte-flexmonster";
    import { onDestroy } from "svelte";

    let pivot: Flexmonster.Pivot;
    let root: am5.Root; 
    let chartdiv: HTMLDivElement;  

    function reportComplete(): void {
        pivot.off("reportcomplete", reportComplete);
        drawChart();
    }

    function drawChart(): void {
        pivot.amcharts?.getData({},
         createChart,
         updateChart
        )
    }

    function createChart(chartData: Flexmonster.GetDataValueObject, rawData: Flexmonster.GetDataValueObject): void{
            /* Create root element and chart instance */
            root = am5.Root.new(chartdiv);
            let chart = root.container.children.push(am5xy.XYChart.new(root, {
            }));

            /* Apply amCharts theme */
            root.setThemes([
                am5themes_Animated.new(root),
            ]);

            /* Apply number format from Flexmonster */
            root.numberFormatter.set("numberFormat", pivot.amcharts?.getNumberFormatPattern((rawData.meta as any).formats[0]));

            /* Create and configure Y axis */
            let yAxis = chart.yAxes.push(am5xy.CategoryAxis.new(root, {
                categoryField: pivot.amcharts?.getCategoryName(rawData)!,
                renderer: am5xy.AxisRendererY.new(root, {
                    cellStartLocation: 0.1,
                    cellEndLocation: 0.9
                })
            }));

            /* Create and configure X axis */
            let xAxis = chart.xAxes.push(am5xy.ValueAxis.new(root, {
                renderer: am5xy.AxisRendererX.new(root, {}),
            }));

            xAxis.set("numberFormatter", am5.NumberFormatter.new(root, {
                "numberFormat": "#a"
            }));

            /* Create and configure series for a bar chart */
            let series = chart.series.push(am5xy.ColumnSeries.new(root, {
                name: pivot.amcharts?.getMeasureNameByIndex(rawData, 0),
                xAxis: xAxis,
                yAxis: yAxis as any,
                sequencedInterpolation: true,
                valueXField: pivot.amcharts?.getMeasureNameByIndex(rawData, 0),
                categoryYField: pivot.amcharts?.getCategoryName(rawData),
                tooltip: am5.Tooltip.new(root, {
                    labelText: '{name}: [bold]{valueX}[/]'
                })
            }));

            chart.set("cursor", am5xy.XYCursor.new(root, {
                behavior: "none",
                xAxis: xAxis,
                yAxis: yAxis
            }));

            /* Add data processed by Flexmonster to the chart */
            yAxis.data.setAll(chartData.data);
            series.data.setAll(chartData.data);

            /* Create initial animation */
            series.appear(1000);
            chart.appear(1000, 100);
    }

    function updateChart(chartData: Flexmonster.GetDataValueObject, rawData: Flexmonster.GetDataValueObject) {
        root?.dispose();
        createChart(chartData, rawData);
    }

    onDestroy(() => {
        if (root) {
            root.dispose();
        }
    });

    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
        toolbar.showShareReportTab = true;
    }
</script>

<div>
    <h1 class="page-title">Integrating with amCharts</h1>
    <div class="description-blocks first-description-block">
        <p>
            Extend Flexmonster’s visualization functionality by integrating with
            the amCharts library:
            <a
                href="https://www.flexmonster.com/doc/integration-with-amcharts/?r=rm_svelte"
                target="_blank"
                class="title-link">Integration with amCharts</a
            >.
        </p>
    </div>
    <Flexmonster
        bind:pivot
        toolbar
        height="600"
        report="https://cdn.flexmonster.com/github/demo-report.json"
        reportcomplete={reportComplete}
        shareReportConnection={{
            url: "https://olap.flexmonster.com:9500",
        }}
        beforetoolbarcreated={customizeToolbar}
        licenseFilePath="https://cdn.flexmonster.com/jsfiddle.charts.key"
    />
    <div class="chart-container">
        <div bind:this={chartdiv} id="amcharts-container" style="width: 100%; height: 500px"></div>
    </div>
</div>
