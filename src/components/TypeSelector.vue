<template>
  <div class="generator__selector">
    <p>Select a type of info to qr above:</p>
    <ul class="generator__selector-list">
      <li v-for="type in generatorTypes" :key="type.name">
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              v-bind="attrs"
              v-on="on"
              color="primary"
              elevation="2"
              icon
              @click.stop="selectType(type)"
              :class="{
                active: activeType === type,
              }"
            >
              <v-icon>{{ type.icon }}</v-icon>
            </v-btn>
          </template>
          <span>{{ type.name }}</span>
        </v-tooltip>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      generatorTypes: [
        {
          name: "Link",
          kind: "link",
          icon: "mdi-link",
          defaultValue: "https://github.com/ruzzHammer",
        },
        {
          name: "Plain text",
          kind: "text",
          icon: "mdi-text-recognition",
          defaultValue: "Hello World!",
        },
        {
          name: "Phone number",
          kind: "phone",
          icon: "mdi-phone-dial",
          defaultValue: "+7 (900) 000-0000",
        },
      ],
      activeType: null,
    };
  },
  mounted() {
    this.selectType(this.generatorTypes[0]);
  },
  methods: {
    selectType(type) {
      this.$emit("type", type);
      this.activeType = type;
    },
  },
};
</script>

<style lang="scss" scoped>
.generator__selector {
  text-align: center;
  margin-bottom: 20px;
  &-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
    gap: 15px 40px;
    max-width: fit-content;
    li {
      display: flex;
      justify-content: center;
      align-items: center;
      flex: 1;
      .v-btn {
        &.active {
          background: var(--v-primary-base);
          color: #fff !important;
        }
      }
    }
  }
}
</style>
