<script lang="ts">
    //Using the vue-flexmonster module (local registration):
    //1. Importing the vue-flexmonster module:
    import { Flexmonster } from "svelte-flexmonster";
    import ToggleSwitch from "../../components/ToggleSwitch.svelte";
    import ToggleButton from "../../components/ToggleButton.svelte";

    let pivot: Flexmonster.Pivot;
    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
        toolbar.showShareReportTab = true;
    }

    function showChart(): void {
      pivot.showCharts(
        "column"
      );
    }

    function showGrid(): void {
      pivot.showGrid();
    }

    function readOnly(): void {
      pivot.setOptions({
        readOnly: true,
      });
      pivot.refresh();
    }

    function interactive(): void {
      pivot.setOptions({
        readOnly: false,
      });
      pivot.refresh();
    }

    function toggleView(event: boolean): void {
      if (event) {
        showChart();
      } else {
        showGrid();
      }
    }
    //2. Using the vue-flexmonster component:
    function toggleMode(event: boolean): void {
      if (event) {
        readOnly();
      } else {
        interactive();
      }
    }
</script>

<div>
    <h1 class="page-title">Using Flexmonster API calls</h1>
    <div class="description-blocks first-description-block">
        <p>
            Flexmonster provides
            <a
                href="https://www.flexmonster.com/api/methods/?r=rm_svelte"
                target="_blank"
                class="title-link">API calls</a
            >
            for interacting with the component. As an example, we've added the toggle
            buttons below. Use them to switch between the views or make Flexmonster
            read-only.
        </p>
    </div>
    <div class="description-blocks">
        <ToggleSwitch
            id="toggleView"
            labelOn="Grid"
            labelOff="Column chart"
            onChange={toggleView}
        />
        <ToggleSwitch
            id="toggleMode"
            labelOn="Interactive"
            labelOff="Read-only"
            onChange={toggleMode}
        />
    </div>
    <Flexmonster
        bind:pivot
        toolbar
        height="600"
        report="https://cdn.flexmonster.com/github/demo-report.json"
        shareReportConnection={{
            url: "https://olap.flexmonster.com:9500",
        }}
        beforetoolbarcreated={customizeToolbar}
    />
</div>
