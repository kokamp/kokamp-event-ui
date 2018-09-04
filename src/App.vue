
  <template>
 <ul class="Event-list">
  <h1>events</h1>
  <div v-for="event in events" v-bind:key="event.id">
   <p @click="setEdit(event)" v-if="!editing">
   {{event.name}}
    {{event.price}}
     {{event.description}}
   </p>
  <div v-else>
   <input type="text" v-model="event.name">
   <button @click="saveevent(event)">Save</button>
   <button @click="removeevent(event)">Remove</button>
  </div> 
  </div>
  <div v-if="!editing">
  <input type="text" v-model="eventName" placeholder="event name"> 
  <button @click="addevent(eventName)">Add</button>
  </div>
 </ul>
</template>

<script>
import axios from 'axios'
export default {
 data() {
 return {
  ROOT_URL: 'http://localhost:3000/events/',
  events: [],
  eventName: '',
  editing: false,
 };
},
  created() {
    this.geteventList();
  },

  methods: {
    geteventList() {
      axios
        .get(this.ROOT_URL)
        .then(response => {
          this.events = response.data;
        })
        .catch(error => console.log(error));
    },
      addevent(name) {
  axios
   .post(this.ROOT_URL, { name })
   .then(response => {
     this.events.push(response.data);
     this.eventName = ''; 
   })
   .catch(error => console.log(error));
 },
 setEdit(event) {
   this.editing = !this.editing;
},
saveevent(event) {
 this.setEdit();
 axios
 .put(`${this.ROOT_URL}/${event.id}`, { ...event })
 .then(response => {
 console.log(response.data);
 })
 .catch(error => console.log(error));
}
  }
}
</script>

<style>
.event-list {
  background-color: rebeccapurple;
  padding: 10px;
  width: 50%;
  text-align: center;
  margin: 0 auto;
  color: white;
}
</style>

