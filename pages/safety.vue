<template>
  <div>
   <decoration />
   <v-btn
        style="margin:80px"
        :href="routeLink.agency.home"
        icon
        class="backtohome"
      >
        <i class="ion-ios-home-outline" />
        <i class="ion-ios-arrow-round-back-outline" />
      </v-btn>
  <div style="margin: 150px;font-weight: 500;">
    <p>😊 please insert your requirements.txt content so we can tell your vulnerability: <br> here I provide you a sample file you can click submit saftey check to find out more 👇</p>
  <editor
  v-model="content"
api-key='your-api-key' :init="{ menubar: false,
        toolbar: false,/* your other settings */ }" />

  <div
    @click="submitSafetyCheck"
    style="display: flex; 
    justify-content: center;
    align-items: center;
    margin: 40px;">
    <v-btn
    rounded
    color="primary"
    dark
    >
    
      Submit safety Check
  
    </v-btn>
</div>
<div
style="display: flex; 
  justify-content: center;
  align-items: center;">
  <pre>{{ response }}</pre>
</div>
</div>
</div>
</template>

<script>
import Editor from '@tinymce/tinymce-vue'
import axios from 'axios'
import Decoration from '~/components/Forms/Decoration'
import link from '~/static/text/link'


export default {
  data: () => {
    return {
      content: '<p>insecure-package==0.1.0</p> \n <p>Django==1.0</p>',
      response: '',
      routeLink: link
    }
  },
  name: 'App',
  components: {
    editor: Editor,
    decoration:  Decoration
  },
  methods: {
    async submitSafetyCheck() {
      try {
        let enhancedContent = this.content.replaceAll('<p>', '')
        enhancedContent = enhancedContent.replaceAll('</p>', '')
        const response = await axios.post(
          'https://securise-backend.runflare.run/safety-check/',
          {
            // requirements: this.content
            requirements: enhancedContent
          },
          {
            headers: {
              'Content-Type': 'application/json'
            }
          }
        )
        console.log(response.data)
        this.response = response.data
      } catch (error) {
        console.error(error)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../components/Forms/form-style.scss';

</style>
