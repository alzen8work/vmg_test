<script>
  import '@/index.css'
  import axios from 'axios'
  import { Input,Button } from 'flowbite-vue'
  
  export default {
    data () {
      return {
        title: 'some title',
        body: 'remove text to check validation',
      }
    },
    components: {
      Button,Input
    },
    mounted() {
      console.log(`the component is now mounted.`)
    },
    created() {
      console.log(`the component is now created.`)
    },
    methods:{
      async fetchUsers() {        
        try {
          const response = await axios.get('https://gorest.co.in/public/v2/users');
          console.log(response);
        } catch (error) {
          console.error(error);
        }
      },
      CreateUserPost() {  
        
        let token = '7dc1ac295382e25b1f9116aa4a32d63705b7e1324a65d27d0402fd70f4a1cadf';
          axios({
            method: 'post',
            url: 'https://gorest.co.in/public/v2/users/24/posts',
            headers: { 
              "Content-Type": "multipart/form-data",
              'Authorization': 'Bearer '+token
            },
            data: {
              title: this.title,
              body: this.body,
            }
          })
          .then(function (response) {
            console.log(response.data);
            let obj1 = response.data;
            let html = '';
            Object.keys(obj1).forEach(function(key) {
                html += key + ' : ' + obj1[key] + '\n';
            });
            alert(html);
           
          })
          .catch(function (error){
            let obj = error.response.data
            Object.keys(obj).forEach(function(key) {
                if( obj[key]['field'] == 'body'){
                  alert('body ' + obj[key]['message']);
                }
                if( obj[key]['field'] == 'title'){
                  alert('title ' + obj[key]['message']);
                }
            });
          });
      }        
    }
  }
</script>

<template>
  <span>
      <Input placeholder="enter your Title" label="Title" v-model="this.title" class="text-black"/>
      <Input placeholder="enter your Body Text" label="Body Text" v-model="this.body" class="text-black"/>
      <Button color="default"  class="w-full mt-9" @click="CreateUserPost">Submit</Button>
      <p class="w-full mt-9" style="color:blue">Resize browser to check responsive</p>
  </span>
</template>
