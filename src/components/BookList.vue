<template>
<div>
    <div v-for="list in getCardsForChecklist">

        <div v-if="list.name">
          <b>Card: {{list.name}}</b> - <a target="_blank" v-bind:href="list.url"> Trello Card </a>
        </div>

        <div>Checklist: {{list.checkListName}}</div>
        <div v-for="item in list.checkItems">
          <input type="checkbox" id="checkbox" v-model="item.state == 'complete'">
          <label for="checkbox">{{ item.name }}</label>
        </div>
        <hr>

    </div>
</div>
</template>

<script>
// import BookItem from "./BookItem";
// import BookForm from "./BookForm";
import axios from "axios";
import { error, log } from "util";

//TODO: move to secure place:
const TRELLO_KEY = "20496bbf6978138b3b3e5462eded71ea";
// const TRELLO_OAUTH_SECRET=

const requestTokenUrl = "OAuthGetRequestToken";

const apiUrl = "https://api.trello.com/";
const token =
  "1e8bca5d65f830eef97d2d2e35d17324f90b5845317e364a289e8d7484045614";

function buildUrl(url) {
  return apiUrl + url + "&key=" + TRELLO_KEY + "&token=" + token;
}

var cardUrl = "/1/boards/Uky0qCHP/cards?filter=visible";
var checkListUrl = "1/boards/Uky0qCHP/checklists?cards=open&fields=all";
var listUrl = "/1/boards/Uky0qCHP/lists?filter=open";
var boardId = "Uky0qCHP";

export default {
  name: "Booklist",
  components: {},
  data() {
    return {
      checkListItems: [],
      cards: [],
      lists: [],
      // newChecklist: []
    };
  },
  mounted() {
    this.getChecklistsOnBoard(boardId);
    this.getAllMyCards();
    this.getAllListsOnBoard(boardId);
    
  },
  methods: {
    getChecklistsOnBoard(boardId) {
      let url = buildUrl(checkListUrl);
      axios
        .get(url)
        .then(respons => {
          this.checkListItems = respons.data;
        })
        .catch(error => {
          console.log(error);
        });

     
         
    },
    getAllMyCards() {
      let url = buildUrl(cardUrl);
      axios
        .get(url)
        .then(respons => {
          this.cards = respons.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    getAllListsOnBoard(boardId) {
      let url = buildUrl(listUrl);
      axios
        .get(url)
        .then(respons => {
          this.lists = respons.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  computed: {
    getCardsForChecklist() {
      let checkLists = this.checkListItems;
      let cards = this.cards;
      let lists = this.lists;
        
      this.checkListItems.forEach(element => {
        Object.defineProperty(element, "checkListName",
        Object.getOwnPropertyDescriptor(element, "name"));
        delete element["name"];
      });

      let result = checkLists.map(checkList =>
        Object.assign(
          checkList,
          cards.find(card => card.id == checkList.idCard)
        )
      );

      return result;
    }
  }
};
</script>

<style>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}
</style>
