<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>WOD</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">WOD</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <CircuitTimer name="Jupiter" />
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import CircuitTimer from '@/components/CircuitTimer.vue';

export default  {
  name: 'Tab1',
  components: { CircuitTimer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage }
}
</script>
