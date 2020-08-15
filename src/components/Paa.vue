<template>
    <div class="paa">
        <div class="textrow">
            <h1 class="head">
                {{ datenow }}
            </h1>
            <div>
                <input
                    v-model="country"
                    class="location">
            </div>
            <h1 class="head">
                {{ client.forecast[0].weather_code.value }}
            </h1>
        </div>
        <div class="textrow2">
            <div class="temps">
                <div class="trend">
                    <trend
                        :data="shit(client.forecast)"
                        gradient-direction="top"
                        :gradient="['#d6d6d6', '#d6d6d6', '#d6d6d6']"
                        :padding="1"
                        :radius="6.5"
                        :stroke-width="1"
                        stroke-linecap="square"
                        auto-draw
                        smooth />
                </div>
                <div class="value">
                    <h2 class>
                        {{ client.forecast[0].temp.value + " °" + client.forecast[0].temp.units }}
                    </h2>
                </div>
            </div>
            <div class="temps">
                <div class="trend">
                    <trend
                        :data="shit2(client.forecast)"
                        gradient-direction="top"
                        :gradient="['#d6d6d6', '#d6d6d6', '#d6d6d6']"
                        :padding="1"
                        :radius="6.5"
                        :stroke-width="1"
                        stroke-linecap="square"
                        auto-draw
                        smooth />
                </div>
                <div class="value">
                    <h2
                        class>
                        {{ client.forecast[0].wind_speed.value + " " + client.forecast[0].wind_speed.units }}
                    </h2>
                </div>
            </div>
        </div>
        <div class="joku">
            <h4
                class="quote">
                "Tähän joku motivational quotepalikkoa joka hakee niitä quoteja vaikka jostain apista tms. Pelkkä array riittäs kans joita loopataan css animaatioilla sitte tässä siististi."
            </h4>
            <h4 class="name">
                - Santeri pigg
            </h4>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import Trend from 'vuetrend'

import { locationOptions } from '../options.js'

export default {
    name: 'Paa',
    components: {
        Trend
    },
    props: {},
    data () {
        return {
            client: {
                ip: null,
                city: 'no location',
                forecast: [],
                location: {
                    lat: null,
                    lon: null,
                    temp: {
                        value: null,
                        units: null
                    },
                    wind_speed: {
                        value: null,
                        units: null
                    },
                    precipitation: {
                        value: null,
                        units: null
                    },
                    wind_direction: {
                        value: null,
                        units: null
                    },
                    observation_time: {
                        value: null
                    },
                    weather_code: {
                        value: 123
                    }
                }
            },
            country: null,
            datenow: '',
            weather: ''
        }
    },
    async mounted () {
        this.getGeoLocation()
        this.interval = setInterval(this.time, 1000)
    // axios
    //   .get(
    //   )
    //   .then(response => (this.weather = response));
    },
    beforeDestroy () {
        clearInterval(this.interval)
    },
    methods: {
        getGeoLocation () {
            navigator.geolocation.getCurrentPosition(GEO => {
                // https://api.bigdatacloud.net/data/reverse-geocode-client?latitude=37.42159&longitude=-122.0837&localityLanguage=en
                axios
                    .get('https://api.bigdatacloud.net/data/reverse-geocode-client', {
                        params: {
                            latitude: GEO.coords.latitude,
                            longitude: GEO.coords.longitude
                        }
                    })
                    .then(response => {
                        this.client.location.lat = GEO.coords.latitude
                        this.client.location.lon = GEO.coords.latitude
                        console.log(response.data.locality)
                        this.country = response.data.city
                        this.getWeather()
                    })
                    .catch(error => {
                        console.log(error)
                    })
            }, error => {
                console.log('error:', error, 'You did not allow your location, trying to get location by IP')
                this.getIPLocation()
            }, locationOptions)
        },
        getIPLocation () {
            axios
                .get('http://ip-api.com/json/', {
                    params: {}
                })
                .then(response => {
                    this.country = response.data.country
                    this.client.location.lat = response.data.lat
                    this.client.location.lon = response.data.lon
                    this.getWeather()
                })
                .catch(error => {
                    console.log(error)
                    this.client.country = 'please disable adblock for site to work'
                })
                .then(function () {
                    // always executed
                })
        },
        getWeather () {
            axios
                .get('https://api.climacell.co/v3/weather/nowcast', {
                    params: {
                        lat: this.client.location.lat,
                        lon: this.client.location.lat,
                        unit_system: 'si',
                        timestep: 60,
                        start_time: 'now',
                        fields: 'temp,weather_code,precipitation,wind_speed,wind_direction',
                        apikey: '0M1fYVKYIDVK4JE1cLQvvpNVxmdG0Btn'
                    }
                })
                .then(response => {
                    this.client.forecast = response.data
                })
                .catch(error => {
                    console.log(error)
                })
                .then(function () {
                    // always executed
                })
        },
        shit (theShit) {
            const theGoodShit = []
            theShit.forEach(shit => {
                theGoodShit.push(shit.temp.value)
            })
            return theGoodShit
        },
        shit2 (theShit) {
            const theGoodShit = []
            theShit.forEach(shit => {
                theGoodShit.push(shit.wind_speed.value)
            })
            return theGoodShit
        },
        time () {
            this.datenow = moment().format('HH:mm')
        }
    }
}
</script>
<style lang="scss" scoped>
.temps {
  width: 30%;
  height: auto;
  align-items: center;
  .trend {
    width: 100%;
  }
  .value {
    text-align: center;
  }
}
.location {
  background-color: transparent;
  border: none;
  color: whitesmoke;
  font-family: "Lato", sans-serif;
  font-weight: 100;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-size: 48px;
  text-transform: uppercase;
  transition: background-color 0.5s;
  &:focus {
    outline: none;
    background-color: rgba(27, 27, 27, 0.171);
  }
  &::selection {
    background: #ffb7b73a;
  }
}
.head {
  width: 40%;
}
.textrow {
  display: flex;
  justify-content: space-between;
  margin: 0 0 100px 0;
  height: 30%;
  width: auto;
}
.textrow2 {
  display: flex;
  justify-content: space-around;
  margin: 0 0 100px 0;
  height: 30%;
  width: auto;
}
.joku {
  text-align: start;
  margin: 50px;
}
.name {
  font-size: 24px !important;
  text-transform: none;
}
.quote {
  font-size: 24px !important;
  text-transform: none;
  font-style: italic;
}
</style>
