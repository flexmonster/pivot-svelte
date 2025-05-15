<script lang="ts">
    import { Flexmonster } from "svelte-flexmonster";
    import ToggleButton from "../../components/ToggleButton.svelte";

    interface Log {
      id: string;
      date: string;
      event: string;
    }
    
    let logs : Log[] = [];
    let logsContainer: HTMLDivElement;
    let pivot: Flexmonster.Pivot;

    const eventList = [
        "afterchartdraw",
        "aftergriddraw",
        "beforegriddraw",
        "beforetoolbarcreated",
        "cellclick",
        "celldoubleclick",
        "chartclick",
        "datachanged",
        "dataerror",
        "datafilecancelled",
        "dataloaded",
        "drillthroughclose",
        "drillthroughopen",
        "exportcomplete",
        "exportstart",
        "fieldslistclose",
        "fieldslistopen",
        "filterclose",
        "filteropen",
        "loadingdata",
        "loadinglocalization",
        "loadingolapstructure",
        "loadingreportfile",
        "localizationerror",
        "localizationloaded",
        "olapstructureerror",
        "olapstructureloaded",
        "openingreportfile",
        "printcomplete",
        "printstart",
        "querycomplete",
        "queryerror",
        "ready",
        "reportchange",
        "reportcomplete",
        "reportfilecancelled",
        "reportfileerror",
        "runningquery",
        "update",
      ];

      function customizeToolbar(toolbar: Flexmonster.Toolbar): void {
      toolbar.showShareReportTab = true;
    }

    function printLog(log: string): void {
      logs = [...logs, {
        id: new Date().getTime() + log,
        date: new Date().toLocaleTimeString(),
        event: log,
      }];
      requestAnimationFrame(() => {
        if (logsContainer) {
          (logsContainer as HTMLDivElement).scrollTop = (logsContainer as HTMLDivElement).scrollHeight;
        }
      });
    }

    function signOffAllEvents(): void {
      for (const eventName of eventList) {
        //remove all handlers for specified event
        pivot.off(eventName);
      }
    }

    function signOnAllEvents(): void {
      for (const eventName of eventList) {
        //add handler for specified event
        pivot.on(eventName, () => {
          printLog(eventName);
        });
      }
    }
    
    function toggleEvents(event: boolean): void {
      if (event) {
        signOffAllEvents();
      } else {
        signOnAllEvents();
      }
    }

    function clearLogs(): void {
      logs = [];
    }
  
</script>

<div>
    <h1 class="title-one page-title">Handling Flexmonster events</h1>

    <div class="description-blocks first-description-block">
      <p>
        Perform an action (for example, click on a grid cell) to trigger a
        <a
          class="title-link"
          target="blank"
          href="https://www.flexmonster.com/api/events/?r=rm_vue"
          >Flexmonster event</a
        >. Scroll down to the log output to see which events get triggered.
      </p>
    </div>
    <div class="description-blocks">
      <ToggleButton
        id="eventsToggle"
        labelOn="Events are tracked"
        labelOff="Events are not tracked"
        checked={true}
        onChange={toggleEvents}
      />
    </div>
    <Flexmonster
      bind:pivot={pivot}
      toolbar
      height="600"
      report="https://cdn.flexmonster.com/github/demo-report.json"
      ready={signOnAllEvents}
      shareReportConnection={{
        url: 'https://olap.flexmonster.com:9500',
      }}
      beforetoolbarcreated={customizeToolbar}
    />

    <div class="section">
      <h3 class="title-4">Log Output</h3>
      <div class="event-logs-wrapper fullwidth">
       <div bind:this={logsContainer} class="content">
        {#each logs as log (log.id)}
        <div>
            <span class="log-label">[ Event ] {log.date}:</span>
            {log.event} [
            <a
              class="log-link"
              href={`https://www.flexmonster.com/api/${log.event}/?r=rm_svelte`}
            >
              see details
            </a>
            ]
          </div>
        {/each}
        </div> 
      </div>
      <div class="section--button">
        <button class="button-red" on:click={clearLogs}>
          Clear Log Output
        </button>
      </div>
    </div>
  </div>