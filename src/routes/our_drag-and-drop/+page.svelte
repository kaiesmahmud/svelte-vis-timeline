<script>
	import { DataSet, DataView, Queue } from "vis-data";
	import { Timeline } from "vis-timeline";
	import { onMount } from "svelte";
	import "vis-timeline/styles/vis-timeline-graph2d.css";

	let timeline1;
	let container;

	const groups = new DataSet();
	const items = new DataSet();

	let todayStart = new Date().setHours(0, 0, 0, 0);
	let todayEnd = new Date().setHours(4, 0, 0, 0);

	const options = {
		stack: true,
		start: todayStart, // Start at midnight today
		end: todayEnd, // End at the last minute of today
		min: todayStart,
		max: new Date().setHours(20, 0, 0, 0),
		zoomMin: 1000 * 60 * 1, // Minimum zoom to 1 hour
		zoomMax: 1000 * 60 * 60 * 60, // Maximum zoom to 12 hours
		editable: true,
		orientation: "top",
		onDropObjectOnItem: (objectData, item, callback) => {
			if (!item) return;
			alert(
				`dropped object with content: "${objectData.content}" to item: "${item.content}"`,
			);
		},
		moveable: true,
		zoomable: true,
		horizontalScroll: true,
		// stack: false, // if items overlap, try disabling stacking
		orientation: { axis: "top", item: "top" },
		multiselect: true,
		onMoving: (item, callback) => {
			console.log("onMoving", item);
			if(item.dataType==groups.get(item.group).dataType){
				// Adding a backup of the group reference
				item.lastGroup=item.group;
			}
			else{
				item.group=item.lastGroup;
			}
			callback(item);
		},
		// Customize the time axis to show only hours
		format: {
			minorLabels: minorLabels,
			majorLabels: majorLabels,
		},
		onAdd: (item, callback) => {
			if(item.dataType==groups.get(item.group).dataType){
				// Adding a backup of the group reference
				item.lastGroup=item.group;
				callback(item);
			}
			
		}
		// editable: { updateTime: true, updateGroup: true }
	};

	function minorLabels(date, scale, step) {
		switch (scale) {
			case "millisecond":
				return date.format("HH:mm:ss");
			case "second":
				return date.format("HH:mm:ss");
			case "minute":
				return date.format("HH:mm");
			case "hour":
				return date.format("HH:mm");
			case "weekday":
				return date.format("ddd D MMMM");
			case "day":
				return date.format("ddd D MMMM");
			case "week":
				return date.format("ddd D MMMM");
			case "month":
				return date.format("MMMM YYYY");
			case "year":
				return date.format("YYYY");
		}
	}
	function majorLabels(date, scale, step) {
		switch (scale) {
			case "millisecond":
				return date.format("HH:mm:ss");
			case "second":
				return date.format("HH:mm:ss");
			case "minute":
				return date.format("HH:mm");
			case "hour":
				return date.format("HH:mm");
			case "weekday":
				return date.format("ddd D MMMM");
			case "day":
				return date.format("ddd D MMMM");
			case "week":
				return date.format("ddd D MMMM");
			case "month":
				return date.format("MMMM YYYY");
			case "year":
				return date.format("YYYY");
		}
	}

	const handleDragStart = (event) => {
		event.dataTransfer.effectAllowed = "move";
		const item = {
			id: new Date(),
			type: "range",
			content: event.target.innerHTML,
			dataType:event.target.title
		};
		
		event.dataTransfer.setData("text", JSON.stringify(item));
		console.log(event.target.title)
	};

	const handleObjectItemDragStart = (event) => {
		event.dataTransfer.effectAllowed = "move";
		const objectItem = {
			content: "objectItemData",
			target: "item",
		};
		event.dataTransfer.setData("text", JSON.stringify(objectItem));
	};

	onMount(() => {
		// Initialize timeline
		timeline1 = new Timeline(container, items, groups, options);
		// Add drag event listeners
		const itemElements = document.querySelectorAll(".items .item");
		const objectItems = document.querySelectorAll(".object-item");

		itemElements.forEach((item) => {
			item.addEventListener("dragstart", handleDragStart);
		});

		objectItems.forEach((objectItem) => {
			objectItem.addEventListener("dragstart", handleObjectItemDragStart);
		});

		// Cleanup function
		return () => {
			if (timeline1) {
				timeline1.destroy();
			}
		};
	});
	// Functions to add tracks
	function addAudioTrack() {
		// Add audio track logic
		groups.add({
			id: `${groups.length}`,
			content: `Audio Track ${groups.length}`,
			dataType: "audio"
		})
	}
	function addImageTrack() {
		// Add image track logic
		groups.add({
			id: `${groups.length}`,
			content: `Image Track ${groups.length}`,
			dataType: "image"
		})
	}
	function addTextTrack() {
		// Add text track logic
		groups.add({
			id: `${groups.length}`,
			content: `Text Track ${groups.length}`,
			dataType: "text"
		})
	}
</script>

<main>
	<h1>Timeline Drag & Drop Example</h1>
	<button on:click={addAudioTrack}>Add Audio Track</button>
	<button on:click={addImageTrack}>Add Image Track</button>
	<button on:click={addTextTrack}>Add Text Track</button>

	<div bind:this={container} id="visualization"></div>

	<div class="items-panel">
		<div class="side">
			<h3>Items:</h3>
			<ul class="items">
				<li draggable="true" class="item" title="audio">Audio Item</li>
				<li draggable="true" class="item" title="image">Image Item</li>
				<li draggable="true" class="item" title="text">Text Item</li>
			</ul>
		</div>

		<div class="side">
			<h3>Object with "target:'item'":</h3>
			<li draggable="true" class="object-item">
				object with target:'item'
			</li>
		</div>
	</div>
</main>

<style>
	/* Add your CSS styles here */
	.items-panel {
		display: flex;
		gap: 2rem;
	}

	.side {
		flex: 1;
	}

	.items {
		list-style-type: none;
		padding: 0;
	}

	.item,
	.object-item {
		cursor: move;
		margin-bottom: 0.5rem;
		padding: 0.5rem;
		background: #f5f5f5;
		border: 1px solid #ddd;
		border-radius: 4px;
	}

	li.item {
		list-style: none;
		width: 150px;
		color: #1a1a1a;
		background-color: #d5ddf6;
		border: 1px solid #97b0f8;
		border-radius: 2px;
		margin-bottom: 5px;
		padding: 5px 12px;
	}
	li.item:before {
		content: "≣";
		font-family: Arial, sans-serif;
		display: inline-block;
		font-size: inherit;
		cursor: move;
	}
	li.object-item {
		list-style: none;
		width: 150px;
		color: #1a1a1a;
		background-color: #d5ddf6;
		border: 1px solid #97b0f8;
		border-radius: 2px;
		margin-bottom: 5px;
		padding: 5px 12px;
	}
	li.object-item:before {
		content: "≣";
		font-family: Arial, sans-serif;
		display: inline-block;
		font-size: inherit;
		cursor: move;
	}
	.items-panel {
		display: flex;
		justify-content: space-around;
	}
</style>
