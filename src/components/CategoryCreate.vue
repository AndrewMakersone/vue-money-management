<template>
  <div class="col s12 m6">
    <div>
      <div class="page-subtitle">
        <h4>Создать</h4>
      </div>

      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input
              id="name"
              type="text"
              v-model="title"
              :class="{invalid: this.$v.title.$dirty && !this.$v.title.required}"
          >
          <label for="name">Название</label>
          <span
              v-if="this.$v.title.$dirty && !this.$v.title.required"
              class="helper-text invalid"
          >
            Введите название категории
          </span>
        </div>

        <div class="input-field">
          <input
              id="limit"
              type="number"
              v-model.number="limit"
              :class="{invalid: this.$v.limit.$dirty && !this.$v.limit.minValue}"
          >
          <label for="limit">Лимит</label>
          <span
              v-if="this.$v.limit.$dirty && !this.$v.limit.minValue"
              class="helper-text invalid"
          >
            Минимальная значение {{this.$v.limit.$params.minValue.min}}
          </span>
        </div>

        <button class="btn waves-effect waves-light" type="submit">
          Создать
          <i class="material-icons right">send</i>
        </button>
      </form>
    </div>
  </div>
</template>

<script>
  import {required, minValue} from 'vuelidate/lib/validators'
  export default {
    name: "CategoryCreate",
    data: () => ({
      title: '',
      limit: 100
    }),
    validations: {
      title: {required},
      limit: {minValue: minValue(100)}
    },
    methods: {
      async submitHandler() {
        if (this.$v.$invalid) {
          this.$v.$touch();
          return
        }

        try {
          const category = await this.$store.dispatch('createCategory', {
            title: this.title,
            limit: this.limit
          });
          this.$quickmessage(`Категория ${category.title} была создана`);
          this.$emit('created', category);
          this.title = '';
          this.limit = 100;
          this.$v.reset();

        } catch (e) { return e }
      }
    },
    mounted() {
      // eslint-disable-next-line no-undef
      M.updateTextFields();
    }
  }
</script>

<style scoped>

</style>
