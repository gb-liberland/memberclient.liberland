<template>
  <q-page class="  q-pa-md" style="overflow:hidden">
    <div class="home-page-bg q-pa-md round-borders shadow-5">
      <div
        class=" relative-position"
        style="width:40%; max-width:350px; min-width:150px"
      >
        <img style="width:100%" src="../statics/images/liberland.png" />
      </div>
      <div class="row text-text2">
        {{ $t("home.description") }}
      </div>
      <div class="row gutter-md">
        <div class="col-xs-12 col-md-4 ">
          <div class="bg-bg1 round-borders shadow-5 q-pa-md full-height box">
            <div class="q-title text-weight-light">
              Log in
            </div>
            <div class="row justify-end">
              <q-btn
                label="login"
                @click="$store.dispatch('global/login')"
                color="primary"
              />
            </div>
          </div>
        </div>
        <div class="col-xs-12 col-md-4 ">
          <div class="bg-bg1 round-borders shadow-5 q-pa-md full-height box">
            <div class="q-title text-weight-light">
              Citizenship Registration
            </div>
            <div class="row justify-end">
              <q-btn label="register" @click="" color="primary" />
            </div>
          </div>
        </div>
        <div class="col-xs-12 col-md-4">
          <div class="bg-bg1 round-borders shadow-5 q-pa-md full-height box">
            <div class="q-title text-weight-light">
              More info
            </div>
            <div class="row justify-end">
              <q-btn
                label="website"
                @click="openURL('https://liberland.org')"
                color="primary"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row justify-end q-py-md text-weight-thin">
      <span>Free Republic of Liberland</span>
    </div>
  </q-page>
</template>

<script>
import { openURL } from "quasar";
import { mapGetters } from "vuex";
export default {
  components: {},
  data() {
    return {
      email_address: "",
      language: "",
      onsubscribemsg: "",
      loading: false
    };
  },

  computed: {
    ...mapGetters({
      getAccountName: "user/getAccountName",
      getIsDark: "ui/getIsDark"
    })
  },

  methods: {
    openURL,
    login() {
      this.$store.dispatch("global/login");
    },
    logout() {
      this.$store.dispatch("global/logout");
    },

    async subscribeNewsletter() {
      this.onsubscribemsg = "";
      let data = { email: this.email_address, language: this.language };

      if (!this.$helper.isEmail(this.email_address) || this.language == "") {
        this.onsubscribemsg = this.$t("index.valid_input_required");
        return false;
      }

      let url =
        this.$helper.noBackSlash(this.$configFile.get("memberclientapi")) +
        "/subscribe";

      this.loading = true;
      try {
        let result = await this.$axios.post(url, data);
        this.onsubscribemsg = this.$t("index." + result.data.message);
        // console.log(result);
      } catch (e) {
        this.onsubscribemsg = this.$t("index.error_occured");
        console.log(e);
      }
      this.clearForm();
    },

    clearForm() {
      this.loading = false;
      this.email_address = "";
      this.language = "";
    }
  }
};
</script>

<style lang="stylus">
@import '~variables'

.box{
  border: 1px solid var(--q-color-primary)
}
.home-page-bg{
  height:600px;
  background: url('../statics/images/homepage-header.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: bottom;

}
</style>
