<script>
import { ref, computed } from 'vue';

export default {
    emits: ['close', 'add-organization'],
    setup(_, { emit }) {
      const name = ref('');
      const director = ref('');
      const phone = ref('');

      const formValid = computed(() => {
          return name.value && director.value && phone.value;
      });

      const add = () => {
          emit('add-organization', {
              name: name.value,
              director: director.value,
              phone: phone.value,
          });
          name.value = '';
          director.value = '';
          phone.value = '';
      };

      return {
          name,
          director,
          phone,
          formValid,
          add,
        };
    },
};
</script>

<template>
    <div class="modal">
      <div class="modal-content">
        <div class="modal-title">
          <h2>Добавить организацию</h2>
        </div>
        <form @submit.prevent="add">
          <div class="modal_form-input">
            <input v-model="name" placeholder="Название:" required />
            <input v-model="phone" placeholder="Номер телефона:" required />
            <input v-model="director" placeholder="ФИО директора:" required />
          </div>
          <div class="modal_form-button">
            <button type="button" @click="$emit('close')">Отмена</button>
            <button type="submit" :disabled="!formValid">ОК</button>
          </div>
        </form>
      </div>
    </div>
</template>

<style scoped>
/* Стили для модального окна */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.5);
}
.modal-content {
  background: rgb(255, 255, 255);
  color: black;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border: 1px solid black;
}
.modal-title{
  border-bottom: 1px solid black;
  padding: 10px;
}
.modal_form-input{
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 10px;
}
.modal_form-button{
  display: flex;
  justify-content: end;
  gap: 15px;
  border-top: 1px solid black;
  padding: 10px;
}
</style>
  