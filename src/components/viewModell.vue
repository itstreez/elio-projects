<template>
  <body>
    <!--  i used MVVM-->
    <div id="big-box">
      <div id="search-container">
        <input type="text" v-model="search" size="15" placeholder="Search .." />
      </div>
      <div id="conversation-list">
        <div
          v-on:click="display(user.id)"
          v-bind:key="user.id"
          v-for="user in filteredSearch"
          v-bind:class="{ conversation: current === user.id }"
          class="conversation"
          id="conversation"
        >
          <img v-bind:src="user.photo" />
          <div class="title-text">
            {{ user.name }}
          </div>
          <div class="created-date">
            {{ user.created }}
          </div>
          <div class="conversation-message">
            {{ user.message }}
          </div>
        </div>
      </div>
      <div id="page-of">
        <h2>
          <button v-on:click="increment" :disabled="true" value="<">
            &lt;
          </button>
        </h2>
        <h1>{{ counter }} of 1</h1>
        <h3>
          <button v-on:click="increment" :disabled="true" value=">">></button>
        </h3>
      </div>
      <div id="chat-title">
        <h1>{{ valtitle }}</h1>
      </div>
      <h3 id="lasts">{{ lasts }}</h3>

      <div id="chat-message-list">
        <h2 id="ms1" class="other1">{{ ms1 }}</h2>
        <h2 id="ms2" class="you1">{{ ms2 }}</h2>
        <h2 id="ms3" class="other2">{{ ms3 }}</h2>
        <h2 id="ms4" class="you2">{{ ms4 }}</h2>
        <h2
          id="inserted"
          class="inserted2" 
          v-for="mess in insertedMessages"
          v-bind:key="mess.id"
        >
          {{ mess.mess }}
        </h2>
      </div>
      <div id="chat-form">
        <input
          @keyup.enter="addMessage()"
          v-bind:title="inputMessage"
          v-model="inputMessage"
          type="text"
          placeholder="type a message and press enter"
          size="100"
          id="txt"
        />
      </div>
    </div>
  </body>
</template>







<script>
// initializing variables for different url array objects
import axios from "axios";
import { setupCache } from 'axios-cache-adapter'


export default {
  data() {
    return {
      newmess: "",
      generatedlast: "",
      inputMessage: "",
      ms1: "Please",
      ms2: "Run",
      ms3: "Json",
      ms4: "Server",
      valtitle: "no chat selected",
      lasts: "",
      current: null,
      value1: "no chat selected",
      message: " You at " + new Date().toLocaleString(),
      counter: 1,
      insertedMessages: [],
      messages: [],
      users: [],
      search: "",
    };
  },

  // caching data with vuejs
  async created() {
    
     const cache = setupCache({
  maxAge: 15 * 60 * 1000
})

// Create `axios` instance passing the newly created `cache.adapter`
const api = axios.create({
  adapter: cache.adapter
})

// Send a GET request to some REST api
api({
  url: 'http://localhost:3000/users',
  method: 'get'
}).then(async (response) => {
  // Do something fantastic with response.data \o/
  this.users = response.data

  // Interacting with the store, see `localForage` API.
  const length = await cache.store.length()

  console.log('Cache store length:', length)
})
api({
  url: 'http://localhost:3000/messages',
  method: 'get'
}).then(async (response) => {
  // Do something fantastic with response.data \o/
  this.messages = response.data

  // Interacting with the store, see `localForage` API.
  const length = await cache.store.length()

  console.log('Cache store length:', length)
})
api({
  url: 'http://localhost:3000/insertedMessages',
  method: 'get'
}).then(async (response) => {
  // Do something fantastic with response.data \o/
  this.insertedMessages = response.data

  // Interacting with the store, see `localForage` API.
  const length = await cache.store.length()

  console.log('Cache store length:', length)
})
  },
  methods: {
    async addMessage() {
      const resp = await axios.post('http://localhost:3000/insertedMessages', { mess: this.inputMessage });

      if (this.inputMessage == "") {
        alert("Please insert some text.");
      } else {
        this.insertedMessages = [...this.insertedMessages, resp.data];

        this.inputMessage = "";
      }
    },
    increment() {
      if (this.counter == 1) {
        this.counter++;
        alert("hi");
      } else if (this.counter == 2) {
        this.counter--;
      }
    },
    display: function (id) {
      this.generatedlast = this.current = id;
      if (this.current == 1) {
        this.valtitle = this.users[0].name;
        this.lasts = "last seen today at 12:09 PM";

        this.ms1 = this.messages[0].ms1;
        this.ms2 = this.messages[0].ms2;
        this.ms3 = this.messages[0].ms3;
        this.ms4 = this.messages[0].ms4;
      } else if (this.current == 2) {
        this.lasts = "last seen today at 12:05 PM";
        this.valtitle = this.users[1].name;
        this.ms1 = this.messages[1].ms1;
        this.ms2 = this.messages[1].ms2;
        this.ms3 = this.messages[1].ms3;
        this.ms4 = this.messages[1].ms4;
      } else if (this.current == 3) {
        this.lasts = "last seen today at 12:00 PM";
        this.valtitle = this.users[2].name;
        this.ms1 = this.messages[2].ms1;
        this.ms2 = this.messages[2].ms2;
        this.ms3 = this.messages[2].ms3;
        this.ms4 = this.messages[2].ms4;
      } else if (this.current == 4) {
        this.lasts = "last seen today at 11:49 AM";
        this.valtitle = this.users[3].name;
        this.ms1 = this.messages[3].ms1;
        this.ms2 = this.messages[3].ms2;
        this.ms3 = this.messages[3].ms3;
        this.ms4 = this.messages[3].ms4;
      } else if (this.current == 5) {
        this.lasts = "last seen today at 3:49 AM";
        this.valtitle = this.users[4].name;
        this.ms1 = this.messages[4].ms1;
        this.ms2 = this.messages[4].ms2;
        this.ms3 = this.messages[4].ms3;
        this.ms4 = this.messages[4].ms4;
      } else if (this.current == 6) {
        this.lasts = "last seen yesterday at 11:49 PM";
        this.valtitle = this.users[5].name;
        this.ms1 = this.messages[5].ms1;
        this.ms2 = this.messages[5].ms2;
        this.ms3 = this.messages[5].ms3;
        this.ms4 = this.messages[5].ms4;
      } else if (this.current == 7) {
        this.lasts = "last seen yesterday at 9:32 PM";
        this.valtitle = this.users[6].name;

        this.ms1 = this.messages[6].ms1;
        this.ms2 = this.messages[6].ms2;
        this.ms3 = this.messages[6].ms3;
        this.ms4 = this.messages[6].ms4;
      }
    },
  },

  computed: {
    filteredSearch() {
      return this.users.filter((user) => user.name.includes(this.search));
    },
  },
};
</script>









