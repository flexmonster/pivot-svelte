<script lang="ts">
    import Highcharts from "highcharts";
    //Importing Flexmonster Connector for Highcharts:
    import "flexmonster/lib/flexmonster.highcharts";
    import "flexmonster/lib/flexmonster.amcharts";
    import { Flexmonster } from "svelte-flexmonster";

    let pivot: Flexmonster.Pivot;
    let chartdiv: HTMLDivElement;

    function reportComplete(): void {
        pivot.off("reportcomplete", reportComplete);
        drawChart();
    }

    function drawChart(): void {
        pivot.highcharts?.getData(
            {
                type: "spline",
            },
            function (data: Flexmonster.GetDataValueObject) {
                Highcharts.chart(chartdiv, data as Highcharts.Options);
            },
            function (data: Flexmonster.GetDataValueObject) {
                Highcharts.chart(chartdiv, data as Highcharts.Options);
            },
        );
    }

    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
        toolbar.showShareReportTab = true;
    }
</script>

<div>
    <h1 class="page-title">Integrating with Highcharts</h1>
    <div class="description-blocks first-description-block">
        <p>
            Integrate Flexmonster with Highcharts and see your data from a new
            perspective:
            <a
                href="https://www.flexmonster.com/doc/integration-with-highcharts/?r=rm_svelte"
                target="_blank"
                class="title-link">Integration with Highcharts</a
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
            id="highcharts-container"
            style="width: 100%; height: 500px"
        ></div>
    </div>
</div>
