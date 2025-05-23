<script setup lang="ts">
import { ref } from "vue";
import dayjs from "dayjs";
interface Props {
  id: string;
  label: string;
  placeholder: string;
  type?: string;
  icon?: string;
  required?: boolean;
  autofocus?: boolean;
  error?: string;
  name: string;
  autocomplete?: "on" | "off";
}

withDefaults(defineProps<Props>(), {
  type: "text",
  required: true,
  autofocus: false,
  autocomplete: "on",
});

const model = defineModel<string | number>();
const inputRef = ref<HTMLInputElement | null>(null);

function triggerDatepicker() {
  inputRef.value?.showPicker();
}

const dateAsString = computed(() => {
  if (!model.value) return "";
  return dayjs(model.value).format("MMMM DD, YYYY [at] h:mm A");
});

function changePassengers(amount: number) {
  model.value = Number(model.value) + amount;
}
</script>
<template>
  <div
    class="input-field"
    :class="{
      'input-field--passengers': type === 'number',
    }"
  >
    <label class="input-field__label sr-only" :for="id">
      {{ label }}
    </label>
    <img
      v-if="icon"
      class="input-field__icon"
      :src="`/assets/icons/${icon}.svg`"
      :alt="`icon ${label}`"
    />
    <button
      class="passengers-arrows__button"
      :class="{
        'passengers-arrows__button--disabled':
          Number(model) <= 1 || model === null,
      }"
      @click="changePassengers(-1)"
      v-if="type === 'number'"
    >
      <img
        class="passengers-arrows__button__icon"
        src="/assets/icons/remove.svg"
        alt="minus icon"
      />
    </button>
    <input
      v-model="model"
      ref="inputRef"
      :id="id"
      class="input-field__input"
      :type="type"
      :placeholder="placeholder"
      :autocomplete="autocomplete"
      :autofocus="autofocus"
      :aria-label="label"
      :aria-labelledby="label"
      :title="label"
      :aria-placeholder="placeholder"
      :name="name"
      :value="model"
    />

    <span
      v-if="type === 'datetime-local'"
      class="input-field__date"
      @click="triggerDatepicker()"
    >
      <span v-if="!dateAsString">{{ label }}:</span> {{ dateAsString }}
    </span>

    <button
      class="passengers-arrows__button"
      :class="{
        'passengers-arrows__button--disabled':
          Number(model) >= 99 || model === null,
      }"
      @click="changePassengers(1)"
      v-if="type === 'number'"
    >
      <img
        class="passengers-arrows__button__icon"
        src="/assets/icons/add_dark.svg"
        alt="minus icon"
      />
    </button>
  </div>
  <div class="input-error" v-if="error">{{ error }}</div>
</template>
<style lang="scss" scoped>
.input-field {
  display: flex;
  gap: 0.5rem;
  width: 100%;
  align-items: center;
  background-color: $primary-color;
  border-radius: $radius;
  padding: 0 0.75rem;
  box-shadow: $shadow;
  overflow: hidden;

  &--passengers {
    padding: 0;
    gap: 0rem;
    box-shadow: none;
  }

  &__label {
    font-size: $small-text;
    font-weight: $skinny;
    white-space: nowrap;
    width: fit-content;
    margin-left: 0.75rem;
  }

  &__icon {
    width: 1rem;
    height: 1rem;
  }

  input {
    font-size: 1rem;
    padding: 0.65rem 0;
    padding-top: 0.75rem;
    border: none;
    color: $text-color;
    background-color: $primary-color;
    width: 100% !important;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;

    &::placeholder {
      color: $text-color-faded;
      font-size: 1rem;
      font-weight: $skinny;
    }

    &[type="search"] {
      max-width: 100%;
    }
    &[type="datetime-local"] {
      width: 100%;
      max-width: 400px;
      min-width: 300px;
      position: absolute;
      opacity: 0;
    }
    &[type="number"] {
      min-width: 50px;
      max-width: 50px;
      text-align: center;
    }
  }

  &__date {
    width: 100%;
    font-size: 1rem;
    padding: 0.65rem 0;
    font-weight: $skinny;
    color: $text-color;
    caret-color: $text-color;
    cursor: pointer;

    &:focus {
      outline: none;
      border: none;
    }

    span {
      font-size: 1rem;
      font-weight: $skinny;
      color: $text-color-faded;
      white-space: nowrap;
    }
  }
}
.input-error {
  color: $error-color;
  font-size: $small-text;
  font-weight: $skinny;
  display: flex;
  line-height: 1rem;
  border-radius: $radius;
  width: fit-content;
}

.passengers-arrows {
  display: flex;
  gap: 0.5rem;
  flex-direction: column;

  &__button {
    width: 1.5rem;
    height: 100%;
    cursor: pointer;
    background-color: $primary-color;
    box-shadow: $shadow;
    color: $text-color;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0.5rem;
    border-radius: $radius 0 0 $radius;
    border-right: 1px solid $base-color;

    &:nth-of-type(2) {
      border-radius: 0 $radius $radius 0;
      border-left: 1px solid $base-color;
    }

    &--disabled {
      cursor: not-allowed;
      opacity: 0.5;
      pointer-events: none;
    }

    &__icon {
      width: 0.75rem;
      height: 0.75rem;
    }
  }
}
</style>
