<template>
    <div>
        <div class="location">{{ location }}</div>
        <div class="weather">{{ weather }}</div>
        <div class="temp">{{ temp }}℃</div>
    </div>
</template>

<script>
    export default {
        name: 'current-weather',
        data () {
            return {
                location: '',
                weather: '',
                temp: '',
                lat: 37.566535,
                lon: 126.977969
            }
        },
        mounted () {
            // this.$bus.$on('sendCoordinate', (payload) => {
            //     // console.log(payload)
            // })
            this.$bus.$on('sendCoordinate', this.setCoordinate)
        },
        methods: {
            setCoordinate(coordinate) {
                this.lat = coordinate.lat;
                this.lon = coordinate.lng;

                this.axios.get('http://api.openweathermap.org/data/2.5/weather', {
                    params: {
                        lat: this.lat,
                        lon: this.lon,
                        APPID: 'b24e89ec4cb066050bbd43edc2158493'
                    }
                }).then(response => {
                    let data = response.data
                    this.location = data.name
                    this.weather = data.weather[0].main
                    this.temp = (data.main.temp - 273.15).toFixed(0)
                })
            }
        }
    }
</script>

<style scoped>
    .location {
        text-align: center;
        font-size: 40pt;
        color: #fff;
    }
    .weather {
        text-align: center;
        font-size: 20pt;
        color: #fff;
    }
    .temp {
        text-align: center;
        font-size: 50pt;
        color: #fff;  
    }
</style>