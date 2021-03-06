<template>
  <Screen class="settings">
    <DashboardContent>
      <DashboardTitle class="settings__title">{{ language.settings }}</DashboardTitle>
      <Label class="settings-label">
        {{ language.currency }}
        <sp-dropdown class="dropdown-wrapper" :label="module.settings.currency">
          <sp-menu>
            <sp-menu-item
              v-for="(currency, index) in currencies"
              :key="'cur' + index"
              v-on:click="currencySelected(index)"
            >{{ currency }}</sp-menu-item>
          </sp-menu>
        </sp-dropdown>
      </Label>
      <Label class="settings-label">
        {{ language.language }}
        <div>
          <sp-dropdown class="dropdown-wrapper" :label="module.settings.language">
            <sp-menu>
              <sp-menu-item
                v-for="(lang, index) in languages"
                :key="'lang' + index"
                v-on:click="languageSelected(index)"
              >{{ lang }}</sp-menu-item>
            </sp-menu>
          </sp-dropdown>
        </div>
      </Label>
    </DashboardContent>

    <Footer>
      <ButtonSecondary :click="showSeed">{{ language.show_seed }}</ButtonSecondary>
      <ButtonSecondary class="exit-button" :click="closeWallet">
        <img src="../assets/images/exit.svg" alt="exit">
        {{language.exit_wallet }}
      </ButtonSecondary>
    </Footer>
  </Screen>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";
import WalletHandlerModule from "@/store/modules/WalletHandlerModule";
import DashboardTitle from "@/components/Text/DashboardTitle.vue";
import Label from "@/components/Form/Label.vue";
import DashboardContent from "@/components/Layout/DashboardContent.vue";
import Screen from "@/components/Layout/Screen.vue";
import Footer from "@/components/Layout/Footer.vue";
import ButtonSecondary from "@/components/Buttons/ButtonSecondary.vue";

/* eslint-disable no-unused-vars */

import Language from "@/lang/langInterface";

/* eslint-enable no-unused-vars */

@Component({components: { DashboardTitle, Label, Screen, Footer, DashboardContent, ButtonSecondary }})
export default class Settings extends Vue {
  @Prop() language!: Language;

  // Probably should have these in a config file, but cba bothering to load it in, so just keep it here
  private currencies = [
    "USD",
    "AUD",
    "BRL",
    "CAD",
    "CHF",
    "CLP",
    "CNY",
    "DKK",
    "EUR",
    "GBP",
    "HKD",
    "INR",
    "ISK",
    "JPY",
    "KRW",
    "NZD",
    "PLN",
    "RUB",
    "SEK",
    "SGD",
    "THB",
    "TRY",
    "TWD",
  ];
  private languages = ["English", "Español", "Français"];
  private module = WalletHandlerModule;

  async currencySelected(index: number) {
    await WalletHandlerModule.changeCurrency(this.currencies[index]);
    this.$emit("currency-changed");
  }

  async languageSelected(index: number) {
    await WalletHandlerModule.changeLanguage(this.languages[index]);
    this.$emit("language-changed");
  }

  closeWallet() {
    this.$emit("close");
  }

  showSeed() {
    this.$emit("show-seed");
  }
}
</script>

<style>
  .dropdown-wrapper {
    width: 100%;
  }
  sp-dropdown {
    --spectrum-dropdown-height: 40px;
    --spectrum-dropdown-padding-x: 16px;
    --spectrum-fieldbutton-background-color: #090C14;
    --spectrum-fieldbutton-background-color-hover: #090C14;
    --spectrum-fieldbutton-background-color-down: #090C14;
    --spectrum-dropdown-placeholder-text-color-hover: #fff;
    --spectrum-global-dimension-size-50: 2px;
    --spectrum-fieldbutton-border-color: #2B2F3A;
    --spectrum-fieldbutton-border-color-hover: #2B2F3A;
    --spectrum-fieldbutton-border-color-down: #2B2F3A;
    --spectrum-fieldbutton-text-color: #fff;
    --spectrum-fieldbutton-text-color-hover: #fff;
    --spectrum-dropdown-placeholder-text-color: #fff;
    --spectrum-dropdown-placeholder-text-font-style: normal;
    --spectrum-dropdown-icon-color-hover: #7E858F;
  }
  sp-popover {
    max-height: 250px;
    transform: translateY(0);
    --spectrum-popover-background-color: #090C14;
    --spectrum-popover-border-color: #2B2F3A;
  }
  sp-menu {
    padding: 15px 0;
    border-radius: 2px;
  }
  sp-menu-item {
    --spectrum-selectlist-option-text-color-selected: #F7931A;
    --spectrum-selectlist-option-padding: 16px;
    --spectrum-selectlist-option-text-color: #ACB2BB;
    --spectrum-selectlist-option-text-color-hover: #F7931A;
    --spectrum-selectlist-option-text-size: 14px;
    --spectrum-selectlist-option-icon-color-selected: transparent;
    --spectrum-selectlist-option-background-color-hover: #13161F;
  }
  sp-menu-item[selected] {
    display: block;
    background-color: #13161F;
  }
  .settings-label + .settings-label {
    margin-top: 16px;
  }
  .dashboard__content {
    padding: 0;
  }
  .settings__title {
    margin-bottom: 40px;
  }
  button.btn-secondary.exit-button {
    display: flex;
    align-items: center;
  }
  .exit-button img {
    margin-right: 8px;
  }
</style>
