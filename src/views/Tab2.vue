<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Workout List</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Workout List</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-searchbar show-cancel-button="focus"></ion-searchbar>
      <ion-list>
        <ion-list-header>
          Olimpo armor
        </ion-list-header>

        <ion-item v-for="(item, index) of listOfWorkout" :key="item+index" lines="full">
          <ion-label>
            <h2>{{item}}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
      
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';

import data from '../data.json';

export default  {
  name: 'Tab2',
  components: {  IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  data() {
    return {
      listOfWorkout: Object.keys(data)
    }
  }
}
</script>