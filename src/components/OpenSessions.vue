<template>
  <section class="section">
    <div class="sessions">
      <h1 class="title">Available Sessions</h1>
      <div
        class="session"
        v-for="(session, index) in sessionsList"
        :key="session.ID"
      >
        
        <h2 class="subtitle is-2">Session #{{ index }}</h2>
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
        
        <a v-bind:href="serverURL + 'detail/domestic-recorder-3/' + session.id">
          <img
            class="waveform"
            :src="backendServerURL +  'domestic-recorder-3' + '/' + session.id + '/' + 'overview.png'"
          />
        </a>
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
      sessionsList: [],
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
          this.sessionsList = data["domestic-recorder-3"].open_sessions;
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
