<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>About Zest</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">About Zest</ion-title>
        </ion-toolbar>
      </ion-header>
      
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';

export default  {
  name: 'Tab3',
  components: {  IonHeader, IonToolbar, IonTitle, IonContent, IonPage }
}
</script>