<template>
  <section class="labels-filter flex">
    <section class="labels-list">
      <div
        class="labels-container"
        v-for="(label, idx) in labels"
        :key="idx"
        :class="{ active: activeLabel === `${label.name}` }"
        @click="onLabelClick(label)"
      >
        <img class="label-img" :src="`${label.src}`" />
        <p class="label-name">
          {{ $filters.capitalizeFirstLetter(label.name) }}
        </p>
      </div>
    </section>
    <div class="filters-btn">
      <!-- <button class="labels-filter-btn btn flex" @click.stop="$emit('filters')">
        <img src="/lable-img/filter-icon.png" class="filter-img" />Filters
      </button> -->
    </div>
  </section>
</template>

<script>
export default {
  name: "label-filter",
  components: {},
  data() {
    return {
      labels: [
        {
          name: "beach",
          src: "/lable-img/beach.jpg",
        },
        {
          name: "tropical",
          src: "/lable-img/tropical.jpg",
        },
        {
          name: "amazing pools",
          src: "/lable-img/pools.jpg",
        },
        {
          name: "creative spaces",
          src: "/lable-img/creative-spaces.jpg",
        },
        {
          name: "shared homes",
          src: "/lable-img/shared_homes.jpg",
        },
        {
          name: "islands",
          src: "/lable-img/island.jpg",
        },
        {
          name: "iconic cities",
          src: "/lable-img/iconic_city.jpg",
        },
        {
          name: "surfing",
          src: "/lable-img/surfing.jpg",
        },
        {
          name: "desert",
          src: "/lable-img/desert.jpg",
        },
        {
          name: "golf",
          src: "/lable-img/golf.jpg",
        },
        {
          name: "skiing",
          src: "/lable-img/skiing.jpg",
        },
        {
          name: "grand-pianos",
          src: "/lable-img/grand-pianos.jpg",
        },
        {
          name: "luxe",
          src: "/lable-img/luxe.jpg",
        },
        {
          name: "trulli",
          src: "/lable-img/trulli.jpg",
        },
        
      ],
      activeLabel: "",
      filterBy: {
        label: "",
      },
    };
  },
  methods: {
    onLabelClick(label) {
      this.activeLabel = label.name;
      this.filterBy.label = label.name;
      this.$emit("setLabelFilter", this.filterBy);
    },
  },
  created() {},
  unmounted() {},
};
</script>
