<script lang="ts">
    // amCharts imports
    import * as am4core from "@amcharts/amcharts4/core";
    import * as am4charts from "@amcharts/amcharts4/charts";
    import am4themes_animated from "@amcharts/amcharts4/themes/animated";

    //Importing Flexmonster Connector for amCharts:
    import "flexmonster/lib/flexmonster.amcharts";
    import { Flexmonster } from "svelte-flexmonster";
    import { onDestroy } from "svelte";

    let pivot: Flexmonster.Pivot;
    let chart: am4charts.PieChart;
    let chartdiv: HTMLDivElement;

    function reportComplete(): void {
        pivot.off("reportcomplete", reportComplete);
        drawChart();
    }

    function drawChart(): void {
        pivot.amcharts?.getData({}, createChart, updateChart);
    }

    function createChart(chartData: Flexmonster.GetDataValueObject, rawData: Flexmonster.GetDataValueObject): void {
      /* Apply amCharts theme */
      am4core.useTheme(am4themes_animated);

      /* Create chart instance */
      chart = am4core.create("amcharts-container", am4charts.PieChart);

      /* Add data processed by Flexmonster to the chart */
      chart.data = chartData.data;

      /* Set an inner radius to transform a pie chart into a donut chart */
      chart.innerRadius = am4core.percent(50);

      /* Create and configure series for a pie chart */
      var pieSeries = chart.series.push(new am4charts.PieSeries());
      pieSeries.dataFields.category = pivot.amcharts?.getCategoryName(rawData);
      pieSeries.dataFields.value = pivot.amcharts?.getMeasureNameByIndex(rawData, 0);
      pieSeries.slices.template.stroke = am4core.color("#fff");
      pieSeries.slices.template.strokeWidth = 2;
      pieSeries.slices.template.strokeOpacity = 1;

      /* Create initial animation */
      pieSeries.hiddenState.properties.opacity = 1;
      pieSeries.hiddenState.properties.endAngle = -90;
      pieSeries.hiddenState.properties.startAngle = -90;
    }

    function updateChart(chartData: Flexmonster.GetDataValueObject, rawData: Flexmonster.GetDataValueObject) {
        chart?.dispose();
        createChart(chartData, rawData);
    }

    onDestroy(() => {
        if (chart) {
            chart.dispose();
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
        <div
            bind:this={chartdiv}
            id="amcharts-container"
            style="width: 100%; height: 500px"
        ></div>
    </div>
</div>
