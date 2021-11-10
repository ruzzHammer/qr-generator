<template>
  <div class="generator">
    <type-selector v-on:type="getType" />
    <div class="generator__input__wrapper">
      <v-textarea
        v-if="activeTypeKind === 'text'"
        label="Text"
        rows="1"
        auto-grow
        clearable
        v-model="value"
      />
      <v-text-field
        v-else-if="activeTypeKind === 'phone'"
        v-model="value"
        class="generator__input"
        v-mask="'+7 (###) ###-####'"
      />
      <v-text-field v-else v-model="value" class="generator__input" />
      <v-btn
        @click="generateQR(value)"
        @keyup.prevent.enter="generateQR(value)"
        tabindex="-1"
        color="primary"
        >Generate</v-btn
      >
    </div>
    <p class="generator__result-text">
      QR generated for:
      <span v-if="!lastGenerated">{{ defaultValueForType }}</span>
      <span v-else>{{ lastGenerated }}</span>
    </p>
    <div class="qr-wrapper">
      <canvas id="qr"></canvas>
    </div>
    <div class="generator__options">
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            v-bind="attrs"
            v-on="on"
            color="primary"
            elevation="2"
            icon
            @click.stop="downloadQR"
          >
            <v-icon>mdi-download</v-icon>
          </v-btn>
        </template>
        <span>Download QR</span>
      </v-tooltip>
    </div>
  </div>
</template>

<script>
import TypeSelector from "./TypeSelector.vue";
import Qrious from "qrious";
import { VueMaskDirective } from "v-mask";
export default {
  components: { TypeSelector },
  directives: { mask: VueMaskDirective },
  data() {
    return {
      activeType: null,
      defaultValueForType: "",
      value: "",
      lastGenerated: "",
      qr: null,
    };
  },
  mounted() {
    this.getType;
    this.generateQR(this.defaultValueForType);
  },
  methods: {
    getType(type) {
      this.activeType = type;
      this.defaultValueForType = type.defaultValue;
      this.generateQR(this.defaultValueForType);
      this.changeValue();
    },
    changeValue() {
      let kind = this.activeTypeKind;
      if (kind === "link") this.value = "https://";
      else if (kind === "phone") this.value = "";
      else this.value = "";
    },
    generateQR(value) {
      this.qr = new Qrious({
        element: document.getElementById("qr"),
        value: value,
        size: 250,
        level: "H",
      });
      this.lastGenerated = value;
    },
    downloadQR() {
      const linkSource = this.qr.toDataURL();
      const downloadLink = document.createElement("a");
      downloadLink.href = linkSource;
      downloadLink.download = `QRCode`;
      downloadLink.click();
      downloadLink.remove();
    },
  },
  computed: {
    activeTypeKind() {
      if (this.activeType) {
        return this.activeType.kind;
      } else return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.generator {
  &__input {
    &__wrapper {
      display: flex;
      align-items: center;
      margin: 0 auto;
      max-width: 350px;
      gap: 10px;
    }
  }
  &__result-text {
    text-align: center;
    span {
      display: block;
      font-size: 18px;
    }
  }
  &__options {
    margin: 40px auto 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
.qr-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
