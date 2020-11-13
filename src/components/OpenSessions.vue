<template>
  <section class="section">
    <div class="sessions">
      <div
        class="recorder"
        v-for="(recorder, key) in sessionsList"
        :key="recorder"
      >
        <h2 class="subtitle is-2"><strong>Recorder:</strong> {{ key }}</h2>
        <div
        class="session"
        v-for="(session, index) in recorder.open_sessions"
        :key="index"
      >
          <h3 class="subtitle is-3">Session #{{ index }}</h3>
          <nav class="level">
          <!-- Left side -->
            <div class="level-left">
              <div class="level-item">
                <div class="tags has-addons">
                  <span class="tag">Lifetime</span>
                  <span class="tag is-primary">{{ formatHourToLive(session.hours_to_live)}} Hours</span>
                </div>              
              </div>
              <div class="level-item">
                <div class="tags has-addons">
                  <span class="tag">Created</span>
                  <span class="tag is-primary">{{ new Date(session.timestamp).toLocaleString() }}</span>
                </div>    
              </div>
            </div>

            <!-- Right side -->
            <div class="level-right">
            </div>
          </nav>

          <a v-bind:href="serverURL + 'detail/' + key + '/' + session.id">
            <img
              class="waveform"
              :src="backendServerURL +  key + '/' + session.id + '/' + 'overview.png'"
            />
          </a>
      </div>
      </div>
    </div>
  </section>
</template>

<script>

export default {
  name: "OpenSessions",
  components: {
  },
  data() {
    return {
      serverURL: "http://server.lan:8080/",
      backendServerURL: "http://server.lan:8234/",
      sessionsList: {},
      selectedAudioFileURL: '',
      selectedArrayBufferURL: '',
    };
  },
  created() {
    console.log('backendServerURL: ' + this.backendServerURL)
    this.getSessionsList();
  },
  methods: {
    getSessionsList() {
      fetch(this.backendServerURL + 'introspect')
        .then((response) => response.json())
        .then((data) => {
          this.sessionsList = data;
          console.log(JSON.stringify(this.sessionsList));
        });
    },
    formatHourToLive(hours) {
      return Math.floor(hours)
    }
  },
};
</script>


<style scoped>

</style>
