<script>
    import { onMount } from "svelte";
    import { DataSet } from "vis-data";
    import { Timeline } from "vis-timeline";
    import moment from "moment";

    let container;
     
    let sdt = [
        {
            group3: [
                {
                    id: 1243,
                    treeLevel: 3,
                    content: "Level 3 1243",
                },
                {
                    id: 1525,
                    treeLevel: 3,
                    content: "Level 3 1525",
                },
                {
                    id: 1624,
                    treeLevel: 3,
                    content: "Level 3 1624",
                },
                {
                    id: 2076,
                    treeLevel: 3,
                    content: "Level 3 2076",
                },
                {
                    id: 1345,
                    treeLevel: 3,
                    content: "Level 3 1345",
                },
                {
                    id: 2078,
                    treeLevel: 3,
                    content: "Level 3 2078",
                },
                {
                    id: 1826,
                    treeLevel: 3,
                    content: "Level 3 1826",
                },
                {
                    id: 2107,
                    treeLevel: 3,
                    content: "Level 3 2107",
                },
            ],
            groups: [
                {
                    id: 10,
                    title: "Group 10",
                    content: "Group 10",
                    treeLevel: 1,
                    nestedGroups: [1, 2, 3, 4, 5, 6],
                },
                {
                    id: 1,
                    content: "North America",
                    treeLevel: 2,
                    nestedGroups: [
                        1243, 1525, 1624, 1345, 2078, 1826, 2076, 2107,
                    ],
                },
                {
                    id: 2,
                    treeLevel: 2,
                    content: "Latin America",
                },
                {
                    id: 3,
                    treeLevel: 2,
                    content: "Europe",
                },
                {
                    id: 4,
                    treeLevel: 2,
                    content: "Asia",
                },
                {
                    id: 5,
                    treeLevel: 2,
                    content: "Oceania",
                },
                {
                    id: 6,
                    treeLevel: 2,
                    content: "Africa",
                },
                {
                    id: 100,
                    title: "Group 100",
                    content: "Group 100",
                    treeLevel: 1,
                    nestedGroups: [101, 102, 103, 104, 105, 106],
                    text: "Totals",
                    EditionId: 0,
                    groupId: 0,
                },
                {
                    id: 101,
                    treeLevel: 2,
                    content: "North America",
                },
                {
                    id: 102,
                    treeLevel: 2,
                    content: "Latin America",
                },
                {
                    id: 103,
                    treeLevel: 2,
                    content: "Europe",
                },
                {
                    id: 104,
                    treeLevel: 2,
                    content: "Asia",
                },
                {
                    id: 105,
                    treeLevel: 2,
                    content: "Oceania",
                },
                {
                    id: 106,
                    treeLevel: 2,
                    content: "Africa",
                },
            ],
        },
    ];
    function randomIntFromInterval(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }

    onMount(() => {
        const startDay = moment()
            .startOf("month")
            .startOf("week")
            .isoWeekday(1);
        const now = moment().minutes(0).seconds(0).milliseconds(0);
        const itemCount = 60;

        // Create groups
        const groups = new DataSet();
        groups.add(sdt[0].groups);
        groups.add(sdt[0].group3);

        // Create items
        const items = new DataSet();
        const groupIds = groups.getIds();
        const types = ["box", "point", "range", "background"];

        for (let i = 0; i < itemCount; i++) {
            const rInt = randomIntFromInterval(1, 30);
            const start = startDay.clone().add(rInt, "days");
            const end = start.clone().add(24, "hours");
            const randomGroupId =
                groupIds[randomIntFromInterval(0, groupIds.length - 1)];
            const type = types[randomIntFromInterval(0, 3)];

            items.add({
                id: i,
                group: randomGroupId,
                content: "item " + i + " " + rInt,
                start: start,
                end: end,
                type: type,
            });
        }

        // Timeline options
        const options = {
            start: startDay.toDate(),
            end: new Date(1000 * 60 * 60 * 24 + new Date().valueOf()),
            horizontalScroll: true,
            zoomKey: "ctrlKey",
            orientation: "both",
            zoomMin: 1000 * 60 * 60 * 240,
        };

        // Create timeline
        new Timeline(container, items, groups, options);
    });
</script>

<p>
    This example demonstrates nested groups that are 3 levels deep. Note that a
    DataSet is used for both items and groups, allowing to dynamically add,
    update or remove both items and groups via the DataSet.
  </p>
  
  <div class="container" bind:this={container} />

<style>
    .container{
        box-sizing: border-box;
        width: 100%;
        height: 300px;
    }
</style>
