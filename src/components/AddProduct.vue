<template>
  <div class="add-product">
    <h1 class="add-product__title">Добавление товара</h1>
    <form class="add-product__form form">
      <fieldset class="form__section section">
        <legend class="section__title _small _necessary">
          Наименование товара
        </legend>
        <input
          ref="name"
          class="section__input"
          type="text"
          placeholder="Введите наименование товара"
          id="name"
          v-model="name"
          pattern="^[^\s]+(\s.*)?$"
          required
        />
        <p v-if="name === ''" class="section__helper">{{ helperMsg }}</p>
        <p v-else-if="!this.$refs.name.checkValidity()" class="section__helper">
          {{ errorMsg }}
        </p>
      </fieldset>
      <fieldset class="form__section">
        <legend class="section__title _small">Описание товара</legend>
        <textarea
          class="section__input _more-text"
          placeholder="Введите описание товара"
          id="text"
          v-model="text"
        ></textarea>
      </fieldset>
      <fieldset class="form__section _required">
        <legend class="section__title _small _necessary">
          Ссылка на изображение товара
        </legend>
        <input
          class="section__input"
          ref="url"
          type="url"
          placeholder="Введите ссылку"
          id="url"
          v-model="url"
          required
        />
        <p v-if="url === ''" class="section__helper">{{ helperMsg }}</p>
        <p v-else-if="!this.$refs.url.checkValidity()" class="section__helper">
          {{ errorMsg }}
        </p>
      </fieldset>
      <fieldset class="form__section _required">
        <legend class="section__title _small _necessary">Цена товара</legend>
        <input
          class="section__input"
          type="text"
          placeholder="Введите цену"
          id="price"
          v-model="price"
          required
          ref="price"
          pattern="^[\s0-9\s]+$"
          @input="numberHandler"
        />
        <p v-if="price === ''" class="section__helper">{{ helperMsg }}</p>
        <p
          v-else-if="
            isNaN(Number(this.price.replace(/\s/g, ''))) ||
            Number(this.price.replace(/\s/g, '')) < 0
          "
          class="section__helper"
        >
          {{ errorMsg }}
        </p>
      </fieldset>
      <button
        class="form__button"
        :disabled="name.replace(/ /ig,'') === '' || price === '' || !this.$refs.url.checkValidity()"
        @click="
          addProduct({
            id: products.length,
            name: name,
            text: text,
            price: price,
            url: url,
          })
        "
      >
        Добавить товар
      </button>
    </form>
  </div>
</template>

<script>
/* eslint-disable */
const numberFormatter = new Intl.NumberFormat("ru-RU");
export default {
  name: "AddProduct",
  props: ["products", "addProduct"],
  data() {
    return {
      name: "",
      text: "",
      price: "",
      url: "",
      helperMsg: "Поле является обязательным",
      errorMsg: "Ввод некорректный",
    };
  },
  methods: {
    numberHandler() {
      if (!isNaN(Number(this.price.replace(/\s/g, "")))) {
        this.price = numberFormatter.format(
          Number(this.price.replace(/\s/g, ""))
        );
        if (this.price === "0") {
          this.price = "";
        }
      }
    },
  },
};
</script>


<style lang="scss" scoped>
@import '../assets/scss/vars.scss';
.add-product {
  margin-right: 16px;
  width: 332px;

  &__form {
    background: $creme;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
      0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
  }
}

.add-header {
  &__title {
    font-size: $extra-big;
    font-weight: 600;
    margin-bottom: 16px;
  }
}

.form {
  padding: 24px;
  &__section {
    margin: 0 0 16px 0;
    position: relative;
  }
  &__button {
    border-radius: 10px;
    padding: 10px 0;
    width: 100%;
    text-align: center;
    font-family: "Inter", sans-serif;
    font-weight: 600;
    font-size: $small;
    margin-top: 8px;
    background: $green;
    color: $white;
    &:disabled,
    &[disabled] {
      background: #eeeeee;
      color: $grey;
    }
  }
}

.section {
  &__title {
    margin-bottom: 4px;
  }
  &__input,
  &__input:required:valid {
    background: $creme;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    padding: 10px 16px 11px 16px;
    width: -webkit-fill-available;
    font-size: $small;
    position: relative;
  }
  &__input:required:invalid {
    border: 1px solid $red;
  }
  &__helper {
    color: $red;
    font-size: $extra-small;
    position: absolute;
    bottom: -11px;
    left: 3px;
  }
}

._small {
  font-size: $smaller;
}

._more-text {
  height: 108px;
}

._necessary {
  position: relative;
  &::after {
    position: absolute;
    top: 0;
    right: -5px;
    width: 4px;
    height: 4px;
    background: $red;
    border-radius: 4px;
    content: "";
  }
}
</style>
