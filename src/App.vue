<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Mission Log</h1>
      </div>
      <div class="section-content-container">
        <h3>Current Assignment</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Mission List</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "003",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001",
          "name": "REDACTED",
          "status": "success"
          },
        {
          "slug": "002",
          "name": "INFILTRATE",
            "status": "partial-success"
          },
        {
          "slug": "003",
          "name": "RETRIEVE",
          "status": "start"
          },
      ],
      "pilots": [
        {
          "callsign": "Architect",
          "alias": "Victor Blayse",
          "code": "BF5D1D96-BB30-5913-B1C1-A666AB76C1AE///VOIDSTORM-PMC///B5AEF9C6-CC60-5C42-839C-BCEB26033B88",
          "corpro": "IPS-N",
          "frame": "VLAD",
          "mech": "Esperanza_Invicta"
        },
        {
          "callsign": "Battery",
          "alias": "Martin",
          "code": "0136BD17-316E-5A41-B1E3-460AA0AC09F9///VOIDSTORM-PMC///E856DB77-99E2-545F-99EA-F77995E11E1A",
          "corpro": "IPS-N",
          "frame": "TORTUGA",
          "mech": "House_Call"
        },
        {
          "callsign": "Panther",
          "alias": "Louise Muselli",
          "code": "067E192D-2DA6-5889-8496-C8D1BA5D0DDB///VOIDSTORM-PMC///BB6B6D8E-5B38-5C14-ACEF-2F4E231E11F8",
          "corpro": "SSC",
          "frame": "SWALLOWTAIL",
          "mech": "Firewall"
        },
        {
          "callsign": "Swipe",
          "alias": "Cincin Becerra",
          "code": "8A918E8F-1A29-58EA-9304-32253CE40E06///VOIDSTORM-PMC///06953251-C16A-5E5F-AE23-65B8F3C4840E",
          "corpro": "HA",
          "frame": "BARBAROSSA",
          "mech": "Edd"
        },
        {
          "callsign": "Sync",
          "alias": 'Rebecca',
          "code": "880FC45D-67B7-5EBF-A507-C965499E13FC///VOIDSTORM-PMC///B00A236A-57AD-54BD-835F-19942C91F3FA",
          "corpro": "IPS-N",
          "frame": "NELSON",
          "mech": "Crescendo"
        },
      ],
      "header": {
        "planet": "Perax-1",
        "year": "5015u",
        "month": "November",
        "system": "Perax",
        "gate": "M'Goun",
        "ring": "ATLAS",
        "headerTitle": "Voidstorm",
        "headerSubtitle": "Mercenary Company",
        "subheaderTitle": "Bravo Team",
        "subheaderSubtitle": "Storm Squadron",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>
