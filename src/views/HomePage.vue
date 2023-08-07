<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-title>
                    <ion-icon :icon="sunnyOutline"></ion-icon>
                    Control de Ventilador</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content :fullscreen="true">
            <ion-header collapse="condense">
                <ion-toolbar>
                    <ion-title size="large" class="ion-text-center">
                        Control de Ventilador
                    </ion-title>
                </ion-toolbar>
            </ion-header>



            <div id="container">
                <div class="weather-container">
                    <div class="temperature">
                        <strong>Temperatura</strong>
                        <p>{{ temperature }}°C</p>
                    </div>


                    <div class="pressure">
                        <strong>Presión</strong>
                        <p>{{ pressure }}%</p>
                    </div>

                    <!--
                    <div class="humidity">
                        <strong>Humedad</strong>
                        <p>{{ humidity }}%</p>
                    </div>
                    -->
                </div>

                <ion-grid class="ion-margin-top">
                    <ion-row>
                        <ion-col>
                            <div class="switch-container ion-text-center">
                                <ion-toggle @ion-change="toggle" :checked="checked" color="success"></ion-toggle>
                            </div>
                        </ion-col>
                    </ion-row>

                    <ion-row>
                        <ion-col>
                            <div class="status-container ion-margin-top">
                                Ventilador
                                <span class="status-on" v-if="checked">Encendido</span>
                                <span class="status-off" v-if="!checked">Apagado</span>
                            </div>
                        </ion-col>
                    </ion-row>
                </ion-grid>


            </div>
        </ion-content>
    </ion-page>
</template>

<script lang="ts">
import { IonItem, IonIcon, IonLabel, IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonToggle, IonRow, IonCol, IonGrid } from '@ionic/vue';
import { sunnyOutline } from 'ionicons/icons';
import { defineComponent, ref } from 'vue';

export default defineComponent({
    components: { IonIcon, IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonItem, IonLabel, IonToggle, IonRow, IonCol, IonGrid },
    setup() {
        const checked = ref(false);
        const temperature = ref(0);
        const pressure = ref(0);
        const humidity = ref(0);

        const token = '';

        const temperaturePath = 'api/states/sensor.esphome_web_827e67_bme280_temperature'
        const pressurePath = 'api/states/sensor.esphome_web_827e67_bme280_pressure'
        const turnOnPath = 'api/services/switch/turn_on';
        const turnOffPath = 'api/services/switch/turn_off';
        const urlBase = 'http://172.18.0.2:8123';

        const headers = {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
            'Authorization': 'Bearer ' + token,
        };

        const getTemperature = async () => {
            const url = `${urlBase}/${temperaturePath}`;

            fetch(url, {
                method: 'GET',
                headers: headers,
            })
                .then(res => res.json())
                .then(data => {
                    temperature.value = data.state;
                })
                .catch(error => {
                    console.log(error);
                })
        }

        const getPressure = async () => {
            const url = `${urlBase}/${pressurePath}`;

            fetch(url, {
                method: 'GET',
                headers: headers,
            })
                .then(res => res.json())
                .then(data => {
                    pressure.value = Math.round(data.state);
                })
                .catch(error => {
                    console.log(error);
                })
        }

        getTemperature();
        getPressure();



        const toggle = async () => {
            checked.value = !checked.value;

            const url = `${urlBase}/${checked.value ? turnOnPath : turnOffPath}`;

            fetch(url, {
                method: 'POST',
                headers: headers,
                body: JSON.stringify({
                    "entity_id": "switch.esphome_web_827e67_rel_0"
                })
            })
                .then(res => res.json())
                .then(data => {
                    //console.log(data);
                })
                .catch(error => {
                    //console.log(error);
                });

            getTemperature();
            getPressure();

            // TODO? - Realizar otra petición para el estado real???
        };


        return { sunnyOutline, checked, temperature, humidity, toggle, pressure };
    },
});
</script>

<style scoped>
#container {
    text-align: center;

    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
}

.weather-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1.5rem;

    width: 100%;
    max-width: 315px;

    margin: 0 auto;
}

.temperature,
.humidity,
.pressure {
    color: #8c8c8c;
    width: 50%;
}

.temperature p,
.humidity p,
.pressure p {
    font-size: 3rem;
    margin-top: 15px;
    margin-bottom: 35px;
}

.temperature p {
    color: #eaac1d;
}

.pressure p {
    color: rgb(54, 138, 228);
}

.humidity p {
    color: rgb(54, 138, 228);
}

ion-toggle {
    padding: 12px;

    --track-background: #ddd;
    --track-background-checked: #ddd;

    --handle-background: #eb7769;
    --handle-background-checked: #95c34e;

    --handle-width: 155px;
    --handle-height: 187px;
    --handle-max-height: auto;
    --handle-spacing: 12px;

    --handle-border-radius: 14px;
    --handle-box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.16);
}

ion-toggle::part(track) {
    height: 120px;
    width: 315px;

    /* Required for iOS handle to overflow the height of the track */
    overflow: visible;
}

.status-container {
    font-size: 2rem;
    line-height: 56px;

    color: #8c8c8c;
}

.status-on {
    color: #95c34e;
}

.status-off {
    color: #eb7769;
}
</style>
