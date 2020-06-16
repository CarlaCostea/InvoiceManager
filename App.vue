<template>
    <web-view :source="{uri:'https://invoicemanager.ro/auth.php?app=true'}" />
</template>

<script>
import { WebView } from "react-native-webview";

export default {
  name: "myComponent",
  components: {
    "web-view": WebView
  }
};
</script>