<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  margin: 0;
  padding: 0;

  box-sizing: border-box;
}

html,
body {
  height: 100%;
}
html {
  font-family: Arial, Helvetica, sans-serif;
  background: lightgray;
  font-size: 10px;
}
body {
  display: grid;
  place-items: center;
}
#big-box {
  display: grid;
  grid:
    "search-container chat-title" 71px
    "conversation-list chat-message-list" 1fr
    "page-of chat-form" 78px
    / 275px 1fr;
  min-width: 800px;
  max-width: 1500px;
  max-height: 800px;

  margin-right: auto;
  width: 100%;
  height: 95vh;
  background: #fff;
  border-radius: 10px;
  border-style: solid;
  border-width: medium;
}
#search-container {
  background: lightcyan;
}
#conversation-list {
  grid-area: conversation-list;
  background: white;
  overflow-y: scroll;
}
#conversation {
  display: grid;
  grid-template-columns: 40px 1fr max-content;
  grid-gap: 10px;
  color: black;
  font-size: 1.3 rem;
  border-bottom: 1px solid black;
  padding: 20px 20px 20px 15px;
}

#conversation.active,
#conversation:hover {
  background: gray;
}
#conversation:hover {
  cursor: pointer;
}
.box {
  border: solid black;
}
img {
  grid-row: span 2;
  height: 100px;
  width: 40px;
  border-radius: 20%;
}
#conversation > img {
  grid-row: span 2;
  height: 60px;
  border-radius: 20%;
}
.title-text {
  padding-right: -200px;
  padding-top: 0px;

  font-weight: bold;
  color: black;
  white-space: nowrap;

  text-overflow: ellipsis;
  font-size: 20px;
}
.created-date {
  color: black;
  font-size: 1.2rem;
}
.conversation-message {
  grid-column: span 2;
  padding-right: 110px;
  white-space: nowrap;

  text-overflow: ellipsis;
  font-size: 18px;

  color: black;
  margin-left: 1px;
}
#search-container input {
  display: grid;
  align-items: left;
  padding: 0 20px;
  grid-area: search-container;
  border-radius: 10px 0 0 0;
  box-shadow: 0 1px 3px -1px rgba(0, 0, 0, 0.75);
  z-index: 1;
  color: #eee;
  outline: none;
  font-weight: bold;
  border-radius: 2px;
  height: 30px;
  border: 0;
  border-radius: 60px;
  padding-left: 40px;
  padding-right: 20px;
  font-size: 1.4rem;
  background: url("images/search/logosearch.svg") no-repeat white;
  background-position: 15px center;
  background-size: 20px 20px;
  margin-top: 20px;
  margin-left: 20px;
  color: gray;
}
#search-container input::placeholder {
  color: gray;
  font-weight: bold;
}

