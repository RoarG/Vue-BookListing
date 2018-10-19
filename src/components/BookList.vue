<template>
<div>
    <div v-for="list in getCardsForChecklist">

        <b>Checklist Name: {{list.name}}</b> - <a target="_blank" v-bind:href="list.url"> Trello Card </a>
        <div v-for="item in list.checkItems">
          <input type="checkbox" id="checkbox" v-model="item.state == 'complete'">
          <label for="checkbox">{{ item.name }}</label>
            {{ item.name }}
            {{ item.id }}
        </div>
        <hr>

    </div>
</div>
</template>

<script>
// import BookItem from "./BookItem";
// import BookForm from "./BookForm";
import axios from "axios";
import { error } from "util";

const apiUrl = "https://api.trello.com/";
const apiKey = "20496bbf6978138b3b3e5462eded71ea";
const token =
  "879ba30c74eb091eb42c222d871559378006b2caa26b338c442585c3edada979";

function buildUrl(url) {
  return apiUrl + url + "&key=" + apiKey + "&token=" + token;
}

var cardUrl = "/1/boards/Uky0qCHP/cards?filter=open";
var checkListUrl = "1/boards/Uky0qCHP/checklists?fields=all";
// var cardUrl = "/1/cards/";
var boardId = "Uky0qCHP";

export default {
  name: "Booklist",
  components: {},
  data() {
    return {
      checkListItems: [],
      cards: []
    };
  },
  mounted() {
    this.getChecklistsOnBoard(boardId);
    this.getAllMyCards();
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
    }
  },
  computed: {
    getCardsForChecklist() {
      let checkLists = this.checkListItems;
      let cards = this.cards;

      checkLists.forEach(element => {
        console.log("find:", cards.find(card => card.id == element.idCard));
      });

      let result = checkLists.map(checkList =>
        Object.assign(
          checkList,
          cards.find(card => card.id == checkList.idCard)
        )
      );
      console.log("SDASD", result);

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
