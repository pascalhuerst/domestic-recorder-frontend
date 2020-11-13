<template>
  <section class="section">
    <div class="sessions">
      <div
        class="recorder"
        v-for="(recorder, key) in sessionsList"
        :key="recorder"
      >
        
        <h2 class="subtitle is-2"> {{ key }} </h2>
        
        <div
        class="block"
        v-for="(session, index) in recorder.open_sessions"
        :key="index"
      >
          <nav class="level">
            <!-- Left side -->
            <div class="level-left">
              <div class="level-item">
                <h3 class="subtitle is-3">Session #{{ index }}</h3>
              </div>
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
              <div class="level-item">
                <span class="tag is-danger">
                   Delete Session 
                  <button class="delete is-small" @click="deleteSession(key, session.id)"></button>
                </span>
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
          <div class="vspace">
          </div>
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
      serverURL: "http://localhost:8080/",
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
    },
    deleteSession(recorderID, sessionID) {

      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          recorderID: recorderID,
          sessionID: sessionID,
        })
      };

      fetch(this.backendServerURL + 'delete', requestOptions)
        .then(response => {
          console.log(JSON.stringify(response));
          this.getSessionsList();
        });
    }
  },
};
</script>


<style scoped>
.vspace {
  padding-top: 50px;
}
</style>
