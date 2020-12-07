<template>
  <md-app>
    <md-app-toolbar class="md-primary" md-elevation="0">
      <md-button class="md-icon-button" @click="toggleMenu" v-if="!menuVisible">
        <md-icon>menu</md-icon>
      </md-button>
      <span class="md-title">OpenSky</span>
      <div class="profile flex ml-3"><span>Hi {{account.user.firstName}}!</span><span class="mx-2"> <router-link  to="/login">Logout</router-link></span></div>
    </md-app-toolbar>

    <md-app-drawer :md-active.sync="menuVisible" md-persistent="mini">
      <md-toolbar class="md-transparent" md-elevation="0">
        <span>Navigation</span>

        <div class="md-toolbar-section-end">
          <md-button class="md-icon-button md-dense" @click="toggleMenu">
            <md-icon>keyboard_arrow_left</md-icon>
          </md-button>
        </div>
      </md-toolbar>

      <md-list>
        <md-list-item>
          <md-icon>move_to_inbox</md-icon>
          <span class="md-list-item-text">Inbox</span>
        </md-list-item>

        <md-list-item>
          <md-icon>send</md-icon>
          <span class="md-list-item-text">Sent Mail</span>
        </md-list-item>

        <md-list-item>
          <md-icon>delete</md-icon>
          <span class="md-list-item-text">Trash</span>
        </md-list-item>

        <md-list-item>
          <md-icon>error</md-icon>
          <span class="md-list-item-text">Spam</span>
        </md-list-item>
      </md-list>
    </md-app-drawer>

    <md-app-content>
      <div class="grid-container">
        <div class="grid-item" v-for="card in cards" :key="card.index">
          <md-card>
            <md-card-header>
              <div class="md-title">{{ card.airport }}</div>
              <div class="md-subhead">{{ card.location }}</div>
            </md-card-header>

            <md-card-media>
              <img :src="card.imageUrl" alt="People" />
            </md-card-media>

            <md-card-content>
              {{ card.description }}
            </md-card-content>

            <md-card-actions>
              <md-button @click="toggleModal(card.icao24, card.logo, card.airport)">More +</md-button>
            </md-card-actions>
          </md-card>
        </div>
      </div>
    
    </md-app-content>
  </md-app>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  name: "Grid",
  data: function() {
    return {
      output: "",
      begin: "",
      end: "",
      icao: "",
      logo:"",
      airport:"",
      cards: [
        {
          index: "0",
          icao24: "KJFK",
          location: "New York, USA",
          airport: "Kennedy International Airport",
          imageUrl: "https://i.ibb.co/xj4J3Jn/jfk3.jpg",
          description:
            "John F. Kennedy International Airport is an international airport in Queens, New York, USA.",
          logo: "https://i.ibb.co/7NGfL9P/JFK-Airport-logo.png"
        },
        {
          index: "1",
          icao24: "RJTT",
          location: "Tokyo, Japan",
          airport: "Haneda Airport",
          imageUrl:
            "https://i.ibb.co/fXS7Z2y/0d466756b1ca9827eb989e6ed1513756-m.jpg",
          description:
            "Haneda was the primary international airport serving Tokyo until 1978; from 1978 to 2010",
          logo: "https://i.ibb.co/c8x3gRM/5790aa90a282a1f7cc64963624499d88.gif"
        },
        {
          index: "2",
          icao24: "ZSPD",
          location: "Shanghai, China",
          airport: "Pudong International Airport",
          imageUrl: "https://i.ibb.co/1T5pXXf/tour-img-1049540-146.jpg",
          description:
            "Shanghai Pudong International Airport is one of the two international airports in Shanghai.",
          logo: "https://i.ibb.co/7Wrh832/Shanghai-Airports.png"
        },
        {
          index: "3",
          icao24: "KLAX",
          location: "L.A, Carlifornia, USA",
          airport: "L.A International Airport",
          imageUrl:
            "https://i.ibb.co/WxpP2qw/1200px-Los-Angeles-International-Airport-Aerial-Photo.jpg",
          description:
            "Los Angeles International Airport, commonly referred to as LAX , is the primary international airport serving Los Angeles.",
          logo:
            "https://i.ibb.co/k9PhwKZ/1200px-Los-Angeles-Airport-logo-svg.png"
        },
        {
          index: "4",
          icao24: "LFPG",
          location: "Paris, France",
          airport: "Roissy Airport",
          imageUrl: "https://i.ibb.co/nLMrqWg/maxresdefault.jpg",
          description:
            "Paris Charles de Gaulle Airport, also known as Roissy Airport, is the largest international airport in France.",
          logo: "https://i.ibb.co/XFScDVh/paris.png"
        },
        {
          index: "5",
          icao24: "ZBAA",
          location: "Beijing, China",
          airport: "Capital International Airport",
          imageUrl: "https://i.ibb.co/tM3dmHw/capital.jpg",
          description:
            "Beijing Capital International Airport is the main international airport serving Beijing.",
          logo: "https://i.ibb.co/YBtszmm/capitallogo.jpg"
        },
        {
          index: "6",
          icao24: "KATL",
          location: "Atlanta,Georgia, USA",
          airport: "Atlanta International Airport",
          imageUrl: "https://i.ibb.co/64b1LXP/atlanta.jpg",
          description:
            "Hartsfield–Jackson Atlanta International Airport is the primary international airport serving Atlanta, Georgia.",
          logo: "https://i.ibb.co/g9L95V7/atlantalogo.png"
        },
        {
          index: "7",
          icao24: "KORD",
          location: "Chicago, USA",
          airport: "O'Hare International Airport",
          imageUrl:
            "https://i.ibb.co/vJgF4ww/Chicago-O-Hare-International-Airport-1.jpg",
          description:
            "O'Hare International Airport  is an international airport located on the Northwest Side of Chicago, USA.",
          logo:
            "https://i.ibb.co/44N6np7/302px-O-Hare-International-Airport-Logosvg.png"
        },
        {
          index: "8",
          icao24: "VTBS",
          location: "Bangkok, Thailand",
          airport: "Suvarnabhumi Airport",
          imageUrl:
            "https://i.ibb.co/3mhFKWY/imgonline-com-ua-Compress-To-Size-o5g-E6-Yqe-ZZnt7.jpg",
          description:
            "Suvarnabhumi Airportis one of two international airports serving Bangkok, Thailand.",
          logo:
            "https://i.ibb.co/cg9wvGh/319px-Suvarnabhumi-Airport-Logosvg.png"
        },
        {
          index: "9",
          icao24: "EGLL",
          location: "London, United Kingdom",
          airport: "Heathrow Airport",
          imageUrl:
            "https://i.ibb.co/QJvRTSz/blog-heathrow1-Belinda-Fewings-for-Unsplash.jpg",
          description:
            "Heathrow Airport, also known as London Heathrow, is a major international airport in London, United Kingdom.",
          logo: "https://i.ibb.co/vmf4FH2/382px-Heathrow-Logo-2013svg.png"
        }
      ],
      modalStatus: false,
      menuVisible: false,
     
    };
  },

 computed: {
        ...mapState({
            account: state => state.account,
            users: state => state.users.all
        })
    },
    created () {
        this.getAllUsers();
    },
  
  methods: {
    toggleModal: function(icao,logo,airport) {
      this.modalStatus = !this.modalStatus;
      this.icao = icao;
      this.logo =logo;
      this.airport = airport;
    },
    toggleMenu() {
      this.menuVisible = !this.menuVisible;
    },
     ...mapActions('users', {
            getAllUsers: 'getAll',
            deleteUser: 'delete'
        })
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