#page-of {
  border-top: 1px solid rgba(0, 0, 0, 0.25);
  border-radius: 0 0 0 10px;
  background: lightcyan;
}
#page-of h1 {
  color: black;
  text-decoration: none;
  font-size: 1.3rem;
  margin-top: -30px;
  margin-left: 115px;
}
#page-of h2 {
  color: black;
  text-decoration: none;
  font-size: 2.2rem;
  margin-top: 25px;
  margin-left: 20px;
}
#page-of h3 {
  color: black;
  text-decoration: none;
  font-size: 2.3rem;
  margin-top: -40px;
  margin-left: 230px;
}
#conversation-list {
  grid-area: conversation-list;
  background-color: #eee;
}
#chat-title,
#chat-form {
  background: #eee;
}
#chat-title {
  background-color: lightcyan;
  font-weight: bold;
  font-style: normal;
  font-family: serif;
  display: grid;
  grid: 36px / 1fr 36px;
  align-content: center;
  align-items: center;
  grid-area: chat-title;
  color: black;
  font-weight: bold;
  font-size: 1.4rem;
  border-radius: 0 10px 0 0;
  box-shadow: 0 1px 3px -1px rgba(0, 0, 0, 0.75);
  padding: 0 20px;
}
#chat-message-list {
  grid-area: chat-message-list;
  overflow-y: scroll;
  display: flex;
  flex-direction: column;
  padding: 0 20px;
}

#chat-form input {
  size: 300px;
  outline: none;
  padding: 15px;
  border: 2px solid black;
  color: #330;
  border-radius: 6px;
  font-size: 1.4rem;
}
#display-results {
  max-width: 100px;
  max-height: 80px;
  width: 50%;
  height: 95vh;
  background: black;
}
#display-results p {
  color: white;
}
::-webkit-scrollbar {
  display: none;
}
#ms1 {
  padding: 9px 14px;
  margin-left: 20px;
  margin-bottom: 5px;
  margin-top: 30px;
  font-size: 20px;
  font-weight: normal;
}
#ms2 {
  flex-direction: row;
  padding: 9px 14px;
  margin-left: 20px;
  margin-top: 30px;

  font-weight: normal;
}
#ms3 {
  margin-left: 20px;
  margin-top: 30px;
  font-weight: normal;
  padding: 9px 14px;
}
#ms4 {
  flex-direction: row;
  padding: 9px 14px;
  margin-left: 20px;
  margin-top: 30px;
  font-weight: normal;
}
#newmessage {
  margin-left: 900px;
  margin-top: 90px;
  font-weight: normal;
}
.other1 {
  display: grid;
  grid-template-columns: 70%;
  background: #eee;
  color: #111;
  border: 1px solid #ddd;
  border-radius: 14px 14px 14px 0;
}
.you1 {
  background: lightcyan;
  color: #111;
  border: 1px solid #ddd;
  border-radius: 14px 14px 0 14px;
  text-align: right;
}
.other2 {
  display: grid;
  grid-template-columns: 70%;
  background: #eee;
  color: #111;
  border: 1px solid #ddd;
  border-radius: 14px 14px 14px 0;
}
.you2 {
  background: lightcyan;
  color: #111;
  border: 1px solid #ddd;
  border-radius: 14px 14px 0 14px;
  text-align: right;
}
#inserted {
  flex-direction: row;
  padding: 9px 14px;
  margin-left: 20px;
  margin-top: 30px;
  font-weight: normal;
}
.inserted2 {
  background: lightcyan;
  color: #111;
  border: 1px solid #ddd;
  border-radius: 14px 14px 0 14px;
  text-align: right;
}
#lasts {
  display: unset;
  grid: unset;
  align-content: center;
  align-items: center;
  grid-area: chat-title;
  color: black;
  font-weight: bold;
  font-size: 1.2rem;
  border-radius: 0 10px 0 0;
  text-align: right;
  margin: 50px 25px;
  padding: 0 0 23px 30px;

  white-space: nowrap;

  text-overflow: ellipsis;
}
#txt {
  height: 80px;
  border: 2px solid red;
}
</style>

