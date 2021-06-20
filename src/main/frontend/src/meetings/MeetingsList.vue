<template>
  <table v-if="meetings.length > 0">
    <thead>
    <tr>
      <th>Nazwa spotkania</th>
      <th>Opis</th>
      <th>Uczestnicy</th>
      <td></td>
    </tr>
    </thead>
    <tbody>
    <tr v-for="meeting in meetings" :key="meeting.title">
      <td>{{ meeting.title }}</td>
      <td>{{ meeting.date }}</td>
      <td>{{ meeting.description }}</td>
      <td>
        <ul v-if="meeting.participants">
          <li v-for="participant in meeting.participants" :key="participant.login">
            {{ participant }}
          </li>
        </ul>
      </td>
      <td style="text-align: right; min-width: 400px">
        <button v-if="meeting.participants.indexOf(username) < 0" class="button-outline"
                @click="$emit('attend', meeting)">
          Zapisz się
        </button>
        <button v-else class="button-outline" @click="$emit('unattend', meeting)">Wypisz się</button>
        <button v-if="meeting.participants.length === 0" class="button" @click="$emit('delete', meeting)">
          Usuń puste spotkanie
        </button>
      </td>
    </tr>
    </tbody>
  </table>
</template>

<script>
    export default {
        props: ['meetings', 'username'],
        methods: {
            addNewMeeting() {
                this.error = false;
                if (this.newMeeting.title) {
                  this.$http.post('meetings', this.newMeeting)
                    .then(() => {
                        this.success('Spotkanie zostało założone. Możesz się na nie zapisać.');
                        this.registering = false;
                    })
                    .catch(response => this.failure('Błąd przy zakładaniu spotkania. Kod odpowiedzi: ' + response.status));
                    this.$emit('added', this.newMeeting);
                    this.newMeeting = {participants: []};
                    this.adding = false;
                } else {
                    this.error = true;
                }
            }
        }
    }
    
</script>
