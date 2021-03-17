<template>
  <div>
    <div>
      <span class="font-bold">Theme</span><br>
      <b-button class="color-white border" onclick="document.body.style.backgroundColor = '';"></b-button>
      <b-button class="color-red border" onclick="document.body.style.backgroundColor = 'indianred';"></b-button>
      <b-button class="color-blue border" onclick="document.body.style.backgroundColor = '#2173ec';"></b-button>
      <b-button class="color-green border" onclick="document.body.style.backgroundColor = 'mediumseagreen';"></b-button>
    <b-container class="col-6 px-0">
      <b-row class="my-1 px-0 border border-radius" v-for="type in dates1" :key="type">
        <b-col class="px-0 text-center font-bold" sm="3">
          <label :for="`type-${type}`">Set date</label>
        </b-col>
        <b-col class="px-0" sm="9">
          <b-form-input v-model="getDate1" debounce="100" :id="`type-${type}`" :type="type"></b-form-input>
        </b-col>
      </b-row>
      <b-row class="my-1 px-0 border border-radius" v-for="type in times1" :key="type">
        <b-col class="px-0 text-center font-bold" sm="3">
          <label :for="`type-${type}`">Set time</label>
        </b-col>
        <b-col class="px-0" sm="9">
          <b-time class="w-100" v-model="getTime1" show-seconds debounce="100" :id="`type-${type}`" :type="type"></b-time>
        </b-col>
      </b-row>
      <div class="my-1 px-0 button-center border border-radius">
        <div class="my-2 px-1 align-left font-bold" v-on:autocomplete="getDate1 + getTime1">Setup: {{ getDate + ' ' + getTime }}</div>
        <b-button class="my-1 px-1 border border-radius button-color-special" v-on:click="extendDeadline1()"><span class="text-color-special">Add deadline</span></b-button>
      </div>
    </b-container>
    <div class="text-center">
      <h3>Deadline: {{ deadline1 }}</h3>
      <flip-countdown
          class="font-bold"
          :deadline="deadline1">
      </flip-countdown>
    </div>
    </div>
    <div class="border align-center">
      <div class="text-center">
        <b-button
            class="border my-2"
            @click="openPopup()">
          Add deadline
        </b-button>
      </div>
      <b-modal
          id="modal-add"
          centered
          title="Add deadline"
          ok-title="Add"
          v-if="getDate2 != null && getTime2 != null"
          cancel-title="Cancel"
          @ok="addDeadline() && extendDeadline2()">
        <b-form>
          <b-form-group
              label-cols="4"
              label="Set date"
              label-class="font-weight-bold">
            <b-row class="my-1" v-for="type in dates2" :key="type">
              <b-form-input
                  v-model="getDate2"
                  debounce="100"
                  :id="`type-${type}`"
                  :type="type">
              </b-form-input>
            </b-row>
          </b-form-group>

          <b-form-group
              label-cols="4"
              label="Set time"
              label-class="font-weight-bold">
            <b-row class="my-1" v-for="type in times2" :key="type">
              <b-time
                  v-model="getTime2"
                  show-seconds
                  debounce="100"
                  :id="`type-${type}`"
                  :type="type">
              </b-time>
            </b-row>
          </b-form-group>
        </b-form>
      </b-modal>

      <table >
        <thead>
        <tr>

          <th scope="col">Deadlines</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(entry, i) in sortedList" :key="i">
          <flip-countdown
              class="font-bold border"
              :deadline="deadline2">
          </flip-countdown>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import FlipCountdown from 'vue2-flip-countdown'
import moment from "moment";

const fmt = "YYYY-MM-DD HH:mm:ss";

export default {
  name: "app",
  components: {
    FlipCountdown,
  },
  data() {
    return {
      deadline1ts: moment().format(fmt),
      deadline2ts: moment().format(fmt),
      getDate1: null,
      getDate2: null,
      getTime1: null,
      getTime2: null,
      dates1: [
        'date',
      ],
      times1: [
        'time',
      ],
      dates2: [
        'date',
      ],
      times2: [
        'time',
      ],
      deadline: "",
      deadlines: [],
    }
  },
  computed: {
    deadline1: function () {
      return moment(this.deadline1ts).format(fmt);
    },
    deadline2: function () {
      return moment(this.deadline2ts).format(fmt);
    },
    sortedList: function () {
      return this.deadlines.slice().sort(function (a, b) {
        return b.deadline - a.deadline;
      });
    }
  },
  methods: {
    extendDeadline1: function () {
      this.deadline1ts = moment(this.getDate1 + "T" + this.getTime1).format(fmt);
    },
    extendDeadline2: function () {
      this.deadline2ts = moment(this.getDate2 + "T" + this.getTime2).format(fmt);
    },
    openPopup() {
      this.getDate2 = ""
      this.getTime2 = ""
      this.$bvModal.show('modal-add')
    },
    addDeadline() {
      this.deadlines.push({deadline: this.deadline});
    },
  },
}

</script>

<style scoped lang="scss">
.opacity0 {
  background: gray;
}
.color-white {
  background: antiquewhite;
}
.color-red {
  background: indianred;
}
color-blue {
  background: #2173ec;
}
.color-green{
  background: mediumseagreen;
}
body{
  background: lightseagreen !important;
}
.border-radius {
  border-radius: 0.3em;
}
.button-center {
  text-align: center;
}
.align-left {
  text-align: start !important;
}
.font-bold {
  font-weight: bold;
}
.button-color-special {
  background: #212529;
}
.text-color-special {
  color: #fad51f;
}
.align-center {
  text-align: -webkit-center;
}
</style>
