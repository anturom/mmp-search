<template>
  <div id="app" class="hfeed site">
    <NavigationBar></NavigationBar>
    <SearchManuscripts></SearchManuscripts>
    <FooterBar></FooterBar>
    <FooterImprint></FooterImprint>
  </div>
</template>

<script>
import NavigationBar from "./components/NavigationBar";
import SearchManuscripts from "./components/SearchManuscripts";
import FooterBar from "./components/FooterBar";
import FooterImprint from "./components/FooterImprint";

export default {
  name: "App",
  components: {
    NavigationBar,
    SearchManuscripts,
    FooterBar,
    FooterImprint
  },
};
</script>
