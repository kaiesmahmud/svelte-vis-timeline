<script>
	import { DataSet, DataView, Queue } from "vis-data";
	import { Timeline } from "vis-timeline";
</script>

<main>
	<h1>Timeline Drag &amp; Drop Example</h1>

	<p>
		For this to work, you will have to define your own
		<code>'dragstart'</code> eventListener on each item in your list of
		items (make sure that any new item added to the list is attached to this
		eventListener 'dragstart' handler). This 'dragstart' handler must set
		<code>dataTransfer</code> - notice you can set the item's information as
		you want this way.
	</p>

	<div id="visualization"></div>

	<div class="items-panel">
		<div class="side">
			<h3>Items:</h3>
			<ul class="items">
				<li draggable="true" class="item">item 1 - box</li>
				<li draggable="true" class="item">item 2 - point</li>
				<li draggable="true" class="item">item 3 - range</li>
				<li draggable="true" class="item">
					item 3 - range - fixed times - <br />
					(start: now, end: now + 10 min)
				</li>
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
