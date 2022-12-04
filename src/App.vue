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
    <section class="section-container" id="events" style="width:545px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
          <div class="event-list-container">
              <div class="fieldgroup">
                  <label for="articleMonth">MONTH: </label>
                  <form autocomplete="off">
                      <select style="margin-left:10px;" id="articleMonth" name="articleMonth" v-model="monthSelected">
                          <option disabled>Select One</option>
                          <option value="0">All</option>
                          <option value="JAN">Janurary</option>
                          <option value="FEB">Feburary</option>
                          <option value="MAR">March</option>
                          <option value="APR">April</option>
                          <option value="MAY">May</option>
                          <option value="JUN">June</option>
                          <option value="JUL">July</option>
                          <option value="AUG">August</option>
                          <option value="SEP">September</option>
                          <option value="OCT">October</option>
                          <option value="NOV">November</option>
                          <option value="DEC">December</option>
                      </select>
                      <label for="articleMonth">  YEAR: </label>
                      <select style="margin-left:10px;" id="articleYear" name="articleYear" v-model="yearSelected">
                          <option disabled>Select One</option>
                          <option value="0">All</option>
                          <option value="5015U">5015U</option>
                          <option value="5016U">5016U</option>
                      </select>
                      <label for="showNew">  SHOW: </label>
                      <select style="margin-left:10px;" id="showNew" name="showNew" v-model="showSelected">
                          <option disabled>Select One</option>
                          <option value="0">All</option>
                          <option value="1" selected>ONLY NEW</option>
                      </select>
                  </form>
              </div>
              <div>
                  <Event v-for="item in eventFilter(monthSelected, yearSelected, showSelected)" :key="item.slug" :event="item" />
              </div>
          </div>
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:620px; height:714px;">
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
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Event from './components/Event.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';
import events from './components/events.json'

export default {
  components: {
    Header,
    Footer,
    Mission,
    Event,
    Pilot,
    Markdown,
    events
  },

  data() {
    return {
      "mission_slug": "008",
      "current_md": "",
      "events": events.events,
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
            "status": "success"
        },
        {
            "slug": "004",
            "name": "EXTRACT",
            "status": "success"
        },
        {
            "slug": "005",
            "name": "DISTRUPT",
            "status": "success"
        },
        {
            "slug": "006",
            "name": "EXTERMINATE",
            "status": "success"
        },
        {
            "slug": "007",
            "name": "ESCORT",
            "status": "success"
        },
        {
            "slug": "008",
            "name": "SUPPORT",
            "status": "start"
        }
      ],
      "pilots": [
        {
            "callsign": "Architect",
            "alias": "Victor Blayse",
            "code": "BF5D1D96-BB30-5913-B1C1-A666AB76C1AE///VOIDSTORM-PMC",
            "corpro": "IPS-N",
            "frame": "ZHENG",
            "mech": "Esperanza_Invicta"
        },
        {
            "callsign": "Battery",
            "alias": "Martin Snyder",
            "code": "0136BD17-316E-5A41-B1E3-460AA0AC09F9///VOIDSTORM-PMC",
            "corpro": "IPS-N",
            "frame": "TORTUGA",
            "mech": "House_Call"
        },
        {
            "callsign": "Panther",
            "alias": "Louise Muselli",
            "code": "067E192D-2DA6-5889-8496-C8D1BA5D0DDB///VOIDSTORM-PMC",
            "corpro": "SSC",
            "frame": "EMPORER",
            "mech": "Nightingale"
        },
        {
            "callsign": "Swipe",
            "alias": "Cincin Becerra",
            "code": "8A918E8F-1A29-58EA-9304-32253CE40E06///VOIDSTORM-PMC",
            "corpro": "HA",
            "frame": "PEGASUS",
            "mech": "Kachunka"
        },
        {
            "callsign": "Sync",
            "alias": 'Rebecca',
            "code": "880FC45D-67B7-5EBF-A507-C965499E13FC///VOIDSTORM-PMC",
            "corpro": "IPS-N",
            "frame": "ATLAS",
            "mech": "Forte"
        },
      ],
      "header": {
        "planet": "Perax-1",
        "year": "5016u",
        "month": "Feburary",
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
      },
      "monthSelected": 0,
      "yearSelected": 0,
      "showSelected": 1
    }
  },

  created() {
    this.loadMissionMarkdown()
  },

  computed: {

  },

  methods: {
      eventFilter(monthSelected, yearSelected, showSelected) {
        var eventArr = Object.values(events.events).slice().reverse();
        if (showSelected == 1) {
            eventArr = eventArr.filter(x => x.new == true);
        } else eventArr.filter(x => x.new == false);

        if (monthSelected != 0 && yearSelected != 0) {
            var byYear = eventArr.filter(x => x.date.includes(yearSelected));
            return byYear.filter(y => y.date.startsWith(monthSelected));
        }
        if (monthSelected != 0) {
            return eventArr.filter(x => x.date.startsWith(monthSelected));
        }
        if (yearSelected != 0) {
            return eventArr.filter(x => x.date.includes(yearSelected));
        }
        return eventArr;
    },
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
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
