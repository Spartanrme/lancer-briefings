<template>
    <div class="event-modal">
        <div class="e-header-container">
            <div class="section-header clipped-medium-backward-pilot">
                <img src="/icons/events-icon.svg" />
            </div>
            <div class="rhombus-back">&nbsp;</div>
        </div>
        <div class="event">
            <Markdown :source="bio" class="markdown" />
        </div>
    </div>
	<!--Window for the picture-->
    <!--<div class="event-modal news">
        <div class="e-header-container">
            <div class="section-header clipped-medium-backward-news">
                <img src="/icons/events-icon.svg" />
                <h1>Event</h1>
            </div>
            <div class="rhombus-back">&nbsp;</div>
        </div>
        <div class="event">
            <img :src="portrait" class="portrait" />
        </div>
    </div>-->
</template>

<script>
import Markdown from 'vue3-markdown-it';

export default {
	components: {
		Markdown
	},
	data() {
		return {
			bio: "",
		}
	},
	props: {
		event: {
			type: Object,
			required: true,
		}
	},
	computed: {
		portrait() {
			return `/eventImages/${this.event.id}.png`
		},
	},
	created() {
		let self = this;
		let md = `/events/${this.event.id}.md`
		var client = new XMLHttpRequest();
		client.open('GET', md);
		client.onreadystatechange = function () {
			self.bio = client.responseText;
		}
		client.send();
	},
	methods: {
		
	}
}
</script>