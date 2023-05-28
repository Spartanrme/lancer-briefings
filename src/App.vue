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
      "mission_slug": "011",
      "current_md": "",
      "events": events.events,
      "missions": [
        {
            "slug": "001",
            "name": "GREEN BOOT",
            "status": "success"
        },
        {
            "slug": "002",
            "name": "DOUSED FORGE",
            "status": "partial-success"
        },
        {
            "slug": "003",
            "name": "SHIFTED SOVEREIGN",
            "status": "success"
        },
        {
            "slug": "004",
            "name": "SILVER RUSH",
            "status": "success"
        },
        {
            "slug": "005",
            "name": "BOILING SHIFT",
            "status": "success"
        },
        {
            "slug": "006",
            "name": "WRATHFUL QUIET",
            "status": "success"
        },
        {
            "slug": "007",
            "name": "PILGRIM'S PATH",
            "status": "success"
        },
        {
            "slug": "008",
            "name": "KINGDOM'S KEYS",
            "status": "success"
        },
        {
            "slug": "009",
            "name": "QUARRY DREAM",
            "status": "success"
        },
        {
            "slug": "010",
            "name": "TRIBAL TECTONICS",
            "status": "success"
        },
        {
            "slug": "011",
            "name": "LACERATED SARCOPHAGUS",
            "status": "start"
        }

      ],
      "pilots": [
        {
            "callsign": "Architect",
            "alias": "Victor Blayse",
            "code": "c61b1535-1fd0-4af0-98d6-dce39f0c21f1//NDL-C-TANGENT-HIDE",
            "corpro": "SSC",
            "frame": "Orchis",
            "mech": "Valiant_Saber",
            "status": "Active"
        },
        {
            "callsign": "Einsamjäger",
            "alias": "Sophia Winterschwert",
            "code": "43b7bc7f-ac17-4b12-8d83-1af33ce4cbf4//NDL-C-LUNAR-HELIX",
            "corpro": "SSC",
            "frame": "MOURNING CLOAK",
            "mech": "Endlose_Nacht",
            "status": "Active"
        },
        {
            "callsign": "Panther",
            "alias": "Louise Muselli",
            "code": "47216c81-488d-449a-a3ce-188b94bfcf32//NDL-C-BRONZE-CAGE",
            "corpro": "SSC",
            "frame": "EMPEROR",
            "mech": "Nightingale",
            "status": "Active"
        },
        {
            "callsign": "Swipe",
            "alias": "Cincin Becerra",
            "code": "a0d6b0d4-867d-496f-b470-20bd3ad65bd5//NDL-C-DELTA-TEMPLE",
            "corpro": "HORUS",
            "frame": "PEGASUS",
            "mech": "Pandemonium",
            "status": "Active"
        },
        {
            "callsign": "Thunderclap",
            "alias": "Juniper \"June\" Crathis",
            "code": "3c33258e-a674-4aaf-b0d4-af95e72212ae//NDL-C-BLIND-FEW",
            "corpro": "HORUS",
            "frame": "LICH",
            "mech": "Cipher",
            "status": "Active"
        },
        {
            "callsign": "Sync",
            "alias": 'Rebecca',
            "code": "b71315de-31ad-43cf-8273-27fb989b6a09//NDL-C-NULL-ASH",
            "corpro": "SSC",
            "frame": "ATLAS",
            "mech": "Forte",
            "status": "Inactive"
        },
        {
            "callsign": "Battery",
            "alias": "Martin Snyder",
            "code": "REDACTED",
            "corpro": "IPS-N",
            "frame": "TORTUGA",
            "mech": "House_Call",
            "status": "AWOL"
        }
      ],
      "header": {
        "planet": "Perax-1",
        "year": "5016u",
        "month": "June",
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

          var eventArr = Object.values(events.events).slice();
          eventArr.pop(); // Getting rid of the placeholder event (id == -1)
          eventArr.reverse();

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
