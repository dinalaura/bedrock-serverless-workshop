<style>
@import '../app.css';
</style>
<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-6 mb-4" style="min-width: 750px;">
        <div class="card-header" style="background-color:#C398BD">
          Welcome to AnyCompany 
        </div>
        <div class="card">
          <img class="card-img-top" src="" alt=""/>
          <div class="card-body">
            <h2> Go to AWS Solution </h2>
            <table hidden>
              <tr>
                <td >
                <label for="model">Model:</label>&nbsp;
                </td>
                <td>
                  <div class="select">
                    <select id="model" name="model">
                      <option value="anthropic.claude-3-haiku-20240307-v1:0" >Claude 3 Haiku</option>
                      <option value="anthropic.claude-3-5-sonnet-20240620-v1:0" selected>Claude 3.5 Sonnet</option>
                      <option value="anthropic.claude-3-opus-20240229-v1:0">Claude 3 Opus</option>
                      <option value="mistral.mistral-7b-instruct-v0:2">Mistral 7B</option>
                      <option value="meta.llama3-1-8b-instruct-v1:0">Llama 3.1 Instruct 8B</option>
                    </select>
                  </div>
                </td>
                <td>
                &nbsp;  <label for="model">Temperature:</label>&nbsp;
                </td>
                <td>
                  <div class="select">
                  <select id="temperature" name="temperature">
                    <option value='0'>0</option>
                    <option value='0.5' selected>0.5</option>
                    <option value='1'>1</option>
                  </select>
                  </div>
                </td>
              
                <td>
                &nbsp;  <label for="model">Tokens:</label>&nbsp;
                </td>
                <td>
                  <div class="select">
                  <select id="token" name="token">
                    <option value="250">250</option>
                    <option value="500">500</option>
                    <option value="1000" selected>1000</option>
                    <option value="2000">2000</option>
                     <option value="5000">5000</option>
                  </select>
                  </div>
                </td>
              </tr>
            </table>
          <br />
            <form @submit="formSubmit" hidden>
              <strong>AnyCompany</strong> <br />
              <input type="text" class="form-control" v-model="name" placeholder="Escribe tu pregunta..." />
              <br />
              <button class="btn btn-success">Haz una pregunta</button>
            </form>
            <br />
            <div id="loading" style="display: none">
              <strong>Cargando...</strong>
            </div>
            <div id="divresult" class="text-secondary mb-2" style="display: block; padding: 3px;">
              <template>
                <div>
                  <strong v-if="output.answer" style="display: block; white-space: pre-line; text-align: left">Response: </strong>
                  <span v-if="output.answer" style="white-space: pre-line; text-align: left">{{output.answer}}</span>
                </div>
              </template>
              <br />
              <strong v-if="output.source_documents" style="display: block; text-align: left">References: </strong>
              <span v-if="output.source_documents" style="white-space: pre-line; text-align: left">{{output.source_documents }}</span>
              <br />
              <strong v-if="output.error" style="display: block; white-space: pre-line; color: red; text-align: left">Error: </strong>
              <span v-if="output.error" style="white-space: pre-line; text-align: left; color: red; font-style: italic;">{{output.error}}</span>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-6 mb-4" style="max-width: 350px;" hidden>
        <div class="card-header" style="background-color:#C398BD">
          Go to Innovatech Section
        </div>
        <div class="card">
          <img class="card-img-top" src="" alt="" />

          <div class="card-body" hidden>
            <label style="font-weight:lighter;color:grey">Dame información acerca de la companía Innovatech en Español. Otras compañías: Green Threads o Apex Roasters</label>
            <br />
            <label style="font-weight:lighter;color:grey">¿Cómo sería el cuerpo de texto de un post de social media de marketing para la companía Innovatech? dame un ejemplo en Español, usa emojis</label>
            <br />
            <label style="font-weight:lighter;color:grey" >Escribe un post para red social el body copy de la publicación para Innovatech en Español, dame un ejemplo de publicación</label>
            <br />
            <label style="font-weight:lighter; color:grey" >¿Cómo ayudar por medio de redes sociales a un cliente de Innovatech a conocer y usar la marca? dame un ejemplo que invite al cliente a conocer la marca</label>
            <br />
            <label style="font-weight:lighter; color:grey" >¿Cómo sería una campaña de marketing para la companía innovatech? Dame un ejemplo en Español</label>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getAuthToken } from './../utils/auth'

  export default {
      mounted() {
          console.log('Component mounted.')
      },
      data() {
          return {
              name: '',
              description: '',
              output: ''
          };
      },
      methods: {
          formSubmit(e) {
              console.log('started.')
              e.preventDefault();
              var x = document.getElementById("divresult");
              var img = document.getElementById("loading");
              var model_select = document.getElementById('model');
              var model_id = model_select.options[model_select.selectedIndex].value;

              var temperature_select = document.getElementById('temperature');
              var temperature = parseFloat(temperature_select.options[temperature_select.selectedIndex].value);

              var token_select = document.getElementById('token');
              var token = parseInt(token_select.options[token_select.selectedIndex].value);
              img.style.display = "block";
              x.style.display = "none";
              let currentObj = this;
              const json = JSON.stringify({
                  query: this.name,
                  model_id: model_id,
                  temperature: temperature,
                  max_tokens: token
              });
             console.log(json)
             const config = {
  headers:{
      'Content-Type': 'application/json',
      'Authorization': getAuthToken()
    }
  };
   this.axios.post('/llms',
   json, config).then(function(response) {
                  img.style.display = "none";
                  x.style.display = "block";
                  console.log(response.data)
                  currentObj.output =response.data
                  console.log(currentObj.output)
              }).catch(function(error) {
                  currentObj.output = error;
              });
          }
      }
  }
</script>
