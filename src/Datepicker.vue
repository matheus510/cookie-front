<template>
    <div>
        <div>
            <button v-on:click="loadReservations">load</button>
            <ul>
                <li v-for="dayReservation in state.dayReservations">
                    <!-- later make this anchor call edit modal :) -->
                    <a href="#"><div>
                    <p>Room: {{  (dayReservation.RoomId == 1) ? "Bolacha" : "Biscoito"  }}</p>    
                    <p>Date Start : {{ parseHours(dayReservation.DateReserveStart) }} </p>
                    <p>Date end: {{ parseHours(dayReservation.DateReserveEnd) }} </p>
                    </div></a>    
                </li>
            </ul>
            <datepicker v-model="state.date" :inline="true"></datepicker>
            <h3>{{ state.date }}</h3>
        </div>
    </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'

let moment = require('moment');

var state = {
    date: '',
    roomList: [],
    dayReservations: [],
}

export default {
    components: {
        Datepicker
    },
    data () {
        return {
            format: 'YYYY-MM-d',
            disabled: {},
            eventMsg: null,
            state: state,
        }
    },
    methods: {
        loadReservations: function () {
            console.log('foi')
            var parsedDate = this.parseDate(this.state.date)
            console.log(parsedDate)
            let response = this.$http.get(`http://localhost:3000/reservation/${parsedDate}`)
            .then(response => {
                this.state.dayReservations = response.body
                console.log('foi')
            }, response => {
                console.log('ruim')
            })
        },
        parseDate: function (date) {
            return moment(date).format("YYYY-MM-DD")
        },
        parseHours: function (date) {
            return moment(date).format("HH:mm")
        }
    }
  }

</script>

<style>
body {
    font-family: 'Helvetica Neue Light', Helvetica, sans-serif;
    padding: 1em 2em 2em;
}
input, select {
    padding: .75em .5em;
    font-size: 100%;
    border: 1px solid #ccc;
    width: 100%
}

select {
    height: 2.5em;
}


code,
pre {
    margin: 1em 0;
    padding: 1em;
    border: 1px solid #bbb;
    display: block;
    background: #ddd;
    border-radius: 3px;
}

.settings {
    margin: 2em 0;
    /*padding: 0 1.5em 1.5em;*/
    border-top : 1px solid #bbb;
    background: #eee;
}

h5 {
    font-size:100%;
    padding: 0;
}

.form-group {
    margin-bottom: 1em;
}

.form-group label {
    font-size: 80%;
    display: block;
}

</style>