<template>
  <div class="app">
    <div class="app__head">
      <h2 class="app__head__title">Sorting Training System</h2>
      <button
        @click="modals.start = true"
        class="app__head__action">Start sorting!</button>
    </div>
    <div class="container">
      <div v-if="users.length"
           class="list__info">{{ users.length }} peoples in the list</div>
      <div class="list__head">
        <div class="list__data list__data--email">Email</div>
        <div class="list__data list__data--count">Potatoes</div>
        <div class="list__data list__data--tag">Tags</div>
        <div class="list__data list__data--name">Full name</div>
        <div class="list__data list__data--location">Location</div>
      </div>
      <div class="list-items">
        <draggable
          v-model="users"
          :animation="200"
          :ghost-class="'ghost'"
          @end="checkOrder"
          draggable=".list__item">
          <div v-for="user in users"
                :key="user.id"
                :class="{'list__item--selected': user.selected}"
                class="list__item">
              <div class="list__data list__data--email">
                <span @click="user.selected = !user.selected"
                      class="list__data__checkbox">
                  <svg class="list__data__checkbox__check" width="11" height="9" viewBox="0 0 11 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M1 4.25551L4.37529 7.5L10 1.5" stroke="white" stroke-width="2"/></svg>
                </span> {{ user.email }}
              </div>
              <div class="list__data list__data--count">{{ user.potatoes }}</div>
              <div class="list__data list__data--tag">
                <ul class="tag">
                  <li class="tag__item">Consumers</li>
                  <li class="tag__item">VIP</li>
                  <li class="tag__item tag__item--more">+1</li>
                </ul>
              </div>
              <div class="list__data list__data--name">{{ user.name }}</div>
              <div class="list__data list__data--location">Lithuania</div>
          </div>
        </draggable>
      </div>
      <p v-if="users.length === 0"
         class="text-center">Data not loaded yet!</p>
    </div>

    <div v-if="modals.start"
         class="modal">
      <div class="modal__content">
        <div class="modal__head">
          <p class="modal__head__title">How many people?</p>
          <svg
            @click="modals.start = false"
            class="modal__head__close" width="18" height="19" viewBox="0 0 18 19" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M11.4443 9.82843L18 3.27277L15.5557 0.82843L9 7.3841L2.44434 0.828433L0 3.27277L6.55566 9.82843L4.43441e-06 16.3841L2.44434 18.8284L9 12.2728L15.5557 18.8284L18 16.3841L11.4443 9.82843Z" fill="#999999"/></svg>
        </div>
        <div class="modal__body">
          <p class="modal__body__description">Enter a number of how many people you want to add to the list.</p>
          <input
            v-model="limit"
            class="modal__body__input"
            type="number"
            min="20"
            max="100">
        </div>
        <div class="modal__footer">
          <button
            @click="modals.start = false"
            class="btn">Cancel</button>
          <button
            @click="fetchUsers"
            :class="{'btn--disabled': (limit < 20 || limit > 100)}"
            class="btn btn--primary">Start</button>
        </div>
      </div>
    </div>
    <div v-if="modals.success"
         class="modal">
      <div class="modal__content">
        <div class="modal__head">
          <p class="modal__head__title">Success</p>
          <svg
            @click="modals.success = false"
            class="modal__head__close" width="18" height="19" viewBox="0 0 18 19" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M11.4443 9.82843L18 3.27277L15.5557 0.82843L9 7.3841L2.44434 0.828433L0 3.27277L6.55566 9.82843L4.43441e-06 16.3841L2.44434 18.8284L9 12.2728L15.5557 18.8284L18 16.3841L11.4443 9.82843Z" fill="#999999"/></svg>
        </div>
        <div class="modal__body">
          <p class="modal__body__description">Time required: {{ message }}</p>
          <p class="modal__body__description">Total moves: {{ move }}</p>
        </div>
        <div class="modal__footer">
          <button
            @click="modals.success = false"
            class="btn">Close</button>
          <button
            @click="restart"
            class="btn btn--primary">Restart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  name: 'SortingSystem',
  components: {
    draggable
  },
  data() {
    return {
      modals: {
        start: false,
        success: false,
      },
      time: null,
      move: 0,
      order: [],
      users: [],
      limit: 20,
      message: '',
      api: 'https://dummyapi.io/data/v1/user',
    };
  },
  mounted() {
  },
  methods: {
    /**
     * Get random number between number range
     * 
     * @param {Number} min
     * @param {Number} max
     * 
     * @return {Number}
     */
    getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },

    /**
     * Get users
     */
    async fetchUsers() {
      let data = await fetch(`${this.api}?limit=${this.limit}`, {
        headers: {
          'app-id': '6112dc7c3f812e0d9b6679dd'
        }
      })
      .then(res => res.json())
      .then(data => data.data)
      .catch(() => []);

      this.reset();

      let users = [];
      let order = new Set();

      data.forEach(({firstName, lastName}) => {
        let potatoes = this.getRandomInt(10, 300);

        while (order.has(potatoes)) {
          potatoes = this.getRandomInt(10, 300);
        }

        order.add(potatoes);

        let user = {
          name: `${firstName} ${lastName}`,
          email: `${firstName}${lastName}@example.co`.toLowerCase(),
          selected: false,
          potatoes
        };

        users.push(user);
      });

      this.users = users;
      this.order = [...order].sort((a, b) => (b - a));

      this.time = Date.now();
      this.modals.start = false;
    },

    /**
     * Get readable date from now by given past date
     * 
     * @param {Object} start
     * 
     * @return {String}
     */
    getReadableTimeFromNow(start) {
      let MILISECONDS = 1000;
      let YEAR_IN_SECONDS = 31536000;
      let MONTHS_IN_SECONDS = 2592000;
      let DAY_IN_SECONDS = 86400;
      let HOUR_IN_SECONDS = 3600;
      let MINUTES_IN_SECONDS = 60;

      let TIME_LENGTHS = [
        {seconds: YEAR_IN_SECONDS, term: 'years'},
        {seconds: MONTHS_IN_SECONDS, term: 'months'},
        {seconds: DAY_IN_SECONDS, termterm: 'days'},
        {seconds: HOUR_IN_SECONDS, term: 'hours'},
        {seconds: MINUTES_IN_SECONDS, term: 'minutes'},
        {seconds: 1, term: 'seconds'}
      ];

      let messageJoins = [];
      let now = new Date();
      let diffInSeconds = parseInt((now.getTime() - start.getTime()) / MILISECONDS)
      
      for (let tl of TIME_LENGTHS) {
        if (diffInSeconds >= tl.seconds) {
          let num_of_times = (diffInSeconds / tl.seconds).toFixed(0);

          diffInSeconds = diffInSeconds % tl.seconds;

          let term = (num_of_times > 1) ? tl.term : tl.term.substring(0, tl.term.length - 1);

          messageJoins.push(`${num_of_times} ${term}`);
        }
      }

      return messageJoins.join(', ');
    },

    /**
     * Check wheather sorting done or not
     */
    checkOrder() {
      let ordered =  this.users.every((user, index) => (this.order[index] === user.potatoes));

      this.move++;

      if (!ordered) {
        return;
      }

      this.message = this.getReadableTimeFromNow(new Date(this.time));
      this.modals.success = true;
    },

    /**
     * Restart sorting
     */
    restart() {
      this.reset();
      this.modals.success = false;
      this.modals.start   = true;
    },

    /**
     * Reset necessary data to default value
     */
    reset() {
      this.time    = null;
      this.move    = 0;
      this.order   = [];
      this.users   = [];
      this.limit   = 20;
      this.message = '';
    },
  }
}
</script>
