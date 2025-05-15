<script lang="ts">
    import { Flexmonster } from "svelte-flexmonster";

    let pivot: Flexmonster.Pivot;

    let data = [
        {
            Category: "Accessories",
            Size: "262 oz",
            Color: "red",
            Destination: "Australia",
            "Business Type": "Specialty Bike Shop",
            Country: "Australia",
            Price: 100,
            Quantity: 225,
            Discount: 23,
        },
        {
            Category: "Components",
            Size: "307 oz",
            Color: "white",
            Destination: "United Kingdom",
            "Business Type": "Warehouse",
            Country: "Canada",
            Price: 200,
            Quantity: 8212,
            Discount: 55,
        },
    ];

    function updateTheData(): void {
        data[0].Price = Math.floor(Math.random() * Math.floor(1000));
        data[1].Price = Math.floor(Math.random() * Math.floor(1000));
        pivot.updateData({ data: data });
    }

    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
        toolbar.showShareReportTab = true;
    }

    function onReady(): void {
        pivot.connectTo({
            data: data,
        });
    }
</script>

<div>
    <h1 class="page-title">Updating the data in Flexmonster</h1>
    <div class="description-blocks first-description-block">
        <p>
            This demo shows how to refresh the data at runtime and keep the
            slice, options, and formatting the same.
        </p>
        <p>
            Try it yourself: configure the component as you wish and click the
            <strong>UPDATE DATA</strong> button.
        </p>
        <p>
            Learn more about updating the data in
            <a
                href="https://www.flexmonster.com/api/updatedata/?r=rm_svelte"
                target="_blank"
                class="title-link">our documentation</a
            >.
        </p>
    </div>
    <button class="button-red" on:click={updateTheData}>Update data</button>
    <Flexmonster
        bind:pivot
        toolbar
        height="400"
        shareReportConnection={{
            url: "https://olap.flexmonster.com:9500",
        }}
        beforetoolbarcreated={customizeToolbar}
        ready={onReady}
    />
</div>
