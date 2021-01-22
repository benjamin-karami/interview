<template>
  <div>
    <div>
      <b-nav class="mt-4 d-flex justify-content-center">
        <NuxtLink class="mx-3" to="/">Home</NuxtLink>
        <NuxtLink class="mx-3" :event="ajaxCall ? 'click' : ''" to="/wordCounter">Word counter Page</NuxtLink>
      </b-nav>
    </div>
    <div class="d-flex justify-content-around mt-5">
      <section>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <b-form-group id="input-group-2" label="Your City:" label-for="input-2">
            <b-form-input
              id="input-2"
              v-model="form.city"
              placeholder="Enter your city"
              required
            ></b-form-input>
          </b-form-group>
          <b-button type="submit" variant="primary">Forcast</b-button>
        </b-form>
        <p class="text-danger mt-3" :style="{opacity: ajaxError ? 1 : 0}">Please Enter Valid city name</p>
      </section>
      <section>
        <div :style="{opacity: ajaxCall ? 1 : 0}" class="col-12 col-md-12 col-sm-12 col-xs-12">
            <div class="card p-4">
                <div class="d-flex">
                    <h6 class="flex-grow-1">{{ajaxCall ? `${cityInfo.location.country} / ${cityInfo.location.name}` : `enter your city`}}</h6>
                    <h6>{{ajaxCall ? cityInfo.location.localtime : null}}</h6>
                </div>
                <div class="d-flex flex-column temp mt-5 mb-3">
                    <h1 class="mb-0 font-weight-bold" id="heading"> {{ajaxCall ? cityInfo.current.temp_c : 0}}° C </h1> <span class="small grey">{{ajaxCall ? cityInfo.current.condition.text : null}}</span>
                </div>
                <div class="d-flex">
                    <div class="temp-details flex-grow-1">
                        <p class="my-1"> <img src="https://i.imgur.com/B9kqOzp.png" height="17px"> <span> {{ajaxCall ? cityInfo.current.wind_kph : null}} km/h </span> </p>
                    </div>
                    <div> <img :src="ajaxCall ? cityInfo.current.condition.icon : null" width="100px"> </div>
                </div>
            </div>
        </div>
      </section>
    </div>
    <!-- <div :style="{opacity: ajaxCall ? 1 : 0}" class="next-Page-btn text-left ml-5 mt-5">
      <NuxtLink to="/wordCounter">Word counter Page</NuxtLink>
    </div> -->
  </div>
</template>
<script>
  export default {
    data() {
      return {
        form: {
          city: '',
        },
        show: true,
        cityInfo: [],
        ajaxCall: false,
        ajaxError: false,
      }
    },

    methods: {

      routeError (){
        alert('Pls frocast a city first')
      },

      onSubmit(event) {
        event.preventDefault()
        const axios = require('axios');
        const params = {
           access_key: '6cdd2111bab947f6a2770004212201',
           city: this.form.city
         }

       axios.get(`https://api.weatherapi.com/v1/forecast.json?key=${params.access_key}&q=${params.city}`)
      .then(response => {
      let apiResponse = response.data;
      this.cityInfo = apiResponse;
      this.ajaxCall= true;
      this.ajaxError= false;
      }).catch(error => {
      console.log(error);
      this.ajaxError= true;
       });
      },
      onReset(event) {
        event.preventDefault()
        this.form.name = ''
      }
    }
  }
</script>


<style>
 .card {
    background-color: #ffffff;
    border-radius: 50px;
    color: #6f707d;
    font-family: 'Marcellus', serif;
    width: 380px;
    box-shadow: 0 2px 12px rgba(0,0,0, 0.2);
}

#heading {
    font-size: 55px;
    color: #2b304d
}

.temp {
    place-items: center
}

.temp-details>p>span,
.grey {
    color: #a3acc1;
    font-size: 12px
}

.fa {
    color: #a5a5b1
}

*:focus {
    outline: none
}
</style>