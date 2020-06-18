

<template>
  <web-view :source="{uri: webViewUrl }" />
</template>

<script>
import { WebView } from "react-native-webview";
import firebase from "react-native-firebase";

let error_url = 'https://invoicemanager.ro/fcm-error.php?error=';
let request_permission_result_no_url = 'https://invoicemanager.ro/no_permission.php';
let url = 'https://invoicemanager.ro/auth.php?app=true&token=';
let fcmToken = "none";
  
export default {

  data() {
    return {
      webViewUrl: ""
    };
  },
  methods: {
    async setToken() {
      let token = firebase.messaging().getToken().then(fcmToken => {
        this.webViewUrl = url + fcmToken;
      }).catch(err => {
        this.webViewUrl = error_url + err.toString();
      });
    },
    async requestPermission() {
      firebase
        .messaging()
        .requestPermission()
        .then(() => {
           this.setToken();
        })
        .catch(error => {
          this.webViewUrl = request_permission_result_no_url;
        });
    },
    async getFCM() {
      
      await firebase
      .messaging()
      .hasPermission()
      .then( async (hasPermission) => {
        if (hasPermission) {
          this.setToken();
        } else {
          this.requestPermission();
        }
        
      });
    }
  },
  mounted: async function() {
    this.getFCM();
  },
  name: "MyComponent",
  components: {
    "web-view": WebView
  }
};
</script>
