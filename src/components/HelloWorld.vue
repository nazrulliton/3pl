<template>
  <div class="hello">
    <div>
      <h1 class="titleText">STARWARS</h1>

      <span class="middleText" v-if="selectedCharacters.length>0 && selectedCharacters.length <=1">You have selected:{{selectedCharacters[0]}}</span>
      <span class="middleText" v-if="selectedCharacters.length>1 && selectedCharacters.length <=2">You have selected:{{selectedCharacters[0]}},{{selectedCharacters[1]}}</span>
      <span class="middleText" v-if="selectedCharacters.length>2 && selectedCharacters.length <=3">You have selected:{{selectedCharacters[0]}},{{selectedCharacters[1]}},{{selectedCharacters[2]}} </span>
      <span v-if="selectedCharacters != null  && selectedCharacters.length != null  && selectedCharacters.length <= 0" class="middleText">Select 3 Characters</span><br>
      <b-row>
        <b-col lg="4" class="pb-2"></b-col>
        <b-col lg="4" class="pb-2">
          <b-button size="lg" variant="success" style="marginRight:20px; width: 180px; fontWeight:600; backgroundColor:#70AD47;">DOWNLOAD</b-button>
        <b-button size="lg" variant="danger" style="width: 180px;fontWeight:600;backgroundColor:#c00000; ">RESET</b-button>
        </b-col>
        <b-col lg="4" class="pb-2"></b-col>
      </b-row>
    </div>

    <b-container>
      <div class="rIcon">
        <b-icon-arrow-right
          v-on:click="nextPage"
          style="color: lightGrey; width: 120px; height: 120px"
        ></b-icon-arrow-right>
      </div>
      <div class="lIcon" id="leftArr">
        <b-icon-arrow-left
          v-on:click="prevPage"
          style="color: lightGrey; width: 120px; height: 120px">
        </b-icon-arrow-left>
      </div>
      <div class="pageDetails">
        <span style="color:white;">Page: {{pageCount}}/10</span>
      </div>

      <b-row class="cardRow">
        <b-col cols="4" v-bind:key="index" v-for="(value, index) in characterData">
          <b-card
            :id="index"
            v-on:click="handleClick(value,index)"
            no-body
            class="card text overflow-hidden"
          >
            <b-row no-gutters>
              <b-col md="5" style="border-Right: 1px solid black;">
                <b-card-img
                  src="data:image/png;base64,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"
                  alt="Image"
                  class="rounded-0"
                ></b-card-img>
              </b-col>
              <b-col md="7">
                <b-card-body>
                  <b-card-text style="text-Align: right;">{{value.name}}</b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>

    </b-container>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data: () => ({
    selectedCharacters: [],
    characterData: [],
    errors: [],
    pageCount: 1,
    startIndex: 1,
    endIndex: 10
  }),
  created() {
    for (let index = this.startIndex; index < this.endIndex; index++) {
      console.log(index);
      // let indexPosition = index;
      axios
        .get(`http://swapi.dev/api/people/${index}/`)
        .then(response => {
          this.characterData.push(response.data);
          console.log(this.characterData);
        })
        .catch(e => {
          console.log("there was an error");
          this.errors = e;
        });
    }
  },
  methods: {
    nextPage: function(event) {
      if (this.startIndex < 82) {
        this.pageCount = this.pageCount + 1;
        this.startIndex = this.startIndex + 9;
        this.endIndex = this.endIndex + 9;
        console.log(this.startIndex);
        console.log(this.endIndex);
        this.characterData = [];
        for (let index = this.startIndex; index < this.endIndex; index++) {
          console.log(index);
          // let indexPosition = index;
          axios
            .get(`http://swapi.dev/api/people/${index}/`)
            .then(response => {
              this.characterData.push(response.data);
              console.log(this.characterData);
            })
            .catch(e => {
              console.log("there was an error");
              this.errors = e;
            });
        }
        if (this.startIndex > 1) {
          document.getElementById("leftArr").style.display = "block";
        }
      }
    },
    prevPage: function(event) {
      if (this.startIndex > 1) {
        this.pageCount = this.pageCount - 1;
        this.startIndex = this.startIndex - 9;
        this.endIndex = this.endIndex - 9;
        console.log(this.startIndex);
        console.log(this.endIndex);
        this.characterData = [];
        for (let index = this.startIndex; index < this.endIndex; index++) {
          console.log(index);
          // let indexPosition = index;
          axios
            .get(`http://swapi.dev/api/people/${index}/`)
            .then(response => {
              this.characterData.push(response.data);
              console.log(this.characterData);
            })
            .catch(e => {
              console.log("there was an error");
              this.errors = e;
            });
        }
        if (this.startIndex < 9) {
          document.getElementById("leftArr").style.display = "none";
        }
      }
    },
    removeCharacter: function(data) {
      for (var i = 0; i < this.selectedCharacters.length; i++) {
        if (this.selectedCharacters[i] === data.name) {
          this.selectedCharacters.splice(i, 1);
        }
      }
    },
    handleClick: function(data, index) {
      var x = document.getElementsByClassName("card")[index];
      // console.log("first character=>", this.characterData[index]);
      if (this.selectedCharacters.includes(data.name)) {
        console.log("already selected!");
        x.style.backgroundColor = "white";
        this.removeCharacter(data);
      } else {
        if (
          this.selectedCharacters.length < 3 &&
          !this.selectedCharacters.includes(data.name)
        ) {
          if (x.id == index) {
            x.style.backgroundColor = "green";
            this.selectedCharacters.push(data.name);
            var check = false;
          }
        }
      }
      console.log("selected characters", this.selectedCharacters);
    }
  }
};
</script>
<style scoped>
.titleText {
  color: black;
  -webkit-text-stroke: 2px yellow;
  font-size: 4rem;
  margin-bottom: 0;
}
.middleText {
  color: white;
  font-size: 2rem;
}
.text {
  color: black;
  margin-bottom: 50px;
}
.rIcon {
  /* color: white; */
  /* background-color: white; */
  right: 10px;
  position: absolute;
  top: 50%;
}
.lIcon {
  /* color: white; */
  /* background-color: white; */
  left: 10px;
  position: absolute;
  top: 50%;
  display: none;
}
.pageDetails {
  position: absolute;
  bottom: 0;
  right: 50%;
  font-weight: 600;
  font-size: 2rem;
}
.turnGreen {
  background-color: green;
}
</style>