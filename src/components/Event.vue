<template>
	<div v-if="event.clickable" class="event-wrapper" @click="eventModal">
		<div style="min-height: 100px; height: 100%;" class="event">
			<img :src="eventPortrait" class="event-portrait" />
			<div class="event-body">
				<div class="name">
					<h2>{{ event.title }}</h2>
					<h1>{{event.type}}//{{ event.date }}</h1>
					<h1><i>FROM {{event.source}}</i></h1>
					<h1>{{ event.subtitle }}...</h1>
					<h1><strong>(Read more...)</strong></h1>
				</div>
			</div>
		</div>
		<div v-if="event.clickable" class="middle">
			<div class="text">&nbsp;</div>
		</div>
	</div>
	<div v-else class="event-wrapper">
		<div style="min-height: 100px; height: 100%;" class="event">
			<img :src="eventPortrait" class="event-portrait" />
			<div class="event-body">
				<div class="name">
					<h2>{{ event.title }}</h2>
					<h1>{{event.type}}//{{ event.date }}</h1>
					<h1><i>FROM {{event.source}}</i></h1>
					<h1>{{ event.subtitle }}</h1>
				</div>
			</div>
		</div>
		<div v-if="event.clickable" class="middle">
			<div class="text">&nbsp;</div>
		</div>
	</div>
</template>

<script>

import EventModal from './EventModal.vue';

export default {
	components: {
		EventModal
	},
	props: {
		event: {
			type: Object,
			required: true,
		}
	},
	computed: {
		eventPortrait() {
			return `/eventImages/${this.event.image}`
		},
	},
	methods: {
		eventModal() {
			this.$oruga.modal.open({
				parent: this,
				component: EventModal,
				custom: true,
				trapFocus: true,
				props: {
					event: this.event,
				},
				class: "TEST",
				width: 1920,
			});
		},
	}
}
</script>