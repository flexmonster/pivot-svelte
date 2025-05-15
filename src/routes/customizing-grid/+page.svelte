<script lang="ts">
    import {Flexmonster} from "svelte-flexmonster"; // Assume you wrapped or aliased it
    import ToggleButton from "../../components/ToggleButton.svelte";
    import { onMount } from "svelte";

    let pivot: Flexmonster.Pivot;

    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
        toolbar.showShareReportTab = true;
    }

    function customizeCellFunction(
        cell: Flexmonster.CellBuilder,
        data: Flexmonster.CellData,
    ): void {
        if (data.measure && data.measure.uniqueName == "Price") {
            let backgroundColor = "#00A45A";
            let textShadowColor = "#095231";
            let borderColor = "#009552";
            cell.style = {
                ...cell.style,
                "background-color": backgroundColor,
                color: "white",
                "font-weight": "bold",
                "text-shadow": `0px 2px 3px ${textShadowColor}`,
                "border-bottom": `1px solid ${borderColor}`,
                "border-right": `1px solid ${borderColor}`,
            };
        }
    }

    function removeCustomization(): void {
        pivot.customizeCell(() => null);
    }

    function applyCustomization(): void {
        pivot.customizeCell(customizeCellFunction);
    }

    function toggleCustomization(event: boolean): void {
        if (event) {
            removeCustomization();
        } else {
            applyCustomization();
        }
    }
</script>

<div>
    <h1 class="page-title">Customizing the grid</h1>
    <div class="description-blocks first-description-block">
        <p>
            Style the grid by adding links, applying custom CSS, or formatting
            the cells. Check our docs for details:
            <a
                href="https://www.flexmonster.com/doc/customizing-grid/?r=rm_svelte"
                target="_blank"
                class="title-link"
            >
                Customizing the grid
            </a>
            .
        </p>
        <p>In this demo, the <strong>Price</strong> measure is customized.</p>
    </div>
    <div class="description-blocks">
        <ToggleButton
            onChange={toggleCustomization}
            labelOn="The grid cells are customized"
            labelOff="The grid cells are not customized"
            id="customizationToggle"
            checked={true}
        />
    </div>

    <Flexmonster
        bind:pivot={pivot}
        toolbar={true}
        report="https://cdn.flexmonster.com/github/customizing-grid-report.json"
        shareReportConnection={{ url: "https://olap.flexmonster.com:9500" }}
        height={600}
        customizeCell={customizeCellFunction}
        beforetoolbarcreated={customizeToolbar}
    />
</div>
