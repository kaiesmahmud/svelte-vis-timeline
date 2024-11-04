<script>
    import { DataSet } from "vis-data";
    import { Timeline } from "vis-timeline";
    import { onMount } from "svelte";

    let timeline;
    let container;
    let customTimeElement;

    // Create a data set
    const data = new DataSet([
        {
            id: 1,
            start: new Date(new Date().getTime() - 60 * 1000),
            end: new Date(),
            content: "Dynamic event",
        },
    ]);

    // Specify options
    const options = {
        showCurrentTime: true,
        moveable: true,
        zoomable: true,
        horizontalScroll: true,
        margin: {
            item: 10, // adds space around items
            axis: 5   // adds space around the timeline axis
        },
        stack: false, // if items overlap, try disabling stacking
        orientation: { axis: "top", item: "top" },
        multiselect: true,
        editable: { updateTime: true, updateGroup: true }
    };

    onMount(() => {
        // Create timeline
        timeline = new Timeline(container, data, options);
        timeline.addCustomTime(new Date());

        // Add event listener
        timeline.on("timechange", (event) => {
            customTimeElement.innerHTML = "Custom Time: " + event.time;

            const item = data.get(1);
            if (event.time > item.start) {
                item.end = new Date(event.time);
                const now = new Date();

                if (event.time < now) {
                    item.content = "Dynamic event (past)";
                    item.className = "past";
                } else if (event.time > now) {
                    item.content = "Dynamic event (future)";
                    item.className = "future";
                } else {
                    item.content = "Dynamic event (now)";
                    item.className = "now";
                }

                data.update(item);
            }
        });

        // Set custom range from -2 minute to +3 minutes current time
        const start = new Date(new Date().getTime() - 2 * 60 * 1000);
        const end = new Date(new Date().getTime() + 3 * 60 * 1000);
        timeline.setWindow(start, end, { animation: false });

        // Cleanup
        return () => {
            if (timeline) {
                timeline.destroy();
            }
        };
    });
</script>

<p style="width: 600px">
    When the custom time bar is shown, the user can drag this bar to a specific
    time. The Timeline sends an event that the custom time is changed, after
    which the contents of the timeline can be changed according to the specified
    time in past or future.
</p>

<div bind:this={customTimeElement} id="customTime">&nbsp;</div>
<p></p>

<div bind:this={container} id="visualization"></div>

<style>
    body {
        font: 11pt verdana;
    }

    .vis.timeline .item.past {
        /* filter: alpha(opacity=50); */
        opacity: 0.5;
    }
</style>
