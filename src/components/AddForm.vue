<template>
  <b-container>
      <b-row class="justify-content-md-center">
        <b-nav-form>
          <b-input-group prepend="New Task">
            <b-form-input placeholder="todo" v-model="content"></b-form-input>
            <button @click="add(content)">+ Add</button>
          </b-input-group>
        </b-nav-form>
  </b-row>
  </b-container>
</template>

<script>
import axios from "axios"

import { POST_URL } from '../../config/routeRequest'
import { mapGetters } from 'vuex'

export default {
  name: 'Form', 
  props: {
    forceRerender: Function
  },
  data () {
    return {
      state: "",
      id: null,
      date: null,
      content: '',
      error: ""
    }
  },
  computed: mapGetters(['getList']),
  methods: {
    getDate: function (){
      const date = new Date()
      const options = {weekday: "long", year: "numeric", month: "long", day: "numeric"};
      const getDate = date.toLocaleDateString('eu-FR', options)
      return getDate
    },
    add: function(content){
      this.date = this.getDate()
      this.id = this.getList
      this.state = {
        id: this.id++,
        name: content,
        todo: true,
        isActive: true,
        createdAt: this.date
      }
      this.content = ""
      this.postRequest(this.state.id, this.state)
    },
    async postRequest(id, body){
      try{
        await axios.post(POST_URL, body)
        this.$store.dispatch('ACTION_POST', body)
      }catch(error){
        this.error = error
        console.error("ERRORS POST REQUEST --> ", this.error)
      }
    }
  }
}
</script>

<style scoped>
.form-inline{
  list-style: none;
}
</style>