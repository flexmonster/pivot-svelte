<script lang="ts">
    import { Flexmonster } from "svelte-flexmonster"; 

    let pivot: Flexmonster.Pivot;

    function showInfo(): void {
        pivot.alert({
            title: "Customizing Flexmonster",
            message:
                "How to customize the Toolbar: <a style='text-decoration:underline; color:#00A45A' target='blank' href='https://www.flexmonster.com/doc/customizing-toolbar/?r=rm_svelte'>https://www.flexmonster.com/doc/customizing-toolbar/</a> <br>",
            type: "info",
            blocking: false,
        });
    }

    function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
      let tabs: Flexmonster.ToolbarTab[] = toolbar.getTabs();
      toolbar.getTabs = () => {
        tabs = [];
        // add new tab
        tabs.push({
          id: "fm-tab-newtab",
          title: "New Tab",
          handler: showInfo,
          icon: toolbar.icons.open,
        });
        return tabs;
      };
    }
</script>

<div>
    <h1 class="page-title">Customizing the Toolbar</h1>
    <div class="description-blocks first-description-block">
        <p>You can add, remove, and update the Toolbar tabs.</p>
        <p>
            In this demo, we’ve removed all the tabs and added a custom
            <strong>New Tab</strong>. See our docs to learn more about the
            Toolbar and its customization:
            <a
                href="https://www.flexmonster.com/doc/customizing-toolbar/?r=rm_svelte"
                target="_blank"
                class="title-link">Customizing the Toolbar</a
            >.
        </p>
    </div>

    <Flexmonster
        bind:pivot
        toolbar={true}
        report="https://cdn.flexmonster.com/github/customizing-grid-report.json"
        height={600}
        beforetoolbarcreated={customizeToolbar}
    />
</div>
