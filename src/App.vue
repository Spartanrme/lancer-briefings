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
                  <input style="margin-left:10px;" type="checkbox" id="articleCheckbox" v-model="checked"><label for="articleCheckbox">SHOW ONLY ARTICLES</label><br>
              </div>
              <div v-if="!checked">
                  <Event v-for="item in this.events.slice().reverse()" :key="item.slug" :event="item" />
              </div>
              <div v-else>
                  <Event v-for="item in this.events.slice().reverse().filter(item => item.clickable == true)" :key="item.slug" :event="item" />
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

export default {
  components: {
    Header,
    Footer,
    Mission,
    Event,
    Pilot,
    Markdown
  },

  data() {
    return {
        "mission_slug": "006",
        "current_md": "",
        "events": [
            {
                "name": "001",
                "date": "OCT 5015U",
                "type": "Headline",
                "title": "PRISON BREAK AND SHOOTOUT AT REDFORGE",
                "subtitle": "LUIS CORT PRIME SUSPECT",
                "source": "THE SURVEY SURVEY",
                "image": "HC.png",
                "clickable": false
            },
            {
                "name": "002",
                "date": "OCT 5015U",
                "type": "ALERT",
                "title": "REDFORGE UNDER LOCKDOWN AFTER TERRORIST ATTACKS",
                "subtitle": "HONG-COLEMAN SECURITY FORCES ENACTING CHECKPOINTS AT ALL INGRESS/EGRESS POINTS",
                "source": "CONSOLIDATED SRCS",
                "image": "ALERT.png",
                "clickable": false
            },
            {
                "name": "003",
                "date": "NOV 5015U",
                "type": "Article",
                "title": "HONG-COLEMAN DECLARES STATE OF EMERGENCY",
                "subtitle": "In a press release held by a Hong-Coleman spokesperson",
                "source": "THE SURVEY SURVEY",
                "image": "Explosion.png",
                "clickable": true
            },
            {
                "name": "006",
                "date": "NOV 5015U",
                "type": "ALERT",
                "title": "HC FLASH-FIRE CONSTELLATION OPERATIONAL AREA EXPANDING",
                "subtitle": "F2C ORBITAL SUPPORT ENCROACHING 50MI OUT CLEARDAY BORDER",
                "source": ".::ELINT//PROCESSED",
                "image": "ALERT.png",
                "clickable": false
            },
            {
                "name": "007",
                "date": "NOV 27 5015U",
                "type": "INFO",
                "title": "MERCURY-ONE CELEBRATES 20th ANNIVERSARY OF CONOP",
                "subtitle": "Silverton Families praise M-1 personnel as example to all people of Perax to the capabilites of humans on the frontier. ",
                "source": "THE M.O.O.N.",
                "image": "ringStationCelebration.png",
                "clickable": false
            },
            {
                "name": "008",
                "date": "NOV 28 5015U",
                "type": "INFO",
                "title": "PIRACY ON THE RISE",
                "subtitle": "Shipping delays expected as cargo ships traveling to the Perax system join into larger convoys in an effort to resist increased space piracy in the Perax system. ",
                "source": "THE M.O.O.N.",
                "image": "news.png",
                "clickable": false
            },
            {
                "name": "009",
                "date": "NOV 30 5015U",
                "type": "INFO",
                "title": "BSC STUDY REVEALS PATTERN OF INCREASING AGGRESSION OF NATIVE PERAX CREATURES",
                "subtitle": "New study by BrightStar scientists reveals increased aggression from native Perax fauna. The cause remains unknown, and the study suggests civilians not stray far from their settlements to best avoid an encounter with the native animals.",
                "source": "THE BRIGHTSTAR POST",
                "image": "bs.png",
                "clickable": false
            },
            {
                "name": "010",
                "date": "DEC 01 5015U",
                "type": "INFO",
                "title": "DELAYED HA MARINES ARRIVE IN PERAX, JOIN GROWING FLEET LED BY HAS \"VALIANT\"",
                "subtitle": "The stalled out ships of the HA 613th Legionary Marines were repaired today, thanks to logistical efforts from the HAS \"Valiant\" stationed in the outskirts of the Perax system. The marines have now joined with the growing HA fleet, alongside vast reserves of supplies that experts estimate will allow the fleet to maintain independent operations for several years.",
                "source": "The Survey Survey",
                "image": "hc.png",
                "clickable": false
            },
            {
                "name": "011",
                "date": "DEC 02 5015U",
                "type": "INFO",
                "title": "TITECH MINIMUM WAGE LAW FAILS TO PASS",
                "subtitle": "A law to increase the minimum wage in TiTech failed to pass today. The law's detractors stated that the proposal \"threatened the economic stability and longevity on Perax.\" The law's supporters are disappointed by the outcome, but remain resolute that other measures still under review will find more success.",
                "source": "The TiTechTimes",
                "image": "tt.png",
                "clickable": false
            },
            {
                "name": "012",
                "date": "DEC 03 5015U",
                "type": "INFO",
                "title": "TERRORISTS RANSOM SILVERTON'S DELIZA SILVESTRI, DEMANDS SILVESTRI SUPERCARRIER \"ILLUSTRIOUS\"",
                "subtitle": "The group responsible for the kidnapping of Deliza Silvestri, a prominent diplomatic figure in the Silverton Families, have made public ransom demands for the return of their captive. They have demanded the forfeiture of a Silvestri family supercarrier trade ship, the \"Illustrious\", in exchange for Deliza's safe return.",
                "source": "THE SILVER SENTRY",
                "image": "sf.png",
                "clickable": false
            },
            {
                "name": "013",
                "date": "DEC 03 5015U",
                "type": "INFO",
                "title": "SSC REACHES AGREEMENT WITH PUC, PLANS FOR CATALOG STORE ON M1",
                "subtitle": "The Smith-Shimano Corpro has concluded negotiations within the PUC to expand their business in the Perax system. The deal will allow SSC to open a storefront in Mercury One's Silver Ring, and permits them expanded trade allowances, in order to market their exclusive catalog of products to the Perax system.",
                "source": "THE M.O.O.N.",
                "image": "news.png",
                "clickable": false
            },
            {
                "name": "014",
                "date": "DEC 05 5015U",
                "type": "INFO",
                "title": "CLEARDAY REJECTS HA OFFER OF CHARITY",
                "subtitle": "Harrison Armory announced a program of providing relief aid to Clearday, supplying them with food, medicine, and clothes. Clearday officials are currently denying the Armory's offer, however, and informing their citizens to reject or destroy Armory supply drops.",
                "source": "The New Dawn Gazette",
                "image": "cd.png",
                "clickable": false
            },
            {
                "name": "015",
                "date": "DEC 07 5015U",
                "type": "INFO",
                "title": "SF ANNOUNCES LT. COL. DE MELO TO LEAD NEW SPECIAL TASK FORCE",
                "subtitle": "A press conference was held in the Silverton Families today to announce the creation of a special task force, headed by recent heroine Lt. Col. de Melo, for the purpose of protecting and securing Silverton Family assets on Perax against hostile action. Lt. Col. de Melo stated that her team would comprise \"the best the Silverton Families have to offer\", and would show \"no hesitation\" to act against any and all threats to the Silverton Family's colony.",
                "source": "THE SILVER SENTRY",
                "image": "sf.png",
                "clickable": false
            },
            {
                "name": "016",
                "date": "DEC 08 5015U",
                "type": "INFO",
                "title": "ENTREPRENEUR SAUL DRAKE BEGINS FORMATION OF \"PERAX TRADERS' COALITION\"",
                "subtitle": "Saul Drake, an enterprising businessman from Titanics Technologies, has put into motion plans to create the \"Perax Traders' Coalition.\" The Coalition is meant to bring together traders and merchants across Perax, and provide an independent network for distributing goods and negotiating trade deals with all Perax colonies, free of allegiance to any one government.",
                "source": "The TiTechTimes",
                "image": "tt.png",
                "clickable": false
            },
            {
                "name": "017",
                "date": "DEC 10 5015U",
                "type": "INFO",
                "title": "HC JOURNALISTS UNCOVER BOMBING PLOT IN GAODAN, TERRORISTS ARRESTED, POSSIBLE LINK TO CLEARDAY",
                "subtitle": "A field investigator of the Hong-Coleman Ministry of Journalism uncovered a planned terrorist bombing of several civilian structures in the recently operational Gaodan mines near Clearday. Hong-Coleman security teams cleared the building being used to organize this act of terrorism, and were able to dismantle the operation in the ensuing firefight. Evidence suggests the terrorists were Clearday citizens who were inflamed by the construction of the Gaodan mines. Clearday leaders claim no involvement in the planned attacks, stating that the citizens were acting independently, and demand the return of the arrested terrorists so they may stand trial.",
                "source": "The Survey Survey",
                "image": "hc.png",
                "clickable": false
            },
            {
                "name": "018",
                "date": "DEC 11 5015U",
                "type": "INFO",
                "title": "GAODAN BOMBING THREAT PROMPTS FORMATION OF PERAX MISSILE INTERCEPT FORCE",
                "subtitle": "The recently thwarted terrorist attack in Gaodan has prompted the Perax United Colonies members to unanimously agree to TiTech's proposed \"Perax Missile Intercept Force.\" The PMIF will be staffed, funded, and supplied by members from all of Perax's colonies, for the sole purpose of monitoring missile launches from low-orbit or the colony surface, and to disable any launch determined to threaten the Union Utopian Pillars.",
                "source": "THE TiTechTimes",
                "image": "tt.png",
                "clickable": false
            },
            {
                "name": "019",
                "date": "DEC 13 5015U",
                "type": "INFO",
                "title": "WALFORD SHOCKED BY NEARBY ARTILLERY FIRE, HC CLAIMS TRAINING EXERCISES",
                "subtitle": "Favor turns against Hong-Coleman among the Perax colonies after a shocking story was made public revealing the extent of their artillery line being established along their border with Titanics Technologies. The article, which was made by a joint effort between TiTech journalists and the Clearday \"New Dawn Gazette\", shows Hong-Coleman artillery fire being launched just outside of Walford's city limits, illuminating the dark night with flashes of detonating shells. Hong-Coleman has stated they were conducting a \"live-fire test\" to ensure the safety of their military personnel, and have denied further comment.",
                "source": "The TiTechTimes",
                "image": "tt.png",
                "clickable": false
            },
            {
                "name": "020",
                "date": "DEC 24 5015U",
                "type": "INFO",
                "title": "IPS-N Contracted to Protect Interstellar Trade Routes in Response to Increasing Piracy",
                "subtitle": "",
                "source": "THE M.O.O.N.",
                "image": "news.png",
                "clickable": false
            },
            {
                "name": "021",
                "date": "DEC 25 5015U",
                "type": "INFO",
                "title": "Shoushan Han Grants Official Pardon to Onodera Oniwa, Welcomed Back Into Service",
                "subtitle": "",
                "source": "The Survey Survey",
                "image": "hc.png",
                "clickable": false
            },
            {
                "name": "022",
                "date": "DEC 26 5015U",
                "type": "INFO",
                "title": "Smith-Shimano Enters Talks with Silverton Families, Plans for Perax Outpost Colony",
                "subtitle": "",
                "source": "THE SILVER SENTRY",
                "image": "sf.png",
                "clickable": false
            },
            {
                "name": "023",
                "date": "DEC 27 5015U",
                "type": "INFO",
                "title": "Silverton Families Enact \"Perax Galactic Outreach Protocols and Procedures\" Act, TiTech's Roman Castillo Decries as Violation of Perax Foreign Affairs Accords",
                "subtitle": "",
                "source": "The TiTechTimes",
                "image": "tt.png",
                "clickable": false
            },
            {
                "name": "024",
                "date": "DEC 30 5015U",
                "type": "INFO",
                "title": "BrightStar's Rokurou Masuda and Hong-Coleman's Shoushan Han Hold Private Meeting, Rumors of BrightStar Entering Escalating Conflict on Perax",
                "subtitle": "",
                "source": "THE NEW DAWN GAZETTE",
                "image": "cd.png",
                "clickable": false
            },
            {
                "name": "025",
                "date": "JAN 1 5016U",
                "type": "INFO",
                "title": "\"Planet Pacifier\" Mohammad Ashok Joins Harrison Armory 613th Fleet, Claims Imminent Beast Hunts as Charitable Service to Colonies",
                "subtitle": "",
                "source": "THE M.O.O.N.",
                "image": "news.png",
                "clickable": false
            }
          ],
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
            "alias": "Martin",
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
            "frame": "SWALLOWTAIL",
            "mech": "Firewall"
        },
        {
            "callsign": "Swipe",
            "alias": "Cincin Becerra",
            "code": "8A918E8F-1A29-58EA-9304-32253CE40E06///VOIDSTORM-PMC",
            "corpro": "HA",
            "frame": "BARBAROSSA",
            "mech": "Blammo"
        },
        {
            "callsign": "Sync",
            "alias": 'Rebecca',
            "code": "880FC45D-67B7-5EBF-A507-C965499E13FC///VOIDSTORM-PMC",
            "corpro": "IPS-N",
            "frame": "BLACKBEARD",
            "mech": "Fortissimo"
        },
      ],
      "header": {
        "planet": "Perax-1",
        "year": "5016u",
        "month": "January",
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
      "checked" : false
    }
  },

  created() {
    this.loadMissionMarkdown()
  },

  computed: {

  },

  methods: {
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
