<script setup lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton } from '@ionic/vue';
import {
  CapacitorBarcodeScanner,
  CapacitorBarcodeScannerCameraDirection,
  CapacitorBarcodeScannerScanOrientation,
  CapacitorBarcodeScannerTypeHint,
} from '@capacitor/barcode-scanner';
import {shallowRef} from "vue";

const scanResult = shallowRef<string | undefined>(undefined);
const errorValue = shallowRef<string | undefined>(undefined);

async function scan(): Promise<void> {
  errorValue.value = undefined;
  scanResult.value = undefined;
  try {
    // SCAN QR CODE
    const result = await CapacitorBarcodeScanner.scanBarcode({
      hint: CapacitorBarcodeScannerTypeHint.QR_CODE,
      cameraDirection: CapacitorBarcodeScannerCameraDirection.BACK,
      scanInstructions: "Scan QR code",
      scanOrientation: CapacitorBarcodeScannerScanOrientation.ADAPTIVE,
    });
    scanResult.value = result.ScanResult;
  } catch (error) {
    errorValue.value = `${error}`;
  }
}
</script>


<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">

      <div class="container">
        <div>Result: {{scanResult ?? ''}}</div>
        <div>Error: {{errorValue ?? ''}}</div>
        <ion-button @click="scan">Scan</ion-button>
      </div>



    </ion-content>
  </ion-page>
</template>

<style scoped>
.container {
  line-height: 1.5;
}
</style>
