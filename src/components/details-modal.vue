<template>
  <div class="stay-order">
    <div class="order-modal-container">
      <div class="order-modal flex">
        <div class="mini-modal-container">
          <div class="order-form-header flex">
            <div>
              <span class="stay-details-price bold">${{ stay.price }}</span>
              night
            </div>
            <div class="reviews-preview flex">
              <div class="star-preview">
                <img src="../styles/icons/star.svg" class="star" />
              </div>
              <span class="review-avg">4.7</span>·
              <span class="total-reviews">3 reviews</span>
            </div>
          </div>
        </div>
        <form class="order-form">
          <div class="dates-pick flex">
            <div class="date-picker-container-left">
              <button class="check-in" @click.prevent="isCalendarShown = !isCalendarShown">
                <div class="order-button">CHECK-IN</div>
                <span class="calender-pick">{{ checkInDate }}</span>
              </button>
            </div>
            <div class="date-picker-container-right">
              <button class="check-out" @click.prevent="isCalendarShown = !isCalendarShown">
                <div class="order-button">CHECKOUT</div>
                <span class="calender-pick">{{ checkOutDate }}</span>
              </button>
            </div>
          </div>
          <div class="guest-input">
            <button @click.prevent="isGuestModalShown = !isGuestModalShown" class="guests">
              <label class="order-button">GUESTS</label>
              <span class="guest-num">{{ totalGuests }}</span>
              <div class="expand-btn">
                <img src="../styles/icons/expand-more.png" class="expand-more" />
              </div>
            </button>
            <div></div>
          </div>
          <!-- <router-link to="/reservation"> -->
          <fancy-btn class="reserve-btn" @click.prevent="reservation">Reserve</fancy-btn>
          <!-- </router-link> -->
          <div class="pricing">
            <p>You won't be charged yet</p>
            <p>
              <span>Price</span>
              <span>${{ $filters.formatNumber(stayPrice) }}</span>
            </p>
            <p>
              <span>Service fee</span>
              <span>${{ stay.cleaningFee }}</span>
            </p>
            <p>
              <span>Total</span>
              <span>${{ $filters.formatNumber(totalFair) }}</span>
            </p>
          </div>
        </form>
      </div>
    </div>
  </div>
  <transition>
    <calender-spread class="calender-container" @closeCalendar="isCalendarShown = false" @dateChange="dateUpdate"
      @click.prevent is-expanded v-if="isCalendarShown">
    </calender-spread>
  </transition>

  <transition>
    <guests-picker class="guest-pick" v-if="isGuestModalShown" @guestsUpdate="updateGuests"
      @closeGuestsModal="isGuestModalShown = false" />
  </transition>
</template>

<script>
import { stayService } from "../services/stay-service";
import calenderSpread from "../components/calender-spread.vue";
import guestsPicker from "../components/guests-picker.cmp.vue";
import fancyBtn from "../components/fancy-btn.cmp.vue";
import reserveModal from "./reserve-modal.vue";

export default {
  props: {
    stay: Object,
  },
  emits: ["makeReservation"],
  components: {
    guestsPicker,
    calenderSpread,
    fancyBtn,
    reserveModal,
  },
  data() {
    return {
      date: {
        start: null,
        end: null,
      },

      guests: {
        adults: 0,
        children: 0,

        total: 0,
      },
      isCalendarShown: false,
      isGuestModalShown: false,
    };
  },
  methods: {
    sumGuests() {
      this.guests.total = this.guests.adults + this.guests.children;
    },
    updateAdults(num) {
      this.guests.adults = num;
      this.sumGuests();
    },
    updateChildren(num) {
      this.guests.children = num;
      this.sumGuests();
    },
    dateUpdate(date) {
      console.log('date', this.date)
      this.date = date;
      this.$store.dispatch({type: "updateDate" , date: this.date})
    },
    reservation() {
      console.log('clicked')
      this.$emit("makeReservation");
      this.$router.push('/reservation')
    },
  },
  computed: {
    checkInDate() {
      return this.date.start
        ? this.date.start.toLocaleDateString("en-US", {
          year: "numeric",
          month: "2-digit",
          day: "2-digit",
        })
        : "Add date";
    },

    checkOutDate() {
      return this.date.end
        ? this.date.end.toLocaleDateString("en-US", {
          year: "numeric",
          month: "2-digit",
          day: "2-digit",
        })
        : "Add date";
    },
    // totalGuests() {
    //   return this.guests.total > 0 ? this.guests.total : "1 guest";
    // },
    totalGuests() {
      const { total } = this.$store.getters.getGuests;
      return total > 0 ? `${total} guests` : "1 guest";
    },
    stayPrice() {
      const nightFee = this.$props.stay.price;
      const totalNights = (this.date.end - this.date.start) / 86400000;
      return nightFee * totalNights;
    },
    totalFair() {
      const nightFee = this.$props.stay.price;
      const totalNights = (this.date.end - this.date.start) / 86400000;
      return nightFee * totalNights + this.$props.stay.cleaningFee;
    },
  },
  created() {
    // let stayToOrder = this.$store.getters.getStayToOrder;
    this.date = this.$store.getters.getDate;
    this.guests = this.$store.getters.getGuests;
  },
  unmounted() { },
};
</script>
