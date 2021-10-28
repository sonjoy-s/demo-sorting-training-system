<template>
  <div class="app">
    <div class="app__head">
      <h2 class="app__head__title">Sorting Training System</h2>
      <button class="app__head__action">Start sorting!</button>
    </div>
    <div class="container">
      <div class="list__info">20 people in the list</div>
      <div class="list__head">
        <div class="list__data list__data--email">Email</div>
        <div class="list__data list__data--count">Potatoes</div>
        <div class="list__data list__data--tag">Tags</div>
        <div class="list__data list__data--name">Full name</div>
        <div class="list__data list__data--location">Location</div>
      </div>
      <div class="list-items">
        <div v-for="item in [1,2,3,4,5]"
            :key="item"
            :class="{'list__item--selected': (Math.random() > .8)}"
            class="list__item">
          <div class="list__data list__data--email">
            <span class="list__data__checkbox">
              <svg class="list__data__checkbox__check" width="11" height="9" viewBox="0 0 11 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M1 4.25551L4.37529 7.5L10 1.5" stroke="white" stroke-width="2"/></svg>
            </span> juozas@bybosas.lt
          </div>
          <div class="list__data list__data--count">1</div>
          <div class="list__data list__data--tag">
            <ul class="tag">
              <li class="tag__item">Consumers</li>
              <li class="tag__item">VIP</li>
              <li class="tag__item tag__item--more">+1</li>
            </ul>
          </div>
          <div class="list__data list__data--name">Juozas Bybosas</div>
          <div class="list__data list__data--location">Lithuania</div>
        </div>
      </div>
    </div>
    

    <div v-if="modal"
         class="modal">
      <div class="modal__content">
        <div class="modal__head">
          <p class="modal__head__title">How many people?</p>
          <svg class="modal__head__close" width="18" height="19" viewBox="0 0 18 19" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M11.4443 9.82843L18 3.27277L15.5557 0.82843L9 7.3841L2.44434 0.828433L0 3.27277L6.55566 9.82843L4.43441e-06 16.3841L2.44434 18.8284L9 12.2728L15.5557 18.8284L18 16.3841L11.4443 9.82843Z" fill="#999999"/></svg>
        </div>
        <div class="modal__body">
          <p class="modal__body__description">Enter a number of how many people you want to add to the list.</p>
          <input class="modal__body__input" type="number">
        </div>
        <div class="modal__footer">
          <button class="btn">Cancel</button>
          <button class="btn btn--primary">Start</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SortingSystem',
  props: {
    msg: String
  },
  data() {
    return {
      modal: true,
    };
  },
  methods: {}
}
</script>
